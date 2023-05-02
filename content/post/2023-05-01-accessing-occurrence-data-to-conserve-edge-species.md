---
title: Accessing GBIF-mediated occurrence data to conserve EDGE species
author: Andrew Rodrigues
date: '2023-05-02'
slug: accessing-occurrence-data-to-conserve-edge-species
categories:
  - GBIF
tags:
  - GBIF
  - Data Use
lastmod: '2023-05-02'
draft: no
keywords: ['EDGE', 'threatened species', 'protected areas', 'conservation financing', 'data availability']
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

> [Evolutionarily Distinct and Globally Endangered](https://www.edgeofexistence.org/) or **EDGE species** have few close relatives on the tree of life and are often extremely unusual in the way they look, live and behave, as well as in their genetic make-up. They represent a unique and irreplaceable part of the world’s natural heritage. 

After seeing an open call for projects that extend existing protected area networks for the conservation of EDGE species through the [EDGE Protected Area and Conserved Area Fund](https://www.edgeofexistence.org/edge-protected-and-conserved-area-fund/#:~:text=The%20EDGE%20Protected%20and%20Conserved,across%20the%20tropics%20and%20subtropics), I wanted to see what data is currently available through GBIF that could help guide decisions on conservation financing. This call will direct funds toward projects that establish or expand protected areas for EDGE species that are a) not currently protected and b) either critically endangered (CR), endangered (EN) or vulnerable (VU) EDGE species whose ranges overlap with other unprotected CR or EN species. Importantly, the call notes that **recent verifiable evidence should exist for the named species occurring within the proposed site.** 

Given these criteria and a [list of eligible critically endangered, endangered and vulnerable species](https://www.edgeofexistence.org/wp-content/uploads/2023/03/2023_EDGE_species_RT_call.xlsx) provided within the call, I decided to look at data availability of EDGE species in three eligible countries within the GBIF member network: [Benin](https://www.gbif.org/country/BJ/about), [Ecuador](https://www.gbif.org/country/EC/about) and [Viet Nam](https://www.gbif.org/country/VN/about). Uses like this are particularly pertinent given the commitment agreed under the new [Kunming-Montreal Global Biodiversity Framework](https://www.cbd.int/article/cop15-final-text-kunming-montreal-gbf-221222) to protect 30 per cent of the Earth by 2030, and I was interested to see to what extent GBIF data can already support spatial prioritization processes. 

## Methods

From the list of 4,214 species identified in the call, I found a total of 45, 139 and 171 threatened EDGE species for Benin, Ecuador and Viet Nam, respectively, after matching them to the [GBIF Backbone Taxonomy](https://www.gbif.org/dataset/d7dddbf4-2cf0-4f39-9b2a-bb099caae36c) and removing synonyms (8 spp.) and unmatched species (1 sp.). I then did an occurrence search for those species in the respective countries, limiting the results by [basis of record](https://docs.gbif.org/course-data-use/en/basis-of-record.html) for preserved specimens, human observations, observations or machine observations. 

Given that I wanted to identify occurrences of species _outside_ existing protected areas, I filtered out records with a coordinate uncertainty greater than 10,000 metres, as uncertainties greater than that would likely introduce too much uncertainty into where exactly the best location for proposed additions to a protected area would be.  

Here is the code used in the package rgbif with the example for Ecuador and using the file that I created using the GBIF species name matching tool named - “Ecuador_edge_normalized_accepted”:

```R
occ_download(
  pred_in("taxonKey", Ecuador_edge_normalized_accepted$key),
  pred_in("country", "EC"),
  pred_in("basisOfRecord", c('PRESERVED_SPECIMEN','HUMAN_OBSERVATION','OBSERVATION','MACHINE_OBSERVATION')),
  pred("hasCoordinate", TRUE),
  pred("hasGeospatialIssue", FALSE),
  pred_or(
    pred_lt("coordinateUncertaintyInMeters", 10000),
    pred_isnull("coordinateUncertaintyInMeters")
  ),
  format = "SIMPLE_CSV",
  user="xxxx",pwd="xxxx",email="xxxxxxx"
)
```

Once I had all my occurrence, I performed some post-download geoprocessing in QGIS to buffer my occurrence points using coordinate uncertainty values in the `coordinateUncertaintyInMeters` field. and only keep those occurrences where the uncertainty buffers did not intersect with any existing protected area as defined using the [Protected Planet data layers](https://www.protectedplanet.net/). 

Occurrence maps are below with a few example areas highlighted.

## Results

The majority of EDGE species listed as coming from each country had data available in GBIF: 84 per cent from Benin, 83 per cent from Ecuador and 63 per cent from Vietnam. For these "GBIF-ready" species, occurrence data is available outside of protected areas for 78, 71 and 51 per cent of them in Benin, Ecuador and Viet Nam, respectively.

Comparing the number of records found outside of protected areas with the total number of occurrences, I found that the lower percentage of occurrence records available for threatened EDGE species outside protected areas in Benin (23 per cent), Ecuador (40 per cent) and Viet Nam (37 per cent), respectively, suggests either a sampling bias within protected areas for those species or a concentration of those species within protected areas.

What is clear is that, **for the majority of threatened EDGE species in these example countries, GBIF has data available**. We would encourage projects to use the data when identifying areas to extend existing protected area networks. 

--- 

### Benin

![Benin](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/benin-edge.png)

![Benin table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/benin_table.png)

---

### Ecuador

![Ecuador](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/ecuador-edge.png)

![Ecuador table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/ecuador_table.png)

---

### Viet Nam

![Viet Nam](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/viet_nam-edge.png)

![Viet Nam table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/vietnam_table.png)

---

### The EDGE alliance 

The EDGE Protected and Conserved Fund is a partnership between [Durrell Wildlife Conservation Trust](https://www.durrell.org/), [On The Edge Conservation](https://www.ontheedge.org/), [Re:wild](https://www.rewild.org/), [Royal Botanic Gardens, Kew](https://www.kew.org/), [Synchronicity Earth](https://www.synchronicityearth.org/) and the [Zoological Society of London](https://www.zsl.org/), with funding provided by [Rainforest Trust](https://www.rainforesttrust.org/).
