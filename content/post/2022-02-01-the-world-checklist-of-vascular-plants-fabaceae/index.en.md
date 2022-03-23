---
title: The World Checklist of Vascular Plants (Fabaceae) 
author: John Waller
date: '2022-03-23'
slug: []
categories:
tags: []
lastmod: '2022-02-01T11:19:49+01:00'
draft: no
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

**The World Checklist of Vascular Plants (WCVP): Fabaceae** is a new GBIF mediated [checklist](https://www.gbif.org/fr/dataset/f7053f73-74fb-4c9f-ab63-de28c61140c2) that drastically increases the coverage of the family Fabacea in the **GBIF backbone**. 

<!--more-->

![](images/https___inaturalist-open-data.s3.amazonaws.com_photos_66470341_original.jpg)<p>
<small><i>Acacia leiophylla Benth.</i> is one of the names added to the GBIF Backbone. [dudz](https://www.inaturalist.org/photos/66470341) 
<img src="images/39px-Cc.logo.circle.svg.png" alt="drawing" width="15"/>[*](https://www.gbif.org/occurrence/2603289072)</small>

The [checklist](https://www.gbif.org/fr/dataset/f7053f73-74fb-4c9f-ab63-de28c61140c2) has more than **87K** names. Over <span style="color:#FDAF02"><b>30K</b></span> of these names are new to the GBIF backbone. Around <span style="color:#509E2F"><b>44K</b></span> of these names were already in the GBIF backbone prior to publication of the **WCVP:(Fabacea)** checklist, but are now sourced from this checklist.  

![](images/legume_name_source.svg)

Many thousands of names were added across many genera in the family. From the graph it is clear that some genera more than doubled the number of names in the group (Medicago,Ononis,Lathyrus...).

![](images/legume_name_source_genus.svg)

## Higher-rank matches

After incorporating WCVP:(Fabacea) into the GBIF taxonomic backbone, around **50K** occurrences which previously only matched to the GBIF backbone taxonomy at a [higher rank](https://data-blog.gbif.org/post/issues-and-flags/), now get matched to the backbone with no higher-rank flag. As can be seen in the graph, most of these newly matched occurrences are coming from the genus Ulex. 

![](images/legume_occ.svg)

There are still around [200K Legume occurrences](https://www.gbif.org/occurrence/search?has_coordinate=true&has_geospatial_issue=false&issue=TAXON_MATCH_HIGHERRANK&taxon_key=5386)
 that get flagged as matching at a higher rank. More than **50%** (140K) of these higher rank matches however are **varieties** or **subspecies** that get moved to a species level rank because of a missing name in the GBIF backbone (or misspelling, missing authorship ect.). 

Decreasing the number of [higher-rank matches](https://data-blog.gbif.org/post/issues-and-flags/) is a way for GBIF to measure if the taxonomic backbone is improving. There are cases, however, when adding more names to a group can actually **increase** the number higher-rank matches in GBIF. 

This can happen when GBIF only had one low-rank name but an update adds more names. Within Legumes, publishers who previously published occurrences with a [dwc:scientificName](https://dwc.tdwg.org/terms/#dwc:scientificName) set to  **Acacia auriculiformis** would get matched to the only available name at that time _Acacia auriculaeformis Benth._[*](https://www.gbif.org/species/2981002) but now there are [two names](https://api.gbif.org/v1/species/match?name=Acacia%20auriculiformis&verbose=TRUE) for GBIF interpretation to choose from : 

1) _Acacia auriculiformis A.Cunn._[*](https://www.gbif.org/species/8587163)  
2) _Acacia auriculaeformis Benth._[*](https://www.gbif.org/species/2981002)

_Acacia auriculiformis A.Cunn._ is a synonym of _Acacia auriculaeformis Benth._ but **GBIF interpretation has no way to tell if a publisher means the synonym or the accepted species**. 

## Legume Phylogeny Working Group (LPWG) 

This effort was undertaken by the **Legume Phylogeny Working Group’s (LPWG)**. [The World Checklist of Vascular Plants (Fabaceae)](https://www.gbif.org/dataset/f7053f73-74fb-4c9f-ab63-de28c61140c2) is a subset of [The World Checklist of Vascular Plants (WCVP)](https://www.gbif.org/dataset/f382f0ce-323a-4091-bb9f-add557f3a9a2). It is published by the [Kew The Royal Botanic Gardens](https://www.gbif.org/publisher/061b4f20-f241-11da-a328-b8a03c50a862). 
