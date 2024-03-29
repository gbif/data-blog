---
title: GBIF Regional Statistics - 2020
author: John Waller
date: '2020-05-19'
slug: gbif-regional-statistics-2020
categories:
  - GBIF
tags:
  - statistics
  - gbif regions
lastmod: '2020-05-11T09:04:43+02:00'
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

I was asked to prepare some statistics for the **GBIF regional regional meetings** being held virtually this year. This blog post is a companion for those meetings.  

You can watch a **video presentation** of the preparation of these meetings [here](https://vimeo.com/413657346). The presentation of this blog post starts [here](https://vimeo.com/413657346#t=23m39s).

* The [North American virtual nodes meeting 2020](
https://www.gbif.org/event/5iluOArQeYT29U402zDrwi/north-american-virtual-nodes-meeting-2020) was on **5 - 6 May 2020**
* The [Europe and Central Asia-virtual nodes meeting 2020](https://www.gbif.org/event/513CFo2fc5hhww0ci9NF5z/europe-and-central-asia-virtual-nodes-meeting-2020) was on **11 - 12 May 2020**
* The [Latin America and Caribbean virtual nodes meeting 2020](https://www.gbif.org/event/3CRkTIGOTe3qfVr3FUvjJF/latin-america-and-caribbean-virtual-nodes-meeting-2020) will be on **18 - 20 May 2020**
* The [Africa virtual nodes meeting 2020](https://www.gbif.org/event/2YqTOjAAVFLwlnGZLrujYW/africa-virtual-nodes-meeting-2020) will be on **10 - 12 June 2020**

GBIF introduced a regional framework across the [GBIF Network](https://www.gbif.org/the-gbif-network) a little more than a decade ago, with groups based on clusters of national participants. Soon after the publication of [Brooks et al. (2016)](https://www.nature.com/articles/sdata20167), GBIF adopted their structure to provide a consistent approach to regional reporting and assessment processes [map](https://www.nature.com/articles/sdata20167/figures/1).

This post will cover countries/areas with some political status and not other [GBIF Affiliates]( https://www.gbif.org/the-gbif-network/gbif-affiliates) and Antarctica.

<!--more-->

![](/post/2020-05-11-gbif-regional-statistics-2020_files/gbif_region_map.svg)

## <span style="color:#509E2F"><b>Occurrence</b></span> and <span style="color:#F1AD16"><b>Species</b></span> counts

* **published records** - do not necessarily occur within the region, but are published by  countries/areas within the region. 
* **about records** - must occur within the region but are not necessarily published by countries/areas within the region.  


![](/post/2020-05-11-gbif-regional-statistics-2020_files/gbif_regions_occ_count.svg)

**Europe & Central Asia** and **North America** have by far the most occurrences about/published in their regions. A large part of these records are from bird observations, which is reflected in the species-count graph below.  

![](/post/2020-05-11-gbif-regional-statistics-2020_files/gbif_regions_num_species.svg)

**Latin America & The Caribbean** have the most species about their region, which makes sense from a [species-richness perspective](https://explorer.naturemap.earth/map). **Europe & Central Asia** and **North America** publish the most species, which could occur in any region or in international waters.   

## <span style="color:#FF9326"><b>Birds</b></span> dominate occurrence totals

![](/post/2020-05-11-gbif-regional-statistics-2020_files/facet_stacked_num_of_occ_Global_nobirds_FALSE.svg)

Since the <span style="color:#FF9326"><b>bird</b></span> data makes it hard to see other groups, I plot the same graph below excluding birds and countries with less than 1 million total occurrences. 

![](/post/2020-05-11-gbif-regional-statistics-2020_files/facet_stacked_num_of_occ_Global_nobirds_TRUE.svg)

A large <span style="color:#FF0040"><b>red bar</b></span> indicates that the country has many occurrences **about** the country that are not in the top most popular groups (<span style="color:#FF9326"><b>birds</b></span>, <span style="color:#509E2F"><b>plants</b></span>, <span style="color:#40BFFF"><b>insects</b></span>, <span style="color:#175CA1"><b>mammals</b></span>, <span style="color:#FFCCCC"><b>molluscs</b></span>, <span style="color:#FDAF02"><b>fungi</b></span>). <span style="color:#FF0040"><b>Other</b></span> records are usually fish, amphibians, or reptiles. One can search for a country (e.g. [The United States](https://www.gbif.org/occurrence/taxonomy?country=US)) on the GBIF web interface and get a more detailed view of that country's taxonomic breakdown.   

## Publishing from Latin America, Oceania, Asia, Africa generate a disproportionate number of citations

![](/post/2020-05-11-gbif-regional-statistics-2020_files/data_use.svg)

This plot shows the number of citations generated from data published from countries within a region. This type of graph is only possible when an author [cites a doi](https://www.gbif.org/literature-tracking) associated with a GBIF download.  

Here we see that **Europe & Central Asia** and **North America** have the most total citations. However, occurrences published from **Latin America & The Caribbean**, **Oceania**, **Asia**, and **Africa** generate a disproportionate number of journal citations for every 10M records they publish. 

## GBIF regions gain 25-50K new species every two years

![](/post/2020-05-11-gbif-regional-statistics-2020_files/timeseries_num_species.svg)

GBIF regions gain around **25-50K new species every two years**. In 2022-2024 **Africa** will have the same number of species (with occurrences) as **North America** had on GBIF in 2016-2018. 

## There are still many data gaps!

![](/post/2020-05-11-gbif-regional-statistics-2020_files/global_num_species.svg)

A plot of raw species counts from equal-area hexagons approximately the size of Iceland. This plot excludes international waters, but includes countries EEZ areas. We see here that while **Iceland** has <span style="color:#749DC7"><b>>5K species</b></span> other areas within Amazonia, Eastern Europe, and Africa have suspiciously fewer, **indicating likely data gaps**.