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

As written about in a previous [blog post](https://data-blog.gbif.org/post/aws-and-gbif/), GBIF now has snapshots of occurrence records on [AWS](https://registry.opendata.aws/gbif/). This allows users to access large tables of GBIF mediated occurrence records from **s3**. 

<!--more-->

## Parquet

GBIF saves the snapshots it exports in a [columnar data format](https://en.wikipedia.org/wiki/Column-oriented_DBMS) known as [parquet](https://parquet.apache.org/). This format allows for **certain types** of queries to run very quickly. 

With **parquet**, the values in each column are physically stored in contiguous memory locations. Parquet contains row group level statistics that contain the minimum and maximum values for each column chunk. Queries that fetch specific column values need not read the entire row data thus improving performance. 

When querying, columnar storage can skip over the non-relevant data quickly. As a result, aggregation queries are less time consuming compared to row-oriented databases. This [article](https://blog.cloudera.com/speeding-up-select-queries-with-parquet-page-indexes/) gives a good introduction to the format.

## Run a big query on your laptop

The R package [arrow](https://arrow.apache.org/docs/r/) allows some queries to run somewhat quickly **without downloading a large dataset** to your local computer. This code will query the GBIF [AWS snapshot](https://registry.opendata.aws/gbif/) in the `gbif-open-data-eu-central-1` region from `2021-11-01`. Look [here](https://gbif-open-data-af-south-1.s3.af-south-1.amazonaws.com/index.html#occurrence/) to find the latest snapshot. 

```r 
# get occurrence counts from all species in Sweden since 1990
# runs in about 5 minutes
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

It is often hard to predict what type of queries will run quickly and which will run **slowly** if at all. I have found that anything that does not aggregate to a count, will run **slowly** or **not at all**. 

The query below takes around **>30 min** to run. It returns around [23 records](https://www.gbif.org/occurrence/search?country=BW&has_coordinate=true&has_geospatial_issue=false&taxon_key=5&license=CC0_1_0&license=CC_BY_4_0). 

```r
# runs slowly
df %>% 
  filter(
  countrycode == "BW",
  kingdom == "Fungi"
  ) %>%
  select(species) %>%
  collect()
```

This aggregation query takes around 10 min to finish. 

```r
# runs faster
df %>% 
  filter(
  countrycode == "BW",
  kingdom == "Fungi"
  ) %>%
  group_by(species) %>%
  count() %>% 
  collect()
```


## gbifdb package

You can also use the new R package [gbifdb](https://github.com/cboettig/gbifdb). The goal of **gbifdb** is to provide a relational database interface to GBIF mediated data.

```r 
# duckdb installation 
install.packages("https://github.com/duckdb/duckdb/releases/download/master-builds/duckdb_r_src.tar.gz", repos = NULL)
devtools::install_github("cboettig/gbifdb")
```

```r
library(gbifdb)
library(dplyr)  

gbif <- gbif_remote()

gbif %>%
  filter(phylum == "Chordata", year > 1990) %>%
  count(class, year)
```

## Citation

If you end up using your query in a research paper, you will want a **DOI**. GBIF now has a [derived dataset service](https://www.gbif.org/derived-dataset/register) for generating a **citable doi** from **a list of involved datasetkeys with occurrences counts**. See the [GBIF citation guidelines](https://www.gbif.org/citation-guidelines) and [previous blog post](https://data-blog.gbif.org/post/derived-datasets/).

You can generate a **citation file** from the query above using the following code. 

```r
# generate a citation file 

citation <- df %>% 
  filter(
  countrycode == "BW",
  kingdom == "Fungi"
  ) %>%
  group_by(datasetkey) %>%
  count() %>% 
  collect()
  
readr::write_tsv("citation.tsv")  
```

You can also now use your citation file with the development version of **rgbif** to create a derived dataset and a citable DOI, although you would need to upload your exported dataset to **Zenodo** (or something similar) first. 
```r
# pak::pkg_install("ropensci/rgbif") # requires development version of rgbif

library(rgbif)

citation_data <- readr::read_tsv("citation.tsv")

# use derived_dataset_prep() if you just want to test
derived_dataset(
citation_data = citation_data,
title = "Research dataset derived from GBIF snapshot on AWS",
description = "I used AWS and arrow to filter GBIF snapshot 2021-11-01.",
source_url = "https://zenodo.org/fake_upload",
user="your_gbif_user_name",
pwd="your_gbif_password"
)
```

Registering a derived dataset can also be done using the [web interface](https://www.gbif.org/derived-dataset/register). 
