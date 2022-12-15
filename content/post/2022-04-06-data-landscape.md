---
title: The data landscape of GBIF
author: Cecilie Svenningsen
date: '2022-04-06'
slug: gbif-data-landscape
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
How does all the different data provided by participants, publisher and organisations fit into the data landscape of GBIF visualized on gbif.org? Here, I will give a general overview of the different systems and data sources and how they relate to one another.  

To share data through GBIF.org, publishers typically have to collate or transform existing datasets into a standardized format. The transformations include additional processing, content editing and mapping the content of the data to standardized data formats. 
 
As a data publisher, you have different [means to share your data on GBIF](https://data-blog.gbif.org/post/installations-and-hosting-solutions-explained/). 

Here, I give an overview of the different data categories and how GBIF handles det different data sources. It is an insight into 'behind the scenes' of GBIF indexing and data management, but it is not an requirement for data publishers to be able to navigate the full complexity of GBIFs data landscape. 

## Publishing tools
As you may know GBIF publisher can publish four different types of data categories/datasets to GBIF:
- metadata
- taxa
- occurrences
- sampling events

As a publisher, you would need to standardize your data to fit into one or more of these categories. This could be done prior to submitting the data by referring to the [Darwin Core standard terms](https://dwc.tdwg.org/terms/) and then a Publishing Toolkit

## Publishing tool: IPT
The Integrated Publishing Toolkit (IPT) is a free, open source software tool used to publish and share biodiversity datasets through the GBIF network.

