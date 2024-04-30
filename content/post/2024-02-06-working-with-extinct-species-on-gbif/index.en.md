---
title: Working with extinct species on GBIF
author: John Waller
date: '2024-02-06'
slug: []
categories:
  - GBIF
tags: []
lastmod: '2024-02-06T10:24:15+01:00'
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

GBIF users are often interested in getting occurrences of only living species or "no extinct" species. This seemingly straightforward request is often difficult to fulfill in practice. 

Currently, there is no occurrence level filter for removing extinct species from an occurrence search. Additionally, there is no authoritative global extinct species list available from GBIF. This article discusses the difficulties and complexities of working with extinct species and some possible solutions. 

<!--more-->

## isExtinct 

GBIF has a **species search** term called `isExtinct`. This term is (as of the writing of this blog post) is not visible on the [web UI](https://www.gbif.org/species/search), but it is available when using the [API](https://api.gbif.org/v1/species/search?rank=SPECIES&isExtinct=true
) or [rgbif](https://docs.ropensci.org/rgbif/reference/name_lookup.html
)/[pygbif](https://pygbif.readthedocs.io/en/latest/modules/species.html#pygbif.species.name_lookup
). 

This search term as the name suggests will tell you whether a taxon was marked as "extinct" by the source checklist of the name. The `isExtinct` designation on the GBIF API  only harvested from the Catalogue Of Life (COL) [source datasets](https://www.catalogueoflife.org/data/source-datasets). 

```r
name_lookup(isExtinct=TRUE)
```

```python
species.name_lookup(isExtinct=True)
```

```shell
https://api.gbif.org/v1/species/search?rank=SPECIES&isExtinct=true
```

Some checklist sources in the COL do not publish any extinct taxa, meaning either that there are no extinct species within the group or the checklist simply doesn't publish that information. Nevertheless, species within the a group of interest might still appear as extinct on GBIF, if it appears in another COL dataset, namely [The Paleobiology Database](https://www.catalogueoflife.org/data/dataset/268676). For example, the COL dataset of dragonflies [The World Odonata List](https://www.catalogueoflife.org/data/dataset/125101) does not publish any [extinct dragonflies](https://api.gbif.org/v1/species/4822633/speciesProfiles), but extinct dragonflies will still appear as extinct on GBIF, when they appear in **PaleoBioDB** or some other COL dataset publishing extinct taxa.  

![](https://mediaphoto.mnhn.fr/media/1497436549184Bgz5CEiWRWmvHVxy)

Around 35K species-ranked names are labeled as `isExtinct` in the [GBIF backbone](https://api.gbif.org/v1/species/search?rank=SPECIES&isExtinct=true&limit=1000&datasetKey=d7dddbf4-2cf0-4f39-9b2a-bb099caae36c). As of the publishing of this blog post, there is no efficient way to extract all 35K `isExtinct` species using the GBIF species API. The current limit for any species search is `limit=1000`. Therefore, one can only extract the first 1000 records from any group. 

However, since all `isExtinct` taxa are sourced from the COL, one way to get all `isExtinct` taxa in GBIF is to download and process the [COL checklist on GBIF](https://www.gbif.org/dataset/7ddf754f-d193-4cc9-b351-99906754a03b). 

Here is some R code where you can extract all of the `isExtinct` species from the COL checklist dataset. 

```r 
# download and process COL dataset isExtinct
# https://download.catalogueoflife.org/col/latest_dwca.zip
library(dplyr)
current_dir <- getwd()
temp_dir <- tempdir()
setwd(temp_dir)

file_url <- "https://download.catalogueoflife.org/col/latest_dwca.zip"
dest_file <- file.path(temp_dir, "latest_dwca.zip")
download.file(file_url, destfile = dest_file, mode = "wb")
unzip(dest_file, exdir = temp_dir)

species_profile <- readr::read_tsv("SpeciesProfile.tsv") |> 
filter(`gbif:isExtinct` == TRUE)

taxon <- readr::read_tsv("Taxon.tsv")

setwd(current_dir)
merge(species_profile,taxon,by="dwc:taxonID",all.x=TRUE) |> 
readr::write_tsv("isExtinct.tsv")
```
 Alternatively, one can also extract all of the `isExtinct` names from the COL checklist dataset, using the  Checklistbank API. 

```shell 
# export all extinct names 
http://api.checklistbank.org/dataset/3LR/export.txt?extinct=true
# export from a certain group 
## Trilobites taxonID=TRL
http://api.checklistbank.org/dataset/COL2022/export.txt?taxonID=TRL&extinct=true
```

You can look up other COL `taxonID` values on the [COL website](https://www.catalogueoflife.org/), just copy the last part of the URL after searching for the group you want. 

* Odonata (B6LCL) https://www.catalogueoflife.org/data/taxon/B6LCL
* Trilobites (TRL) https://www.catalogueoflife.org/data/taxon/TRL
* Birds (V2) https://www.catalogueoflife.org/data/taxon/V2

When using the `isExtinct` term, one should keep in mind that the coverage and reliability of this term can vary between taxonomic groups and COL source datasets. 

[checklistbank API docs](https://github.com/CatalogueOfLife/backend/blob/master/API.md)

## IUCN Threat Status

The term `isExtinct` is not always reliable for more **recently extinct** species. For more recently extinct species, `iucnRedListCategory` tends to work better. For example, no adult [Golden Toads](https://www.gbif.org/species/2422444) have been seen since May 15, 1989. While GBIF lists `isExtinct=false` for the Golden Toad, the `iucnRedListCategory` lists the Golden Toad correctly as ["EXTINCT"](https://api.gbif.org/v1/species/2422444/iucnRedListCategory). 

Here are some examples of how to use `iucnRedListCategory` to search for extinct species and occurrences with **rgbif** and the API: 

```r
occ_search(taxonKey=2422444,iucnRedListCategory="EX")
name_usage(2422444,data="iucnRedListCategory")
# occ_download(pred(iucnRedListCategory,"EX"))
```

```shell 
https://api.gbif.org/v1/species/2422444/iucnRedListCategory
```

The `iucnRedListCategory` is also available as a web UI search : 
https://www.gbif.org/occurrence/search?iucn_red_list_category=EX

## fossil occurrences 

Finally, one might want to consider occurrences marked with the basis of record "FOSSIL SPECIMENS". While fossil specimens may not always be extinct taxa, it is often useful for filtering out or cross-referencing potentially extinct occurrences. 

```r
# rgbif has a useful default function for removing fossil occurrences and other
# often filtered data, such as living specimens. 
occ_download(pred_default())
occ_download(pred_not(pred("basisOfRecord","FOSSIL_SPECIMEN")))
```

```python 
from pygbif import occurrences as occ
occ.download('basisOfRecord !in ["FOSSIL_SPECIMEN"]')
```

Working with extinct taxa can be challenging, but hopefully you have found some solutions here. As always, if you haven't found what you are looking for, you can always contact us at helpdesk@gbif.org. 

