---
title: Make big queries using GBIF parquet snapshots
author: John Waller and Carl Boettiger
date: '2022-01-04'
slug: []
categories:
  - GBIF
tags: []
lastmod: '2022-01-04T10:19:14+01:00'
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


As written about in previous [blog posts](https://data-blog.gbif.org/post/aws-and-gbif/), GBIF now has snapshots of occurrence records on [AWS](https://registry.opendata.aws/gbif/) and [Azure](https://github.com/microsoft/AIforEarthDataSets/blob/main/data/gbif.md). This allows users to access large tables of GBIF mediated occurrence records from the cloud. 

<!--more-->

Typically when working with such large datasets, one needs buy cluster time on AWS or Azure. However, there are certain types of queries that do not require large computing resources **even with very large datasets**.  

GBIF saves the snapshots it exports in a [columnar data format](https://en.wikipedia.org/wiki/Column-oriented_DBMS) known as [parquet](https://parquet.apache.org/). This format allows for **certain types** of queries to run very quickly. 

## Run a big query on your laptop

The R package [arrow](https://arrow.apache.org/docs/r/) allows some queries to run somewhat quickly **without downloading a large dataset** to your local computer. This code will query the GBIF [AWS snapshot](https://registry.opendata.aws/gbif/) in the `gbif-open-data-eu-central-1` region from `2021-11-01`. Look here to find the latest [snapshot](https://gbif-open-data-af-south-1.s3.af-south-1.amazonaws.com/index.html#occurrence/). 

```r 
# get occurrence counts from all species in Sweden since 1990
library(arrow)
library(dplyr)

gbif_snapshot <- "s3://gbif-open-data-eu-central-1/occurrence/2021-11-01/occurrence.parquet"
df <- arrow::open_dataset(gbif_snapshot)

df %>% 
filter(
countrycode == "SE",
class == "Mammalia", 
year > 1990
) %>%
group_by(species) %>% 
count() %>%
collect()
```

Only certain [dplyr verbs](https://arrow.apache.org/docs/r/articles/dataset.html)
 will work on a arrow dataset objects. 

## Queries that do not work well

It is often hard to predict what type of queries will run quickly and which will run **slowly** if at all. I have found that anything that does aggregate to a count, will run **slowly** or **not at all**. 

```r
# runs very slowly
df %>% 
filter(
countrycode == "BW",
kingdom == "Fungi"
) %>%
select(species) %>%
collect()
```

It surprising that this query runs **so slowly** because it is around [23 records](https://www.gbif.org/occurrence/search?country=BW&has_coordinate=true&has_geospatial_issue=false&taxon_key=5&license=CC0_1_0&license=CC_BY_4_0).


## gbifdb package

You can also use the new R package [gbifdb](https://github.com/cboettig/gbifdb). 

```r 
install.packages("https://github.com/duckdb/duckdb/releases/download/master-builds/duckdb_r_src.tar.gz", repos = NULL)
devtools::install_github("cboettig/gbifdb")
```

```r
library(gbifdb)
library(dplyr)  
```


```r
gbif <- gbif_remote()

gbif %>%
  filter(phylum == "Chordata", year > 1990) %>%
  count(class, year)
```

