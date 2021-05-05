---
title: Derived datasets are here
author: Daniel Noesgaard
date: '2021-05-05'
slug: derived-datasets
categories:
  - GBIF
tags:
  - DOI
  - CiteTheDOI
  - Derived datasets
lastmod: '2021-05-05T14:24:11+02:00'
draft: no
keywords: []
description: ''
comment: no
toc: ''
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

## To DOI or not to DOI

You've finished an analysis using GBIF-mediated data, you're writing up your manuscript and checking all the references, but you're unsure of how to cite GBIF. If you [Google it](https://www.google.com/search?q=how+to+cite+GBIF), you'll probably end up reading our [citation guideslines](https://www.gbif.org/citation-guidelines) and quickly realize that GBIF is all about DOIs. Datasets have their own DOIs and downloads of aggregated data also have their own DOIs. 

But maybe you didn't download data through the GBIF.org portal. Maybe you relied on an R package like rgbif or dismo that retrived data synchronously from the GBIF API? Maybe a grad student downloaded if for you? Maybe you accessed the data through the Microsoft AI for Earth program and ran the analysis on Azure? In any case, which DOI do you cite _if you don't have one_?

## Introducing Derived Datasets

To overcome this problem and any other situation in which a user wishes to assign a DOI to a subset of GBIF-mediated data, for which a DOI doesn't exist or isn't representative, we introduce the **Derived Dataset**. Simply put, a derived dataset is a citable record (with a unique DOI) representing a dataset that doesn't exist as a conventional, unfiltered GBIF.org download. So what does it take to create one and how do you do it?

As a minimum you'll need two things: 1) a GBIF.org account and 2) a list of contributing (parent) datasets with record counts—i.e. how many records did each dataset contribute. If your data contains all available fields, you'll need to summarize it based on the `datasetKey` field. There are numerous ways of doing this in R, using Excel, datamash, and other similar tools. But in the end you'll want to have a list like this:

```
<datasetKey 1>		<# records>
<datasetKey 2>		<# records>
<datasetKey 3>		<# records>
```
etc.

Secondly, in the interest of reproducibility, do yourself and the world a favour and deposit your dataset somewhere publically accessible (e.g. Zenodo). Then when you're ready, go to the [derived dataset form](https://www.gbif.org/derived-dataset/register) and fill it out:

![](/post/2021-05-05-derivedDatasets_files/dd_form.png)

All it takes is a **title**, the **URL of where your dataset is deposited**, a **list of datasets** (by datasetKey or DOI) with record counts either as a CSV file or filled in manually, and a **description of what the dataset** is and how it came together. That's it! The form has two additional optional fields, one for specifying an original download DOI if the derived dataset represents a filtered version of that download, and a registration date, if, for someone reason you wish the derived dataset to be registered in the future rather than immediately (e.g. embargoed materials).

And that's it! Once the registration is complete, you can click the **GO TO DATASET** button to see the result and get the DOI.

