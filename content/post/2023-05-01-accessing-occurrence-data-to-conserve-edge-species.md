---
title: Accessing GBIF-mediated occurrence data to conserve EDGE species
author: Andrew Rodrigues
date: '2023-05-01'
slug: accessing-occurrence-data-to-conserve-edge-species
categories:
  - GBIF
tags:
  - GBIF
  - Data Use
lastmod: '2023-04-28'
draft: no
keywords: ['EDGE', 'threatened species', 'protected areas', 'conservation financing', 'data availability']
description: ''
authors: ''
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

> Evolutionarily Distinct and Globally Endangered (EDGE) species have few close relatives on the tree of life and are often extremely unusual in the way they look, live and behave, as well as in their genetic make-up. They represent a unique and irreplaceable part of the world’s natural heritage. (https://www.edgeofexistence.org/)
 
After reviewing the recent call from the [EDGE Protected Area and Conserved Area Fund](https://www.edgeofexistence.org/edge-protected-and-conserved-area-fund/#:~:text=The%20EDGE%20Protected%20and%20Conserved,across%20the%20tropics%20and%20subtropics) for projects that extend existing protected area networks for the conservation of EDGE species, I wanted to see what data is currently available through GBIF that could help guide decisions on conservation financing. Funds from this call are directed towards projects that establish or expand protected areas for EDGE species that are a) not currently protected and b) either critically endangered (CR), endangered (EN) or vulnerable (VU) EDGE species whose ranges overlap with other unprotected CR or EN species. Importantly, recent verifiable evidence should exist for the named species occurring within the proposed site. 

Given these criteria and a [list of eligible critically endangered, endangered and vulnerable species](https://www.edgeofexistence.org/wp-content/uploads/2023/03/2023_EDGE_species_RT_call.xlsx) provided within the call, I decided to take three eligible countries within the GBIF member network-  Benin, Ecuador and Viet Nam - and look at data availability of EDGE species in GBIF. Uses like this are particularly pertinent given the recent commitments in the new [Kunming-Montreal Global Biodiversity Framework](https://www.cbd.int/article/cop15-final-text-kunming-montreal-gbf-221222) to protect 30 per cent of the Earth by 2030, and I was interested to see to what extent GBIF data can already support spatial prioritization processes. 


#### Subhed on methods

From the call's list of 4,214 species, I found a total of 45, 139 and 171 threatened EDGE species for Benin, Ecuador and Viet Nam, respectively, after matching them to the [GBIF Backbone Taxonomy](https://www.gbif.org/dataset/d7dddbf4-2cf0-4f39-9b2a-bb099caae36c) and removing synonyms (8) and unmatched species (1). I then did an occurrence search for those species in the respective countries, limiting the results by [basis of record](https://docs.gbif.org/course-data-use/en/basis-of-record.html) for preserved specimens, human observations, observations or machine observations. 

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

### Subhed on results

84%, 83% and 63% of EDGE species listed as coming from either Benin, Ecuador or Vietnam respectively had data available in GBIF. Of those species for which we have data in GBIF, 78%, 71% and 51% of of those species in Benin, Ecuador and Viet Nam respectively, had occurrence data available outside of protected areas.

When I compared numbers of occurrence records found outside of protected areas with the total number of occurrences, I found that 23%, 40% and 37% of occurrence records for threatened EDGE species in Benin, Ecuador and Viet Nam respectively were from outside protected areas suggesting either a sampling bias within protected areas for those species or a concentration of those species within protected areas.

What is clear is that, for the majority of threatened EDGE species in these example countries, GBIF has data available. we would encourage that use of the data when identifying areas to extend existing protected area networks. 

![Benin table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/benin_table.png)
![Ecuador table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/ecuador_table.png)
![Viet Nam table](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/vietnam_table.png)

---

![Benin](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/benin-edge.png)

![Ecuador](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/ecuador-edge.png)

![Viet Nam](/post/2023-05-01-accessing-occurrence-data-to-conserve-edge-species/viet_nam-edge.png)
