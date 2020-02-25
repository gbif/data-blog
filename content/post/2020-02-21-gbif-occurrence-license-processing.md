---
title: GBIF occurrence license processing
author: John Waller
date: '2020-02-21'
slug: gbif-occurrence-license-processing
categories:
  - GBIF
tags: []
lastmod: '2020-02-21T09:37:10+01:00'
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

**GBIF** will now process occurrence licenses record-by-record. 


<center><h3>**iNaturalist research-grade observations**</h3> </center> 

![](/post/2020-02-21-gbif-occurrence-license-processing_files/problem-fix.jpg)

Previously all **occurrence licenses** defaulted to their **dataset license** (provided by the publisher). 

<!--more-->

> In 2014, following a community-wide consultation, the GBIF Governing Board established a general policy to "ensure that all species occurrence datasets within the network are associated with digital licenses equivalent to one of…three choices supplied by Creative Commons" (...) "As of August 2016, all open-access species occurrence datasets on GBIF.org carry standardized, machine-readable licences. These changes improve transparency and reproducibility and support GBIF's mission to promote free and open access to biodiversity data." -- from [Licensing Milestone For Data Access In GBIF](https://www.gbif.org/news/82812/licensing-milestone-for-data-access-in-gbiforg) See also [legal license consultation in 2017](https://www.gbif.org/document/6Z903VDErucikyqAuQSYwc/summary-of-consultation-responses-licensing-of-data-within-gbif)

The dataset level licenses were introduced because record level licenses were unstandardized, often not interpretable or misapplied, so that handling this at dataset level was the only manageable way forward.

But this did not work well for datasets made up of records contributed by thousands of individuals like **iNaturalist**. [discussion here](https://forum.inaturalist.org/t/inaturalist-data-on-gbif-shows-only-cc-by-nc-excluding-cc0-and-cc-by/9952/30)

![](/post/2020-02-21-gbif-occurrence-license-processing_files/iNaturalistScreenShot.jpg)

## Types of licenses GBIF accepts

GBIF still accepts 3 types of **creative commons** licenses for an occurrence record: 

1. **CC0** : under which data are made available for any use without restriction 
2. **CC BY** : under which data are made available for any use provided that attribution is appropriately given for the sources of data used, in the manner specified by the owner
3. **CC BY-NC** : under which data are made available for any use provided that attribution is appropriately given and provided the use is not for commercial purposes

However, the vast majority of occurrences **have no record-level license** attached to them at all. 

![](/post/2020-02-21-gbif-occurrence-license-processing_files/licenses.svg)

Based on the outcome of the consultation, and the agreement reached with data publishers, the decision was taken that to reach the goal of machine-readable license for all occurrence dataset, GBIF would in all cases use the license given by the data owner in their metadata, while the data owner / publisher would ensure that this accurately reflects the content of the dataset - choosing the most restrictive available license for datasets with mixed licensing, and excluding records from the published dataset that cannot be satisfied by the agreed licensing schemes.

As of the writing of this post, only **4 datasets** now have **2 or more occurrence licenses** due to record-by-record parsing by GBIF. 

1. [iNaturalist research-grade observations](https://www.gbif.org/occurrence/search?dataset_key=50c9509d-22c7-4a22-a47d-8c48425ef4a7)
2. [Insectarium de Montréal (IMQC)](https://www.gbif.org/occurrence/search?dataset_key=3484c814-c0f1-453c-8501-50d4846c4ba6)
3. [Natural history museum data on Canadian Arctic marine benthos](https://www.gbif.org/occurrence/search?dataset_key=eaf9401e-a5e3-4a27-89ad-2d6ac6559167)
4. [A new western Canadian record of Epeoloides pilosulus (Cresson)](https://www.gbif.org/occurrence/search?dataset_key=ad4c9413-95bc-4b3c-a1e9-b7cf16f391a2)

## Current processing logic

The occurrence-level license field is a text field, meaning the values from records in +19K <br>[occurrence datasets](https://www.gbif.org/dataset/search?type=OCCURRENCE) vary widely. Some variants we could interpret easily, whereas others cannot be. In most cases records will still get their dataset-level license applied to them. In situations where we cannot parse or interpret the character string into one of the **3 creative commons licenses** mentioned above, we will default to the **dataset license**. 

Currently, GBIF uses this [dictionary](https://github.com/gbif/parsers/blob/master/src/main/resources/dictionaries/parse/license.tsv) to parse licenses. So if you are a data publisher and want you want your record-by-record occurrences licenses to parse correctly, you can use a supported string from the dictionary or **preferably** use one of the following character strings in the license field: 

1. `https://creativecommons.org/publicdomain/zero/1.0/legalcode`
2. `https://creativecommons.org/licenses/by/4.0/legalcode`
3. `https://creativecommons.org/licenses/by-nc/4.0/legalcode`

Less than 30 datasets have more than one license supplied to them, but only 4 datasets have 2 or more parsable licenses that GBIF accepts. The remaining datasets have one of the following scenarios: 

1. Un-parsable licenses (for example: "Joe's special license")
2. Left the field blank
3. Unsupported licenses (for example: "http://creativecommons.org/licenses/by-sa/1.0/legalcode")

in which case we have given the occurrences the license supplied by the publisher in the meta-data, **which has to be one of the 3 supported creative commons licenses**.

We are in the process of contacting publishers where their occurrence licenses disagrees discernibly from the license on the individual occurrence records.

## Multimedia licenses not changed!

None of the above discussion will have any change to **images**, **video**, or **audio** licenses given to GBIF. These types of licenses have always been parsed individually. See this [post](https://data-blog.gbif.org/post/gbif-multimedia/) for more information about GBIF multimedia licenses.  

