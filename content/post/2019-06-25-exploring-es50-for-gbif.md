---
title: Exploring es50 for GBIF
author: John Waller
date: '2019-07-10'
slug: exploring-es50-for-gbif
categories:
  - GBIF
tags:
  - es50
  - data gaps
  - maps
lastmod: '2019-06-25T14:39:06+02:00'
draft: no
keywords: []
description: ''
authors: ''
comment: no
toc: ''
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


![](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_1.jpg) 

It has been [suggested](https://discourse.gbif.org/t/hunger-mapping-gbif-data-blog/607/3) that GBIF could make **es50** maps similar to what organizations like **OBIS** are <br>[already doing](https://www.researchgate.net/figure/Hurlberts-index-ES50-calculated-on-a-grid-of-5x5-degrees-ES50-is-the-expected_fig4_242783847). I decided to make one for **land animals** (graph above). [link to code](https://github.com/jhnwllr/es50) 

<!--more-->

> **es50 (Hulbert index)** is the statistically expected number of unique species in a random sample of 50 occurrence records, and is an indicator of biodiversity richness. The score can be computed without random sampling, but the mean of infinite random sampling will produce the same result. <br><br> [Obis Definition here](https://obis.org/indicators/documentation/)

Here I plot a global es50 map for **animal genera** on GBIF. Each es50 score represent the expected number of unique animal genera from a sample of 50 occurrence records. I chose to use genus here for efficiency and to avoid naming issues, since I did not do any extensive quality control. Each hexagon is about 480 km across and of equal area, as you can see some distortion towards the poles. <br> [link to grid](/post/2019-06-25-exploring-es50-for-gbif_files/wireframe.jpg) 

You might be thinking, why do this strange statistical exercise? Why not just use species counts? Well, one of the main advantages of es50 is that it somewhat corrects for **sampling bias**.

# Species <b>richness</b> is correlated with <b>effort</b>

<!-- ![](/post/2019-06-25-exploring-es50-for-gbif_files/effortAndSpeciesCounts.svg)  -->
<img width="100%" src="/post/2019-06-25-exploring-es50-for-gbif_files/effortAndSpeciesCounts.svg"></img>

Here I plot the unique animal genus counts versus the number occurrence records for a global grid equal area hexagons (on land). The amount of occurrence records in a region has is highly predictive of the number of unique animal species that region will have. According to raw GBIF records, **Belgium**, **Sweden**, and parts of the **USA** are more biodiverse than **Brazil** and **Madagascar**. And **Iceland** is nearly as species rich as parts of **Brazil**. If GBIF were to plot raw species count maps, it would obviously be non-sense because of **sampling bias**.

> Without some correction for sampling bias we might want to turn **all of Belgium** into a Nature reserve.


# Occurrence records are highly biased to the <span style="color:#D53E4F"><b>north</b></span>

![](/post/2019-06-25-exploring-es50-for-gbif_files/globalOccurrenceCounts_1.svg)

Records in North America, Europe, Austrailia, and South Africa make up close to 85% of all occurrence records collected by the GBIF network. And [70%](https://www.gbif.org/occurrence/search?has_coordinate=true&has_geospatial_issue=false&geometry=POLYGON((-180%2035,180%2035,180%2090,-180%2090,-180%2035))) of all occurrence records are found north of 35°. This heavy sampling bias, means most groups will have their center of diversity shifted to the north. 

# Most raw species-count maps will mirror occurrence-count maps

![](/post/2019-06-25-exploring-es50-for-gbif_files/globalSpeciesCounts_1.svg)

Here is the raw genus count map for animals. It is not hard to see that it simply **mirrors sampling effort**. Unfortunatly most groups (with the exception of birds) will probably have maps that look this way.  

# <span style="color:#3288BD"><b><u>Low</u></b></span> species counts in a known hotspot

<img width="100%" src="/post/2019-06-25-exploring-es50-for-gbif_files/miniMapSpCount_1.svg"></img>

This a raw animal genus count map of **South East Asia** shows <span style="color:#3288BD"><b><u>low</u></b></span>-to-<span style="color:#ABDDA4"><b><u>medium</u></b></span> animal diversity even though this region is a known [biodiversity hotspot](https://discourse.gbif.org/t/hunger-mapping-gbif-data-blog/607/). The sampling effort in this region is [here](/post/2019-06-25-exploring-es50-for-gbif_files/miniMapOccCounts_1.svg). 


# <span style="color:#D53E4F"><b><u>High</u></b></span> es50 scores in a known hotspot

<img width="100%" src="/post/2019-06-25-exploring-es50-for-gbif_files/miniMapEs501.svg"></img>

Our **es50** plot shows most of South East Asia as having medium to high diversity. Although there is still some noise and the result might not be transparent as a simple count map, es50 is able to highlight actual regions of high biodiversity. 

<img width="100%" src="/post/2019-06-25-exploring-es50-for-gbif_files/latGraphs_1.svg"></img>

Here I plot latitude curves for <span style="color:#D54355"><b>es50</b></span> and <span style="color:#3487BD"><b>genus counts</b></span> for animal genera. While the es50 curve seems to be **still slightly biased near 50°N**, the curve obviously is much closer to capturing relative diversity than our genus count curve, which is **total bi-modal nonsense** and only really shows us effort. 

# es50 fail cases

**es50** does sometimes produce nicer looking maps (see fail cases below), but it does not give us what most people want - **species counts**. Unfortunately very few  taxonomic groups (i.e. probably only birds) are well-sampled enough globally to produce species-count maps that are not mirrored occurrence count graphs. Another drawback of es50 is that your group needs to have a reasonable expectation of having >50 species within a given grid cell (although **50 is a somewhat arbitrary choice**). 

> [OBIS Warning](https://obis.org/indicators/documentation/): ES50 assumes that individuals are randomly distributed, the **sample size** is sufficiently **large**, the samples are taxonomically similar, and that all of the samples have been taken in the **same manner**.

### es50 for frogs - few places with >50 species (sample size violation)
![](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_952.svg)

In this map there are **many places where there are fewer than 50 frog species** even though I use fairly large hexagons. In these cases, GBIF might want to change the threshold from 50 species to something smaller. 

### es50 for insects - biased sampling (same manner violoation)

![](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_216.svg)

In this map, the insect data seems to be too highly biased to produce a reasonable es50 result. Perhaps due to malaise trap proejcts, such as the [Swedish Malaise Trap Project](https://www.gbif.org/en/dataset/38c1351d-9cfe-42c0-97da-02d2c8be141c). There could also be other high-effort inventory projects in the USA and Europe that violate the **same manner** assumption. 

# es50 maps for other groups

All of the plots below are based equally-spaced (480 km apart) and equal-area hexagons. I generated the grids using [dggridR](https://cran.r-project.org/web/packages/dggridR/vignettes/dggridR.html). I also removed fossils and livinig specimens from the occurrence records.  


### es50 map links:

* **Mammals** [jpg](post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_359.jpg) | [pdf](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_359.pdf) | [svg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_359.svg) (based on species counts)
* **Birds** [jpg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_212.jpg) | [pdf](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_212.pdf) | [svg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_212.svg) (based on species counts)
* **Frogs** [jpg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_952.jpg) | [pdf](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_952.pdf) | [svg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_952.svg) (based on species counts)
* **Vascular Plants** [jpg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_7707728.jpg) | [pdf](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_7707728.pdf) | [svg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_7707728.svg) (based on genus counts)
* **Insects** [jpg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_216.jpg) | [pdf](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_216.pdf) | [svg](/post/2019-06-25-exploring-es50-for-gbif_files/map_es50_300_216.svg) (based on genus counts)



<!--more-->
