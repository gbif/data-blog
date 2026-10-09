---
title: Extending the Parquet data snapshots
author: Matthew Blissett
date: '2026-09-29'
slug: extending-parquet-snapshots
categories:
  - GBIF
tags: []
lastmod: '2026-09-29T10:00:00+02:00'
draft: yes
keywords: []
description: ''
authors: ''
comment: no
toc: no
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: yes
contentCopyright: no
reward: no
mathjax: no
mathjaxEnableSingleDollar: no
mathjaxEnableAutoNumber: no
hideHeaderAndFooter: no
flowchartDiagrams:
  enable: no
  options: ''
sequenceDiagrams:
  enable: no
  options: ''
---

In April 2021, GBIF began exporting [monthly data snapshots](https://www.gbif.org/occurrence-snapshots) to the Microsoft Planetary Computer (Azure), as an Amazon AWS Open Dataset and as a Google Public Dataset.  These exports contain similar data columns as a [“Simple” format download](https://techdocs.gbif.org/en/data-use/download-formats#simple), but use the Parquet format which is better suited for use with cloud or big data tools.

Since then, the Parquet format has gained support for geographic data types and tooling has improved to support partitioning, better compression and indexing features.  These allow for faster querying, potentially directly within websites without needing web services.  Tools such as ArcGIS and QGIS are also introducing support for working with data in Parquet format, and GBIF users have requested additional fields be added to the download format and cloud snapshots.

**We are therefore seeking feedback for an expanded Parquet download format containing similar data columns to a Darwin Core Archive format download.  Previews of three possible structures are available and described here.**

## Quick example

[DuckDB](https://duckdb.org) is a command-line database tool with built-in support for querying Parquet files on cloud data systems.

[Install DuckDB](https://duckdb.org/install/), run it, and search for spider occurrences with CC0-licenced media – a query that is not possible through the GBIF API or website.

```
INSTALL parquet,
LOAD parquet;
INSTALL httpfs;
LOAD httpfs;

WITH spiders AS (
  SELECT
    gbifid,
    datasetKey,
    scientificName,
    unnest(multimedia) AS multimedia
  FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/p_taxon/*/*', hive_partitioning = true)
  WHERE taxonPartition LIKE '%Arachnida%'
    AND classKey = 'CCQKT'
)
SELECT
  gbifid,
  datasetKey,
  scientificName,
  'https://api.gbif.org/v1/image/cache/occurrence/' || gbifid || '/media/' || MD5(multimedia.identifier) AS apiUrl
FROM spiders
WHERE multimedia.license LIKE 'http://creativecommons.org/publicdomain/zero%'
LIMIT 1000;
```

## Overview of the data tables

Three tables are provided, containing the same data but using different structure and partitioning.  The first table (`p_taxon`) has taxon partitioning and includes verbatim and multimedia data as additional columns.  The second (`p_taxon_a5`) and third (`p_taxon_h3`) tables have a combined taxonomic and geographic partitioning, with verbatim and multimedia data as separate tables.

In each case there are five significant changes compared to the current Parquet exports.

### 1. Almost all columns are included

Rather than the "simple" view, these exports contain the same columns as a Darwin Core Archive download from GBIF.

### 2. Coordinate column and precalculated grids

A column `coordinates` contains the interpreted coordinates (the `decimalLatitude` and `decimalLongitude` values).

There are also additional columns for [A5](https://a5geo.org) or [H3](https://h3geo.org) discrete global grids (DGGSs).

This allows geographic functions such as `ST_Within` (see below) to be used directly, as well as faster generation of maps and some general statistical analysis, and use in GIS software.

### 3. Taxonomy and grid partitions

The sample tables use Hive partitioning to split the data into files of very roughly equal chunks.  Taxonomy partitioning is at different ranks, since some bird families contain more occurrences than other entire kingdoms.

The `p_taxon` table has 194 taxon partitions with names like `Animalia_Arthropoda_Arachnida`.  Use `SELECT DISTINCT taxonPartition FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/p_taxon/*/*', hive_partitioning = true) ORDER BY taxonPartition;` to see them.

Note some groups like *Lepidoptera* are partitioned into one or more lower rank groups (`Animalia_Arthropoda_Insecta_Lepidoptera_Geometridae` etc), with another partition containing all other *Lepidoptera* (`Animalia_Arthropoda_Insecta_Lepidoptera__PARTIAL`).

Specifying the appropriate partition will save a lot of time when querying. For example, to query for foxes ([taxonKey 87C5](https://www.gbif.org/taxon/87C5)) use `taxonPartition = 'Animalia_Chordata_Mammalia__PARTIAL' AND genusKey = '87C5'`.  The database can then completely ignore hundreds of files containing birds, insects, plants etc.

The `p_taxon_a5` and `p_taxon_h3` tables have a smaller number of taxon partitions (`SELECT DISTINCT taxonPartition FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_a5/*/*', hive_partitioning = true) ORDER BY taxonPartition;`.)

These geo-partitioned tables are also partitioned on either A5 (resolution 2) or H3 (resolution 0) cells.  For geographic queries, calculate the complete coverage in cells for your query and add this to the WHERE clause. For example, to query for occurrences in the polygon `'POLYGON ((-9.9 49.3, 2.8 49.3, 2.8 59.6, -9.9 59.6, -9.9 49.3))'`:

```
INSTALL a5;
LOAD a5;
INSTALL spatial;
LOAD spatial;

SELECT
  COUNT(*) AS count,
  countryCode
FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_a5/*/*/*', hive_partitioning = true, hive_types = {'a5_r2': UBIGINT})
WHERE a5_r2 IN a5_geometry_to_cells('POLYGON ((-9.9 49.3, 2.8 49.3, 2.8 59.6, -9.9 59.6, -9.9 49.3))'::GEOMETRY, 2)
AND ST_Within(coordinates, 'POLYGON ((-9.9 49.3, 2.8 49.3, 2.8 59.6, -9.9 59.6, -9.9 49.3))'::GEOMETRY)
GROUP BY countryCode
ORDER BY count DESC
LIMIT 5;
```

This should give a similar result to [the same polygon on www.GBIF.org](https://www.gbif.org/occurrence/search?geometry=POLYGON%28%28-9.9+49.3%2C2.8+49.3%2C2.8+59.6%2C-9.9+59.6%2C-9.9+49.3%29%29&view=dashboard&layout=country.v-TABLE) (counts have increased since the snapshot was taken).

### 4. Inclusion of Verbatim and Multimedia tables

In `p_taxon`, verbatim data (as provided to GBIF by the publisher) is included as a `struct` named `verbatim`, e.g. `verbatim.eventDate` to see the original value for this column.  Interpreted multimedia data is included in a struct array named `multimedia`.  (See the [quick example above](#quick-example).)

In `p_taxon_a5` and `p_taxon_h3`, verbatim and multimedia data are stored as separate tables, partitioned into `gbifid // 1000000` chunks (the record with `gbifid = 12345678` will be stored in the partition with `gbifid_div1000000 = 12000000`).  Access them within DuckDB like this:

```
SELECT occ.*, ver.*, mul.*
  FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_a5/*/*/*', hive_partitioning = true, hive_types = {'a5_r2': UBIGINT}) occ
  INNER JOIN read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/verbatim/p_gbifid1000000/*/*', hive_partitioning = true, hive_types = {'gbifid_div1000000': UBIGINT}) ver ON (occ.gbifid//1000000) = ver.gbifid_div1000000 AND occ.gbifid = ver.gbifid
  LEFT JOIN read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/multimedia/p_gbifid1000000/*/*', hive_partitioning = true, hive_types = {'gbifid_div1000000': UBIGINT}) mul ON (occ.gbifid//1000000) = mul.gbifid_div1000000 AND occ.gbifid = mul.gbifid
  WHERE taxonPartition LIKE '%Chordata' AND genusKey = '87C5'
  LIMIT 10;
```

### 5. Improved compression

Zstd compression is used.  The Parquet files are significantly smaller than with the previous Snappy compression.  Tools supporting Parquet should handle this change automatically.

## Additional examples

### 1. Using a Python notebook to query and analyse occurrence data

This Python notebook queries for the verbatim (published) coordinates of records in Brazil and displays them on a map.  Note the mirror image copies of the country with negated or transposed coordinates.

<details>
    <summary style="font-style: italic">Click to expand and show setup</summary>

```python
import duckdb
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Initialize DuckDB and load HTTPFS extension for S3 access
con = duckdb.connect()
con.execute("INSTALL httpfs;")
con.execute("LOAD httpfs;")

# Configure S3 for public bucket access (GBIF public data [development])
con.execute("SET s3_region='us-east-1';")
con.execute("SET s3_url_style='path';")

print("DuckDB initialized.")
```

    DuckDB initialized.

</details>

```python
query_heatmap = """
SELECT
  CASE WHEN array_contains(issue, 'PRESUMED_NEGATED_LONGITUDE') THEN FLOOR(-decimalLongitude)
       WHEN array_contains(issue, 'PRESUMED_SWAPPED_COORDINATE') THEN FLOOR(decimalLatitude)
       ELSE FLOOR(decimalLongitude)
  END AS lon_bin,
  CASE WHEN array_contains(issue, 'PRESUMED_NEGATED_LATITUDE') THEN FLOOR(-decimalLatitude)
       WHEN array_contains(issue, 'PRESUMED_SWAPPED_COORDINATE') THEN FLOOR(decimalLongitude)
       ELSE FLOOR(decimalLatitude)
  END AS lat_bin,
  COUNT(*) AS record_count
FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_a5/*/*') occ
WHERE occ.countryCode = 'BR'
  AND occ.decimalLatitude IS NOT NULL
  AND occ.decimalLongitude IS NOT NULL
GROUP BY ALL
"""

print("Executing spatial aggregation query...")
df_grid = con.execute(query_heatmap).df()
print(f"Aggregated into {len(df_grid)} 1° grid cells.")
```

<details>
    <summary style="font-style: italic">Click to expand and show figure generation</summary>

```python
import urllib.request
import io
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import LogNorm

# 1. Download GBIF WGS84 background tiles
url_north = "https://tile.gbif.org/4326/omt/0/0/0@1x.png?style=gbif-classic"
url_south = "https://tile.gbif.org/4326/omt/0/1/0@1x.png?style=gbif-classic"

with urllib.request.urlopen(urllib.request.Request(url_north)) as response:
    img_west = plt.imread(io.BytesIO(response.read()))
with urllib.request.urlopen(urllib.request.Request(url_south)) as response:
    img_east = plt.imread(io.BytesIO(response.read()))

# 2. Prepare the raster data
# Ensure bins are integers for numerical sorting
df_grid['lat_bin'] = df_grid['lat_bin'].astype(int)
df_grid['lon_bin'] = df_grid['lon_bin'].astype(int)

raster_df = df_grid.pivot_table(index='lat_bin', columns='lon_bin', values='record_count', fill_value=0, dropna=False)

raster_df = raster_df.reindex(columns = range(-179, 180))
raster_df = raster_df.reindex(range(-179, 179), axis=1)

# Set the 'bad' color (for masked/zero values) to transparent so the map shows through
cmap = plt.get_cmap('YlOrRd').copy()
cmap.set_bad(color='none')  # Transparent

for x in range(-179, 179):
    raster_df.at[x, 90] = 0
    raster_df.at[-179, x] = 0

# Sort index ascending so lowest latitude is at the bottom (for origin='lower')
raster_df_sorted = raster_df.sort_index(ascending=True)

max_val = float(raster_df.max().max())

# 3. Plotting
plt.figure(figsize=(10, 10))

# Plot background tiles
plt.imshow(img_west, extent=[-180, 0, -90, 90], origin='upper', zorder=0)
plt.imshow(img_east, extent=[-0, 180, -90, 90], origin='upper', zorder=0)

# Plot heatmap
im = plt.imshow(
    raster_df_sorted,
    extent=[-180, 180, -180, 180],
    origin='lower',
    cmap=cmap,
    aspect='equal',
    interpolation='nearest', # Preserves the distinct 1° pixel blocks
    norm=LogNorm(vmin=1, vmax=max(2.0, max_val)), # Prevents LogNorm error if max_val <= 1
    zorder=1 # Ensure heatmap is drawn above the map tiles
)

cbar = plt.colorbar(im, label='Count of Records (Logarithmic Scale)', shrink=0.8)

plt.title('1° Resolution Raster Heat Map of GBIF Coordinate Fixes in Brazil\n(Logarithmic Record Count)',
          fontsize=16, fontweight='bold')
plt.xlabel('Longitude (°)', fontsize=12)
plt.ylabel('Latitude (°)', fontsize=12)

# Add a subtle grid to emphasize the 1° resolution
plt.xticks(np.arange(-180, 181, 20))
plt.yticks(np.arange(-180, 181, 20))
plt.grid(True, color='white', linestyle='-', linewidth=0.5, alpha=0.7)

plt.tight_layout()
plt.show()

```

</details>

![Heat map of GBIF coordinate fixes in Brazil](/post/2026-10-02-extending-parquet-snapshots/coordinate-fixes-brazil.png)

### 2. Create a small dashboard for a country

**Parquet files can be used to create an interactive dashboard, without any dependency on the live GBIF APIs.**  Some queries may be able to use the cloud files directly, but for better performance we can create a Parquet data cube.

The Parquet data cube is created using DuckDB.  This stores counts of occurrences, species and the highest event date within different A5 cells for each kingdom and basis of record.

```
COPY (
  SELECT
    a5_lonlat_to_cell(decimalLongitude, decimalLatitude, 6) AS a5_r6,
    a5_lonlat_to_cell(decimalLongitude, decimalLatitude, 7) AS a5_r7,
    a5_lonlat_to_cell(decimalLongitude, decimalLatitude, 8) AS a5_r8,
    kingdomKey,
    basisOfRecord,
    MAX("year") AS max_year,
    COUNT(*) AS occurrence_count,
    COUNT(DISTINCT speciesKey) AS species_count
  FROM read_parquet('s3://gbif-public-data/development/2026-08-01-DWCA/p_taxon/*/*', hive_partitioning = true)
  WHERE
    countryCode = 'PT'
    AND NOT hasGeospatialIssues
  GROUP BY CUBE (a5.r6, a5.r7, a5.r8, kingdomKey, basisOfRecord)
) TO 'portugal-cube-a5.parquet' (FORMAT 'PARQUET', COMPRESSION 'ZSTD', COMPRESSION_LEVEL 8);
```

The query takes about 4 minutes to run, and the result is a 1 MB file.  This could be automated with a cronjob, GitHub action or similar to keep the dashboard up-to-date.

An LLM-generated dashboard exposes the cube on a map, with all queries running in the user's browser.  This could be added to any static site, without any need for APIs or web services.

![Example dashboard for Portugal](/post/2026-10-02-extending-parquet-snapshots/portugal-cube.png)

[View the dashboard](https://labs.gbif.org/~mblissett/2026/10/dashboard-example.html)

You can also [view and query the cube](https://www.parquet-viewer.com/online-parquet-viewer#v=1&url=https%3A%2F%2Flabs.gbif.org%2F%7Emblissett%2F2026%2F10%2Fportugal-cube-a5.parquet) using one of several online Parquet file viewers.

## Summary

Three snapshots are provided.  During development, they are available on an Amazon S3 bucket.  They are both derived from the [1 August 2026 DWCA snapshot <https://doi.org/10.15468/dl.8yrbe7>](https://doi.org/10.15468/dl.8yrbe7), and should be cited with that DOI if used in a publication.

**The first export is partitioned by taxonomic groups** at different levels, stored in a `taxonPartition` (virtual) column.  Within each partition, data are ordered by taxonomy.

**The second export is partitioned by A5 almost-equal-area pentagonal grid cells** at R2 level.  Data are ordered by coordinate (Hilbert curve).

**The third export uses the more widely supported H3 grid cells** at R0 level.  Data are ordered by coordinate (Hilbert curve).

The second and third exports can be joined to separate verbatim and multimedia tables.  Data in these tables is ordered by `gbifid`.

Paths for all of these:

```
s3://gbif-public-data/development/2026-08-01-DWCA/p_taxon/*/*

s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_a5/*/*/*

s3://gbif-public-data/development/2026-08-01-DWCA/occurrence/p_taxon_h3/*/*/*

s3://gbif-public-data/development/2026-08-01-DWCA/verbatim/p_gbifid1000000/*/*

s3://gbif-public-data/development/2026-08-01-DWCA/multimedia/p_gbifid1000000/*/*
```

Column names are the same as for GBIF downloads (generally Darwin Core term short names), with the addition of the partitioning columns, a `coordinates` column and columns for A5 and H3 cell identifiers.  Array, numeric and timestamp data types have been used where appropriate.

## Feedback

Feedback on this data format is appreciated. In particular,

1. Does the partitioning suit the queries you would make, or would you suggest a different partitioning?

2. Are the DGGS (A5 and H3) columns useful, and do you have a preference for one or the other?

3. Should the verbatim Darwin Core extensions supported by GBIF be added?

4. Is the increased size of the combined `p_taxon` table worth not needing to join to other tables for verbatim and multimedia data?
