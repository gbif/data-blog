---
title: Which data can be shared through GBIF and what cannot
author: Cecilie Svenningsen
date: '2022-10-28'
slug: data-shareability
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

Preparing a dataset to be shared on GBIF.org can be a daunting task and many publishers realise that not all their data fit in the [DwC standard](https://www.gbif.org/darwin-core) and [extensions](https://rs.gbif.org/extensions.html) GBIF use to structure, standardise and display biodiversity data. This blog post will cover what types of data that fits in GBIF, give examples of data that does not fit in the current format of GBIF, and provide guidance to how you can share data that is relevant for your dataset in a metadata-only dataset or through a third-party.

## What data can you share on GBIF?

At its core, [GBIF](https://www.gbif.org/what-is-gbif) is a data infrastructure to share, view and retrieve global species occurrence data. All GBIF-mediated data comes from [four](https://www.gbif.org/dataset-classes) different types of [datasets](https://data-blog.gbif.org/post/choose-dataset-type/), published by [endorsed organizastions](https://www.gbif.org/endorsement-guidelines). However, researchers, collections, monitoring professionals, citizen scientists etc. often collect data that extends beyond just species occurrences, which to a large extend can be captured in a [sampling event dataset](https://www.gbif.org/data-quality-requirements-sampling-events). But what should you do if you happen to have data that does not seems to fit in GBIF?

**NOTE!** this blog post is relevant for publishers using GBIFs current model to publish data. It is expected that it we be much easier to share information-rich datasets with the [new data model](https://www.gbif.org/new-data-model) when it is released.

### Data structure vs. data content
There are two main reasons for why data is not shareable on GBIF:
1. The data structure is not supported within the star schema GBIF use to organize how data is related
2. The data has no associated DwC data standard field

This blog post focus on the data content, but if you want to familiarise yourself with the schema and the associated data sharing limitations, you can read more in the [Darwin Core Archive guide](https://ipt.gbif.org/manual/en/ipt/2.5/dwca-guide).

The central idea of an archive is that its data files are logically arranged in a star-like manner, with one core data file surrounded by any number of ‘extension’ data files. Core and extension files contain data records, one per line. Each extension record (or ‘extension file row’) points to a record in the core file; in this way, many extension records can exist for each single core record. This is sometimes referred to as a “star schema”.

## What you can share on GBIF?


## What you cannot share on GBIF
During the years, GBIF has seen some very creative ways to "make the DwC standard fit to other types of data". The creativity tends to arise when researchers have (super cool!) complex, data-rich research projects often involving novel observation methods or scientific collections want to digitize more information from their collections. 

For example, this [parka](https://arctos.database.museum/guid/UAM:EH:UA67-133-0001) is a single observation that consist of fur from multiple animal species, and it is a highly valued cultural and historical object. Each of the  
* observations of humans (although they [occassionally](https://www.gbif.org/occurrence/search?taxon_key=2436436) make their way into citizen science and collections datasets)
* species occurrences in extraction blanks and PCR negatives from DNA-dreived occurrence datasets



## How to use metadata-only dataset to make your work more visible
