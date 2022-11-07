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

Preparing a dataset to be shared on GBIF.org can be a daunting task and many publishers realise that not all their data fit in the [DwC standard](https://www.gbif.org/darwin-core) and [extensions](https://rs.gbif.org/extensions.html) GBIF use to structure, standardise and display biodiversity data. 

This blog post will cover what types of data that fits in GBIF, give examples of data that does not fit in the current format of GBIF, and provide guidance to how you can share data that is relevant for your dataset in a metadata-only dataset or through a third-party.

## What data can you share on GBIF?
Let us start with what type of data that actually fits in GBIF. At its core, [GBIF](https://www.gbif.org/what-is-gbif) is a data infrastructure to share, view and retrieve global species occurrence data. All GBIF-mediated data comes from [four](https://www.gbif.org/dataset-classes) different types of [datasets](https://data-blog.gbif.org/post/choose-dataset-type/), published by [endorsed organizastions](https://www.gbif.org/endorsement-guidelines).

![data you can share on GBIF](/post/2022-10-28-which-data-can-be-shared-through-GBIF-and-what-cannot/please-share.png)

✔️**Examples of data you can share in GBIF** (the list is not exhaustive)
* the occurrence of a taxa at a given time and place ([example](https://www.gbif.org/occurrence/3085788310), notice each term is a standardised DwC or extension field)
* DNA-derived occurrences ([example](https://www.gbif.org/occurrence/2238661140), notice that this occurrence is in a [cluster](https://www.gbif.org/occurrence/2238661140/cluster) with other occurrences that  represent the same organism in [this occurrence](https://www.gbif.org/occurrence/3019774852) and the occurrence record for the [preserved specimen](https://www.gbif.org/occurrence/3024253052) from which the DNA was obtained)
* species checklists for a country or area ([example](https://www.gbif.org/dataset/c20a2c3b-5041-4062-9658-85d269480384))
* sampling events (example of [event record](https://www.gbif.org/dataset/aea17af8-5578-4b04-b5d3-7adf0c5a1e60/event/000279e0-f841-4522-a69c-5ca24b7d5bd6) from an [sampling event dataset](https://www.gbif.org/dataset/aea17af8-5578-4b04-b5d3-7adf0c5a1e60), notice both present and absent occurrences were recorded)
* metadata datasets - a dataset that describes the data without necessarily having any associated occurrences. Metadata dataset will be covered a bit more in detail below, since this is a good way to describe data that might not fit well in GBIF ([example](https://www.gbif.org/dataset/03493d8b-d05e-4f4b-9bd8-8b48abe81924), notice that this dataset actually have associated [events](https://www.gbif.org/dataset/03493d8b-d05e-4f4b-9bd8-8b48abe81924/event/br:sibbr:peld:mlrd:al:2008-5-23-11-int) AND [occurrences](https://www.gbif.org/occurrence/search?dataset_key=03493d8b-d05e-4f4b-9bd8-8b48abe81924))
* metadata-**only** datasets ([example](https://www.gbif.org/dataset/4c3f6e6a-3432-4dc7-9a89-6ad3eb3d1209), no occurrences or events, just describing the data)

## What you cannot or should not share on GBIF
Researchers, collections, monitoring professionals, projects etc. often collect data that extends beyond just species occurrences, which to a large extend can be captured in a [sampling event dataset](https://www.gbif.org/data-quality-requirements-sampling-events) or in [extensions](https://rs.gbif.org/extensions.html). But what should you do if you happen to have data that does not seems to fit in GBIF?

### Data structure vs. data content
There are two main reasons for why data is not shareable on GBIF:
1. **Data structure**: the data structure is not supported within the star schema GBIF use to organize how data is related (the publication contains tables that relate to the extension files and not the core file)
2. **Data content**: the data has no associated DwC data standard field or you cannot find a suitable field in any of the extensions

This blog post focus on the data content (reason #2), but if you want to familiarise yourself with the schema and the associated data sharing limitations, you can read more in the [Darwin Core Archive guide](https://ipt.gbif.org/manual/en/ipt/2.5/dwca-guide).

> The central idea of an archive is that its data files are logically arranged in a star-like manner, with one core data file surrounded by any number of > ‘extension’ data files. Core and extension files contain data records, one per line. Each extension record (or ‘extension file row’) points to a record in > the core file; in this way, many extension records can exist for each single core record. This is sometimes referred to as a “star schema”.

GBIF has seen some very creative ways to *"make the DwC standard fit to other types of data"*. The creativity tends to arise when researchers have (super cool!) complex, data-rich research projects often involving novel observation methods or scientific collections want to digitize more information from their collections. 

For example, we had to delete the dataset and the occurrences from this [dataset](https://www.gbif.org/dataset/53efcac4-cd08-4b4d-93e6-cb06c1d37de1) since all the occurrences were in the future. The project modelled probable occurrences to help inform policy makers of likely outbreaks of wheat disease, but GBIF does not support potential occurrences that happen in the future, only observed occurrences.

![what not to share on GBIF](/post/2022-10-28-which-data-can-be-shared-through-GBIF-and-what-cannot/dontshare.png)

:x:**Examples of data that does not fit in GBIF** (the list is not exhaustive)
* observations of humans (although they [occassionally](https://www.gbif.org/occurrence/search?taxon_key=2436436) make their way into citizen science and collections datasets)
* species occurrences in extraction blanks and PCR negatives from DNA-dreived occurrence datasets
* future occurrences (like the example above)
* occurrences of other things than organisms (for example [ecosystems](https://www.gbif.org/dataset/f513fe98-b1c3-45ee-8e14-7f2a5b7890bf), national parks, rocks (although we do support [fossil specimen occurrences](https://www.gbif.org/occurrence/search?basis_of_record=FOSSIL_SPECIMEN)))

## How to use metadata-only dataset to make your work more visible

**NOTE!** if you are in doubt whether your data fits into GBIF, you can always contact helpdesk@gbif.org and ask for guidance.
