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

Preparing a dataset to be shared on GBIF.org can be a daunting task and many publishers realise that not all their data fit in the [DwC standard](https://www.gbif.org/darwin-core) and [extensions](https://rs.gbif.org/extensions.html) GBIF use to package, standardise and display biodiversity data. In this blog post, I will cover what types of data that fits in GBIF, give examples of data that does not fit in the current format of GBIF, and provide guidance to how you can share data that is relevant for your dataset in a metadata-only dataset or through a third-party.

At its core, [GBIF](https://www.gbif.org/what-is-gbif) is a data infrastructure to share, view and retrieve global species occurrence data. All GBIF-mediated data comes from [four](https://www.gbif.org/dataset-classes) different types of [datasets](https://data-blog.gbif.org/post/choose-dataset-type/), published by [endorsed organizastions](https://www.gbif.org/endorsement-guidelines). However, researchers, collections, monitoring professionals, citizen scientists etc. often collect data that extends beyond just species occurrences, which to a large extend can be captured in a [sampling event dataset](https://www.gbif.org/data-quality-requirements-sampling-events). But what should you do if you happen to have data that does not seems to fit in GBIF?

**NOTE!** this blog post is relevant for publishers using GBIFs current model to publish data. It is expected that it we be much easier to share information-rich datasets with the [new data model](https://www.gbif.org/new-data-model) when it is released.


## Data structure vs. data content
Let me start by explaing that there are two main reasons for why data is not shareable on GBIF:
1. The data structure is not supported within the star schema GBIF use to organize how data is related
2. The data has no associated DwC data standard field

### Star schema
When data is published to GBIF, the publisher always has to choose a core corresponding to the dataset type they want to publish. The core corresponds to the main data table, whether that is occurrences (occurrence core), a sampling event (event core) or a checklist of species for a given area (taxon core). The core can then have one layer of associated tables that holds additional data on (some of) the rows in the main table/core.

[figure of correct setup and figure of wrong setup]

## What you can share on GBIF?


## What you cannot share on GBIF
During the years, GBIF has seen some very creative ways to "make the DwC standard fit to other types of data". The creativity tends to arise when researchers have (super cool!) complex, data-rich research projects often involving novel observation methods or scientific collections want to digitize more information from their collections. 

For example, this [parka](https://arctos.database.museum/guid/UAM:EH:UA67-133-0001) is a single observation that consist of fur from multiple animal species, and it is a highly valued cultural and historical object. Each of the  
* observations of humans (although they [occassionally](https://www.gbif.org/occurrence/search?taxon_key=2436436) make their way into citizen science and collections datasets)
* species occurrences in extraction blanks and PCR negatives from DNA-dreived occurrence datasets



## How to use metadata-only dataset to make your work more visible
