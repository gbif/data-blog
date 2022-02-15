---
title: Using apache-arrow with GBIF parquet snapshots
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

As written about in a previous [blog post](https://data-blog.gbif.org/post/aws-and-gbif/), GBIF now has database snapshots of occurrence records on [AWS](https://registry.opendata.aws/gbif/). This allows users to access large tables of GBIF mediated occurrence records from Amazon **s3** remote storage. 

<!--more-->

## Parquet advantages

GBIF saves the snapshots it exports in a [columnar data format](https://en.wikipedia.org/wiki/Column-oriented_DBMS) known as [parquet](https://parquet.apache.org/). This format allows for **certain types** of queries to run **very quickly**. 

With parquet, the values in each column are physically stored in contiguous memory locations. Parquet contains row **group level statistics** that contain the minimum and maximum values for each column chunk. Queries that fetch specific column values need not read the entire row data thus improving performance. Additionally, file sizes for parquet tables are **typically smaller** than the equivalent csv file.

## Run a big query on your laptop with R

> Interfaces to the arrow package are also available in [other languages](https://arrow.apache.org/)

The R package [arrow](https://arrow.apache.org/docs/r/) allows large queries to run locally by only downloading the parts of the dataset necessary to perform the query.

This code will query the GBIF [AWS snapshot](https://registry.opendata.aws/gbif/) in the `gbif-open-data-eu-central-1` region from `2021-11-01`. Look [here](https://gbif-open-data-af-south-1.s3.af-south-1.amazonaws.com/index.html#occurrence/) to find the latest snapshot. 

```r 
# get occurrence counts from all species in Sweden since 1990
library(arrow)
library(dplyr)

gbif_snapshot <- "s3://gbif-open-data-eu-central-1/occurrence/2021-11-01/occurrence.parquet"
df <- open_dataset(gbif_snapshot)

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

Only certain [dplyr verbs](https://arrow.apache.org/docs/r/articles/dataset.html) will work on a arrow dataset objects.  

## Query performance

It is sometimes hard to predict what type of queries will run quickly. I have found that anything that does not aggregate to a count, will tend to run more **slowly**. 

The query below takes longer to run. It returns around [23 records](https://www.gbif.org/occurrence/search?country=BW&has_coordinate=true&has_geospatial_issue=false&taxon_key=5&license=CC0_1_0&license=CC_BY_4_0). 

```r
# runs relatively slowly
df %>% 
  filter(
  countrycode == "BW",
  kingdom == "Fungi"
  ) %>%
  select(species) %>%
  collect()
```

This aggregation query is much faster. 

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

There are a few things that can be done to make arrow queries **run faster**: 

- Have a fast internet connection (>=100 mb/s).
- Try removing **array type** columns first `select(-mediatype,-issue)`.
- Pick an [ASW region](https://registry.opendata.aws/gbif/) near you.
- Download a **local copy**.

It also possible to download a **smaller local subset of data**, which I discuss below. **A local copy will always run faster than a copy on AWS**. 

## Downloading a simple parquet from GBIF

**Simple parquet** downloads are currently an [undocumented feature](https://github.com/gbif/gbif-api/blob/dev/src/main/java/org/gbif/api/model/occurrence/DownloadFormat.java). There is **no promise** that this feature will remain stable or function well.   

Below you can make a simple parquet download using **rgbif**. Set up your GBIF credentials first by following this [short tutorial](https://docs.ropensci.org/rgbif/articles/gbif_credentials.html).

```r
# install.packages("rgbif") # download latest version
library(rgbif)
# all Botswana occurrences
download_key <- occ_download(pred("country", "BW"),format = "SIMPLE_PARQUET") 

occ_download_wait(download_key) # wait for download to finish
occ_download_get(download_key) 
zip::unzip(paste0(download_key,'.zip')) # creates a folder "occurrence.parquet"
# rgbif::occ_download_import() # does not yet work for parquet downloads.
```

Wait a few minutes for the download to finish. **Simple parquet** downloads tend to take up less disk space than the equivalent **simple csv** download. This parquet download of [Botswana](https://www.gbif.org/occurrence/search?country=BW) is unzipped **67MB**, while a [simple-csv](https://www.gbif.org/occurrence/download/0138730-210914110416597) download of Botswana is unzipped **350MB**. 

Apache arrow parquet datasets also allow for **lazy loading**, so only the data after `collect()` is loaded into your r-env memory. 

```r
# This 'slow' query will run very quickly locally
library(arrow)
library(dplyr)

local_df <- open_dataset("occurrence.parquet")

local_df %>% 
  select(-mediatype,-issue) %>% # for query speed
  filter(
  countrycode == "BW",
  kingdom == "Fungi"
  ) %>%
  collect()
```

## gbifdb package

You can also use the new R package [gbifdb](https://github.com/cboettig/gbifdb). The goal of **gbifdb** is to provide a relational database interface to GBIF mediated data. The project is under active development.

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

If you end up using your query from AWS in a research paper, you will want a **DOI**. 

> If you made a **simple parquet** download from GBIF, then you can just **use the DOI associated with that download**. 

GBIF now has a [derived dataset service](https://www.gbif.org/derived-dataset/register) for generating a **citable doi** from **a list of involved datasetkeys with occurrences counts**. See the [GBIF citation guidelines](https://www.gbif.org/citation-guidelines) and [previous blog post](https://data-blog.gbif.org/post/derived-datasets/).

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

You can also now use your citation file with the development version of **rgbif** to create a derived dataset and a citable DOI, although you would need to upload your exported dataset to **Zenodo** (or something similar) first. Set up your GBIF credentials first by following this [short tutorial](https://docs.ropensci.org/rgbif/articles/gbif_credentials.html).

```r
# install.packages("rgbif") # requires latest version of rgbif

library(rgbif)

citation_data <- readr::read_tsv("citation.tsv")

# use derived_dataset_prep() if you just want to test
derived_dataset(
citation_data = citation_data,
title = "Research dataset derived from GBIF snapshot on AWS",
description = "I used AWS and arrow to filter GBIF snapshot 2021-11-01.",
source_url = "https://zenodo.org/fake_upload"
)
```

Registering a derived dataset can also be done using the [web interface](https://www.gbif.org/derived-dataset/register). 
