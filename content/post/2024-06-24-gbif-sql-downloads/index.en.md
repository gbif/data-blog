---
title: GBIF SQL Downloads
author: John Waller
date: '2024-06-24'
slug: []
categories: []
tags: []
lastmod: '2024-06-24T14:36:55+02:00'
draft: yes
keywords: []
description: ''
authors: ''
comment: no
toc: no
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: no
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

> GBIF has an experimental feature that allows users to download data from the GBIF database in SQL format. <https://techdocs.gbif.org/en/data-use/api-sql-downloads>

> This article has a companion article focused on rgbif \[gbif-sql-downloads\]().

> If your download can be formulated using the traditional predicate interface, it is usually going to be faster to use that interface.

The experimental Occurrence SQL Download API allows users to query GBIF occurrences using SQL. In contrast to the [Predicate Download API](https://techdocs.gbif.org/en/data-use/api-sql-downloads), the SQL API allows selection of the columns of interest and generation of summary views of GBIF data.

SQL downloads, like [predicate downloads](https://techdocs.gbif.org/en/data-use/api-downloads), required you to have a GBIF [user account](https://www.gbif.org/user/profile).

<!--more-->

## SQL Download Workflow

The first step is to prepare your query. There is only one table available for querying, the **occurrence** table. You can check if the query is ok using [query validation](https://techdocs.gbif.org/en/data-use/api-sql-downloads#sql-validation). You can also check what fields are available in the `occurrence` table using this endpoint `https://api.gbif.org/v1/occurrence/download/describe/sql`. There are +400 fields available.

``` sql
SELECT datasetKey, countryCode, COUNT(*) FROM occurrence WHERE continent = 'EUROPE' GROUP BY datasetKey, countryCode
```

This query should be included in a json POST request, and saved to a file named, for example, `query.json`.

``` json
{
  "sendNotification": true,
  "notificationAddresses": [
    "userEmail@example.org" 
  ],
  "format": "SQL_TSV_ZIP", 
  "sql": "SELECT datasetKey, countryCode, COUNT(*) FROM occurrence WHERE continent = 'EUROPE' GROUP BY datasetKey, countryCode" 
}
```

The request should then be sent as a POST request to the endpoint `https://api.gbif.org/v2/occurrence/download/request`

``` shell
curl --include --user YOUR_GBIF_USERNAME:YOUR_PASSWORD --header "Content-Type: application/json" --data @query.json https://api.gbif.org/v1/occurrence/download/request
```

The download will appear in your [GBIF user account](https://www.gbif.org/user/download).

## SQL examples - Species Counts

One common query that is difficult to do with the regular download interface is to get a count of species by some other dimensions. This query gets a table with **countries with the most species published to GBIF** without having to download a large table and do the aggregation locally.

``` sql
SELECT publishingcountry, specieskey, COUNT(*) as occurrence_count
FROM occurrence
WHERE publishingcountry IS NOT NULL AND specieskey IS NOT NULL
GROUP BY country, specieskey
ORDER BY occurrence_count DESC;
```

## SQL examples - Time Series







## SQL examples - Grid Functions

Making a global map of unique species counts per grid cell is a common task, but because it requires a spatial join with the chosen spatial grid, it can be difficult to do without working with a large amount occurrence records.

For this reason GBIF's SQL downloads provide support for a few pre-defined [grid functions](https://techdocs.gbif.org/en/data-use/api-sql-download-functions). These functions will return a **grid cell code** for each occurrence, which can then be used to aggregate or plot the data.

-   **EEA Reference Grid**, GBIF_EEARGCode
-   **Military Grid Reference System**, GBIF_MGRSCode
-   **Quarter degree cell code**, GBIF_QDGCCode

Below is example of working with the **Military Grid Reference System** (MGRS) grid. This example will return a table with unique species counts per grid cell and an attached grid code, which in this case is `mgrs`.

``` sql
SELECT 
  GBIF_MGRSCode(
    100000, 
    decimalLatitude,
    decimalLongitude,
    COALESCE(coordinateUncertaintyInMeters, 0) 
  ) AS mgrs,
  COUNT(DISTINCT speciesKey) AS unique_species_count
FROM
  occurrence
GROUP BY
  mgrs
```

The grid code can then be used to join with a shapefile or geojson file that contains the grid cells.

![](https://private-user-images.githubusercontent.com/4245213/328404302-d56ebcfd-9057-4e7a-a3fc-b0490c59f333.jpg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTkyMzY1NjcsIm5iZiI6MTcxOTIzNjI2NywicGF0aCI6Ii80MjQ1MjEzLzMyODQwNDMwMi1kNTZlYmNmZC05MDU3LTRlN2EtYTNmYy1iMDQ5MGM1OWYzMzMuanBnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDYyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA2MjRUMTMzNzQ3WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OGFhNWQwMzQ4MzY0ZWE4Mjk4NDVkNzI2YTBiNzhlY2RlZjQ1ZjQyMDlhZDhkZTA2ZGNhNDcyNmU2NjU1YTI5NyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.Z4sD8Sho8Id5sa8vUWMhm9KhjIn_u04Njj_pGf3u4rg)

> You can get the code used to generate the figure from the [rgbif companion article]().

This image uses shapefiles from [this repository](https://github.com/klaukh/MGRS).

The sql grid functions were originally designed to be used with creating [species occurrence cubes](https://b-cubed.eu/data-and-evidence). Therefore a randomization parameter was supported `COALESCE(coordinateUncertaintyInMeters, 0)`. This should be set to 0 if you want to use the grid functions with **no randomization**.

``` sql
SELECT 
  GBIF_EEARGCode(
    10000, 
    decimalLatitude,
    decimalLongitude,
    COALESCE(coordinateUncertaintyInMeters, 0) 
  ) AS cellcode,
  COUNT(DISTINCT speciesKey) AS unique_species_count
FROM
  occurrence
GROUP BY
  cellcode
```

In general the hardest part of generating images with GBIF's sql grid functions is finding the corresponding shapefile or geojson file that matches the grid function used. The EEA reference grid example can be found here.

<https://sdi.eea.europa.eu/data/93315b78-089d-43a5-ac76-b3df627b2e4cf>

![](images/plot.jpg)

## Supported SQL

Only `SELECT` queries are supported, and only queries against a single table named `occurrence`. `JOIN` queries and sub-queries are not allowed. Selecting `*` is also not allowed. One must explicitly select the columns needed.

`GROUP BY` queries are supported, as are basic SQL window functions (`OVER` and `PARTITION BY`). `ORDER BY` is supported.

Most common SQL operators and functions are supported, such as `AND`, `OR`, `NOT`, `IS NULL`, `RAND()`, `ROUND(…)`, `LOWER(…)`, etc. Case is ignored by the GBIF SQL parser, and all column names are returned as lowercase.

## When not to use

If you need only commonly used occurrence columns and simple filters, most of the time you can use the regular predicate download interface instead of the SQL interface, and it will likely be faster.

Keep in mind that if you only need species counts for one dimension, then [facet queries](https://api.gbif.org/v1/occurrence/search?facet=specieskey) are usually going to be a much faster option. Some examples below:

```         
http://api.gbif.org/v1/occurrence/search?facet=speciesKey&country=US
http://api.gbif.org/v1/occurrence/search?facet=speciesKey&country=US&year=1800,1900
http://api.gbif.org/v1/occurrence/search?facet=speciesKey&country=US&year=1800,1900&basisOfRecord=HUMAN_OBSERVATION
http://api.gbif.org/v1/occurrence/search?facet=country&facetLimit=200
```

## Further Reading
