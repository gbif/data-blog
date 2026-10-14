---
title: Recent breaking changes to SQL downloads to support the COL XR
author: 'John Waller'
date: '2026-10-14'
slug: sql-downloads-col-xr
categories:
  - GBIF
tags:
  - API
  - SQL
  - taxonomy
  - Catalogue of Life
lastmod: '2026-10-07T10:00:00+02:00'
draft: no
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

GBIF SQL downloads now use the [Catalogue of Life Extended Release (COL XR)](https://www.catalogueoflife.org/building/releases) as the default taxonomy for organizing results. They also use COL XR taxon keys by default. [See the previous post for more information.](https://data-blog.gbif.org/post/catalogue-of-life-taxonomic-backbone/)

If you have existing SQL queries that use numeric GBIF Backbone taxon keys, update them to use alphanumeric COL XR keys or reference the `gbif_classification` field to continue using the GBIF Backbone taxonomy. Because the GBIF Backbone will not be updated, we recommend transitioning your queries and scripts to COL XR.

<!--more-->

## Examples 

Suppose you have an existing query that counts the number of species in each bird family using the old GBIF Backbone taxon key for birds (`classkey = '212'`).


```sql 
-- This query will return zero records 
SELECT
  familykey,
  family,
  COUNT(DISTINCT specieskey) AS species_count
FROM occurrence
WHERE classkey = '212'
  AND familykey IS NOT NULL
  AND specieskey IS NOT NULL
GROUP BY familykey, family
ORDER BY species_count DESC
```

To continue using the backbone taxonomy, you need to qualify each taxonomy field with `occurrence.gbif_classification` as shown below:

```sql 
-- Use this query to continue using the GBIF Backbone taxonomy
SELECT
  occurrence.gbif_classification.familykey AS familykey,
  occurrence.gbif_classification.family AS family,
  COUNT(DISTINCT occurrence.gbif_classification.specieskey) AS species_count
FROM occurrence
WHERE occurrence.gbif_classification.classkey = '212'
  AND occurrence.gbif_classification.familykey IS NOT NULL
  AND occurrence.gbif_classification.specieskey IS NOT NULL
GROUP BY
  occurrence.gbif_classification.familykey,
  occurrence.gbif_classification.family
ORDER BY species_count DESC
```

The query above will work. However, it is probably a better idea to update to use the COL XR taxonomy rather than continuing to rely on the **deprecated** GBIF Backbone.

```sql 
-- Probably a better idea to update to use COL XR taxonomy
SELECT
  familykey,
  family,
  COUNT(DISTINCT specieskey) AS species_count
FROM occurrence
WHERE classkey = 'V2'
  AND familykey IS NOT NULL
  AND specieskey IS NOT NULL
GROUP BY familykey, family
ORDER BY species_count DESC
```

To find corresponding COL XR taxon keys for existing GBIF Backbone taxon keys, use the **rgbif** function [`gbif_to_col()`](https://docs.ropensci.org/rgbif/reference/gbif_to_col.html). Alternatively, use the endpoint below:

```url
https://api.gbif.org/v2/species/match?checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b&scientificNameID=gbif:212
```

Alternatively, copy the final part of the URL after searching for the taxon on [GBIF](https://www.gbif.org/taxon/V2).

```url
https://www.gbif.org/taxon/V2
```

## Using names directly from COL XR

It is also possible to filter by names in COL XR directly instead of using the taxon keys. 

```sql
SELECT
  "order" AS taxonomic_order,
  familykey,
  family,
  COUNT(DISTINCT specieskey) AS species_count
FROM occurrence
WHERE class = 'Aves'
  AND familykey IS NOT NULL
  AND specieskey IS NOT NULL
GROUP BY "order", familykey, family
ORDER BY taxonomic_order, species_count DESC
```

Keep in mind that you should exclude authorship when filtering by `genus` or other higher taxonomic ranks. 

```sql
-- will return 1 row. Since there is 1 species in the genus Puma. Don't use 'Puma Jardine, 1834'. With authorship will return zero rows.  
SELECT
  genus,
  COUNT(DISTINCT specieskey) AS species_count
FROM occurrence
WHERE genus = 'Puma'
  AND taxonrank = 'SPECIES'
  AND specieskey IS NOT NULL
GROUP BY genus
```

Excluding authorship means there might be **homonym issues**, so it is usually a better idea to always use COL taxon keys or use a more complex query like `WHERE class = 'Mammalia' AND genus = 'Puma'`.

## Additional up-coming changes expected for the occurrence table

In the near future, the column `OccurrenceStatus` with values "Present" and "Absent" will be changed to "Detected" and "NotDetected". 

## More information

See the [SQL downloads documentation](https://techdocs.gbif.org/en/data-use/api-sql-downloads#taxonomy-columns) for the complete list of taxonomy columns and examples. 
For broader guidance on migrating from the GBIF Backbone to COL XR, see [How to migrate from the legacy GBIF Backbone Taxonomy to Catalogue of Life Extended Release](/post/catalogue-of-life-taxonomic-backbone/).