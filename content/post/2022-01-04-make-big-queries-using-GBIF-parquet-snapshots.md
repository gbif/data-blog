---
title: Make big queries using GBIF parquet snapshots
author: John Waller
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


As written about in previous [blog posts](https://data-blog.gbif.org/post/aws-and-gbif/), GBIF now has database snapshots of occurrence records on [AWS](https://registry.opendata.aws/gbif/) and [Azure](https://github.com/microsoft/AIforEarthDataSets/blob/main/data/gbif.md). This allows users to access large tables of GBIF mediated occurrence records from the cloud. 

<!--more-->

Typically when working with such large datasets, one needs buy cluster time on AWS or Azure. However, there are certain types of queries that do not require large computing resources **even with very large datasets**.  

GBIF saves the snapshots it exports in a [columnar data format](https://en.wikipedia.org/wiki/Column-oriented_DBMS) known as [parquet](https://parquet.apache.org/). This format allows for **certain types** of queries to run very quickly. 

## Run a big query on your laptop

The R package [arrow](https://arrow.apache.org/docs/r/) allows some q to run somewhat quickly without downloading a large dataset to your local computer. 


https://arrow.apache.org/docs/r/articles/dataset.html

With 

https://arrow.apache.org/docs/r/

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





## Queries that do not work

It is often hard to predict what type of queries will run quickly and which will run **slowly**. I have found that 










