---
title: How to migrate from the legacy GBIF Backbone Taxonomy to Catalogue of Life Extended Release 
author: John Waller
date: '2026-06-23'
slug: catalogue-of-life-taxonomic-backbone
categories:
  - GBIF
tags:
  - API
  - taxonomy
  - catalogue-of-life
lastmod: '2026-06-19T10:00:00+02:00'
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

Recently [GBIF.org](https://www.gbif.org/occurrence/search?occurrenceStatus=present) has updated the default taxonomy it uses to organize occurrence records to the [Catalogue of Life Extended Release](https://www.catalogueoflife.org/building/releases) (COL XR). The [GBIF backbone](https://doi.org/10.15468/39omei)
was last updated in 2023, and **will not be updated again**. We encourage users of our APIs to [migrate towards](https://techdocs.gbif.org/en/data-processing/taxonomy-interpretation) using the COL XR, namely by switching the **checklistKey** parameter to `checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b`.

{{< vimeo 1029184370 >}}

<!--more-->

The COL XR is a more up-to-date and comprehensive taxonomy than the GBIF backbone. It includes more species, more ranks, and will be updated [more frequently](https://www.catalogueoflife.org/building/releases). The COL XR is more maintainable and will allow GBIF to address taxonomic issues more quickly and effectively.

## What happens with old links and old taxonKeys?

GBIF.org has already switched to using the COL XR as the default. So now when you search for occurrences on GBIF.org you are seeing them organized against the taxonomy from COL XR instead of the out-of-date GBIF backbone. For example, the GBIF backbone taxonKey for ***Calopteryx splendens* (Harris, 1780)** is **1427067**.

An old occurrence search link without the checklistKey parameter will return **nothing**: 
https://www.gbif.org/occurrence/search?occurrenceStatus=present&taxonKey=1427067

However, if you add the checklistKey for old GBIF backbone you will get the old results:
https://www.gbif.org/occurrence/search?occurrenceStatus=present&taxonKey=1427067&checklistKey=d7dddbf4-2cf0-4f39-9b2a-bb099caae36c

If however, you use the COL XR taxonKey for ***Calopteryx splendens* (Harris, 1780)** which is **Q2M4** you will get the newly organized results:
https://www.gbif.org/occurrence/search?occurrenceStatus=present&taxonKey=Q2M4

If there is no difference between the GBIF backbone and COL XR taxonomies for a given taxon, the results will be the same. 

No checklistKey is needed here because the COL XR is now the default taxonomy on GBIF.org. If you have old links to species pages that use GBIF backbone taxonKeys, these will automatically redirect to corresponding COL XR **taxon** pages. For example, if you have a link that uses an old GBIF backbone taxonKey: 
https://www.gbif.org/species/1427067

It will redirect to the corresponding taxon page using the COL XR taxonKey:
https://www.gbif.org/taxon/Q2M4

## Occurrence record processing 

All occurrence records are now organized against both the GBIF backbone taxonomy and the COL XR. The API response now has a `classifications` block including two taxonomies. You can see this on a simple occurrence API lookup: 
https://api.gbif.org/v1/occurrence/4021395742 

To use the **occurrence search API** you can use the COL identifiers (`623RM`) and the COL XR checklistKey (`7ddf754f-d193-4cc9-b351-99906754a03b`):
https://api.gbif.org/v1/occurrence/search?familykey=623RM&checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b

For occurrence search API, the GBIF backbone taxonomy is still the default, so if you want to use the COL XR taxonomy you will need to add the checklistKey parameter.

## How to do taxon matching 

If you have a list of names, the **species match tool** can be used to match them to the COL XR taxonomy
https://www.gbif.org/tools/species-lookup

To use the species match API to lookup a taxon identifier you can use:
https://api.gbif.org/v2/species/match?scientificName=Felidae&checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b 

If you have an old GBIF backbone integer taxonKey and want to resolve it to the new COL XR identifier, you can use the `scientificNameID` parameter with the matching service:
https://api.gbif.org/v2/species/match?checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b&scientificNameID=gbif:1427067

This will return the corresponding COL XR taxonKey (`Q2M4`) for the old GBIF backbone taxonKey (`1427067`).

A mapping file has also been generated that maps taxon identifiers from the legacy GBIF Backbone Taxonomy taxonKeys (integer) to their corresponding COL XR identifiers (alpha-numeric).
https://download.checklistbank.org/col/gbif/README.html

## GBIF Maps API 

When using the [GBIF Maps API](https://techdocs.gbif.org/en/openapi/v2/maps) with a COL XR taxonKey (`taxonKey=623RM`) the addition of a checklistKey (`checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b`) will return the COL XR map. If omitted the GBIF backbone will be used to maintain backwards compatibility, but you will also need to use the GBIF backbone (integer) taxonKey. 

```html
<img src="https://api.gbif.org/v2/map/occurrence/density/{z}/{x}/{y}@1x.png?taxonKey=623RM&checklistKey=7ddf754f-d193-4cc9-b351-99906754a03b&style=purpleYellow.point" />
```

## Occurrence Downloads 

For the [occurrence download services](https://techdocs.gbif.org/en/data-processing/taxonomy-interpretation#occurrence-download-api) you can declare the taxonomy like this:

```json
{
 "creator": "jwaller",
 "notification_address": [
  "jwaller@gbif.org"
 ],
 "format": "DWCA",
 "predicate": {
  "type": "equals",
  "key": "TAXON_KEY",
  "value": "Q2M4",
  "checklistKey": "7ddf754f-d193-4cc9-b351-99906754a03b"
 },
 "checklistKey": "7ddf754f-d193-4cc9-b351-99906754a03b"
}
```

## rgbif, pygbif, and rcol  

By default, **rgbif** and **pygbif** still use the old GBIF backbone taxonomy, and you will need to add the checklistKey parameter to use the COL XR taxonomy. However, both packages will likely switch to using the COL XR as the default in the near future. 

```r
library(rgbif)

# Species match using COL XR
name_backbone(
  name = "*Calopteryx splendens*",
  checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b"
)
# Q2M4

# Occurrence search using COL XR taxon keys
occ_search(
  taxonKey = "Q2M4",
  checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b",
  limit = 10
)

# Occurrence download using COL XR taxon keys
occ_download(
  pred("taxonKey", "Q2M4", 
       checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b"),
  checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b"   
)
```

```python
from pygbif import species, occurrences

# Species match using COL XR
species.name_backbone(
  scientificName="*Calopteryx splendens*",
  checklistKey="7ddf754f-d193-4cc9-b351-99906754a03b"
)

# Occurrence search using COL XR taxon keys
occurrences.search(
  taxonkey="Q2M4",
  checklistKey="7ddf754f-d193-4cc9-b351-99906754a03b",
  limit=10
)

# Occurrence download using COL taxon keys
occurrences.download(
  "taxonKey = Q2M4",
  checklistKey = "7ddf754f-d193-4cc9-b351-99906754a03b"
)
query = {
    "type": "equals",
    "key": "TAXON_KEY",
    "value": "5WZLF",
    "checklistKey": "7ddf754f-d193-4cc9-b351-99906754a03b"  # Used for filtering
}
occurrences.download(query, checklistKey="7ddf754f-d193-4cc9-b351-99906754a03b")

```

Certain rgbif and pygbif functions will not return COL XR results even with a checklistKey parameter. For example, `name_lookup()`, `name_suggest()`, and `name_usage()` will only return results from the out-of-date GBIF backbone taxonomy, and will not accept a checklistKey parameter. 

A new package [rcol](https://www.catalogueoflife.org/2026/06/20/rcol-r-package) has replacement functions for these, and will return results from the COL XR taxonomy.

```r
library(rcol)

col_search(q = "*Calopteryx splendens*")
col_suggest(q = "*Calopteryx splendens*")
col_usage("Q2M4")
```

## Further reading 

- [GBIF techdocs article on taxonomy interpretation](https://techdocs.gbif.org/en/data-processing/taxonomy-interpretation)
- [Catalogue of Life Extended Release](https://www.catalogueoflife.org/building/releases)

