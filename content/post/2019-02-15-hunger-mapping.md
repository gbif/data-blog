---
title: 'Hunger mapping'
author: John Waller
date: '2019-02-15'
slug: hunger-mapping
categories:
  - GBIF
tags:
  - hunger maps
  - data gaps
  - data quality
lastmod: '2019-02-15T10:10:56+01:00'
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

# Where are we missing biodiversity data?

A **hunger map** is a map of missing biodiversity data (a biodiversity **data gap**). The main challenge with hunger mapping is proving that a species [does not exist but should exist](https://en.wikipedia.org/wiki/Evidence_of_absence)  in a region. Hunger maps are important because they could be used to prioritize funding and digitization efforts. Currently, [GBIF](https://www.gbif.org/what-is-gbif) has no way of telling what species are missing from where. In this blog post I review some potential ways GBIF could make global biodiversity hunger maps.

<!--more-->

# Checklists 

With the checklist approach one simply compares GBIF occurrence data with **a list of species that should be in the country** (or area). This is the **gold standard** of hunger mapping and should be the preferred method **if a checklist exists**. 

![](/post/2019-02-15-hunger-mapping_files/iNaturalistMap.png)

Here we see [iNaturalists approach to hunger mapping](https://www.inaturalist.org/taxa/102661-Hetaerina-cruentata). All checklists seen on [iNauturalist](https://www.inaturalist.org/home) are **user supplied**, meaning that a checklist is created for a region by using the same data that the observation is based on.  

> From iNaturalists' help page: <br><br>
"Every place has a default check list, and whenever an observation is made within the place's boundaries and it has achieved research-grade status, the species observed will get automatically added to the place's check list." -- [source](https://www.inaturalist.org/pages/help)

Obviously GBIF would not want to count occurrences as regional checklists as **GBIF would be eating its own tail.** Ideally we would want a set of **regional checklists** that have been created **without the use of pre-existing GBIF occurrence datasets**. This way we can tell what species should be in a certain country and compare with what species we have lat-lon occurrence data. 

Here are some examples of some **regional checklists** published on GBIF: 


* [Korean Peninsula Flora](https://www.gbif.org/dataset/e09e1e1f-2460-4017-a964-e999abd2bf66)
* [Catalogue of Texas spiders](
https://www.gbif.org/dataset/2c0abc5f-bffd-471b-a932-2288a87668bb)
* [Lista de táxones de la flora vascular española](https://www.gbif.org/dataset/91fecd78-0986-4713-9c36-77532846ee25)
* [Checklist of Danish Fungi](https://www.gbif.org/dataset/2b94a042-fe01-4d9f-8995-d996c21d33cd)
* [Checklist of Beetles of Canada and Alaska](https://www.gbif.org/dataset/7a9bccd4-32fc-420e-a73b-352b92267571)
* [Checklist of Danish Flies and Mosquitoes](https://www.gbif.org/dataset/a7350d23-6727-4eb6-ae0b-6f86fc8d6a71)
* [Catalogue of the Lepidoptera of Belgium](https://www.gbif.org/dataset/848271aa-6a4f-4bae-a055-7081f3e70c4b)

Currently, regional checklist data is **very patchy and disorganized**. And if a country is very data-poor, **it is even less likely to have a regional checklist**. 

# Predictive modelling

While well-curated regional checklists are the gold standard of hunger mapping, they are very hard to organize and use. Another approach would be to generate species denisty maps through statistical modelling.  

![](/post/2019-02-15-hunger-mapping_files/Birds_all_spp.jpg)

For example, this bird diversity map was produced by [biodiversity mapping](https://biodiversitymapping.org/wordpress/index.php/home/), using a statistical modeling approach [citation](https://www.pnas.org/content/110/28/E2602.abstract). Such modelling approaches could be used to compare predicted versus observed species counts. Unfortunately generating global species richness maps requires fairly good global coverage. 

<br>
![](/post/2019-02-15-hunger-mapping_files/molDragonflies.png)
<br>

And even for groups like dragonflies, generating a global species richness map can be difficult, as we can see with this [map of life](https://mol.org/) [species richness map](https://mol.org/patterns/richnessrarity?taxa=dragonflies&indicator=sr) for dragonflies. 


# Intuition and comparison

Below I plot the **global genus counts of animals** (kingdom Animalia) by country according GBIF occurrence data. On this map the **United States has more animal genera than Brazil**, and **Iceland has similar animal genus counts to most African countries**. Obviously one can infer there are likely large **data-gaps** present on this map  **without needing a checklist from each country**.  

![](/post/2019-02-15-hunger-mapping_files/genusCountAnimals.png)

<br> [interactive map available here](https://jhnwllr.github.io/charts/Animals_genusKey.html)

Often an expert only needs taxon counts from a certain area to be able to judge if a data gap is present or not. Additionally even non-experts would be able to guess that [DR Congo](https://www.gbif.org/country/CD/summary) should have  **greater than two times** the animal genus count of [Iceland](https://www.gbif.org/country/IS/summary). 

![](/post/2019-02-15-hunger-mapping_files/icelandGenusCount.png)


# Interactive global genus count maps

Below I link to some interactive genus count maps. These maps should be used for illustration and comparison purposes only, since counting taxonomic units is a hard problem even at the genus level. The numbers might **very much inflated by duplicates and synonyms** (or other issues). In the maps above I have not run any quality controls like excluding extinct species and synonyms. Still the maps broadly highlight where the GBIF network might be hungry for data. 


* [Plants -- kingdom Plantae genus count map](https://jhnwllr.github.io/charts/Plants_genusKey.html)
* [Animals -- kingodm  Animalia genus count map](https://jhnwllr.github.io/charts/Animals_genusKey.html)
* [Mammals -- class Mammalia genus count map](https://jhnwllr.github.io/charts/Mammalia_genusKey.html)
* [Amphibians -- class Amphibia genus count map](https://jhnwllr.github.io/charts/Amphibia_genusKey.html)
* [Ants --  family Formicidae genus count map](https://jhnwllr.github.io/charts/Ants_genusKey.html)
* [Birds -- class Aves genus count map](https://jhnwllr.github.io/charts/Aves_genusKey.html)
* [Butterflies -- order Lepidoptera genus count map](https://jhnwllr.github.io/charts/Butterflies_genusKey.html)
* [Dragonflies -- order Odonata genus count map](https://jhnwllr.github.io/charts/Dragonflies_genusKey.html)
* [Frogs -- order Anura genus count map](https://jhnwllr.github.io/charts/Frogs_genusKey.html)
* [Fungi -- kingdom Fungi genus count map](https://jhnwllr.github.io/charts/Fungi_genusKey.html)
* [Conifer Trees -- class Pinopsida genus count map](https://jhnwllr.github.io/charts/Pinosida_genusKey.html)
* [Reptiles -- class Reptilia genus count map](https://jhnwllr.github.io/charts/Reptilia_genusKey.html)


# Problems with counting species 

**One issue that will effect any method is species counting.** How to divide species will always be controversial. So even if we have a checklist or a model prediction of a species count this will need to be reconciled somehow with the GBIF backbone. 

> "We are currently working with Catalogue of Life and other partners to try to deliver a more seamless and complete working checklist of all species but, right now, our view of the available data includes hundreds of thousands of scientific names which may or may not be accepted species." <br> -- [GBIF's statement about species counts](https://www.gbif.org/about-species-counts)

Even for common species like the European herring gull (Larus argentus) there are [15 unique names](https://www.gbif.org/occurrence/gallery?taxon_key=9741811&taxon_key=9741394&taxon_key=9806227&taxon_key=5846445&taxon_key=8794868&taxon_key=9493513&taxon_key=8754531&taxon_key=2481139&taxon_key=9793385&taxon_key=9250315&taxon_key=8731929&taxon_key=9401820&taxon_key=6178140&taxon_key=9132226&taxon_key=9813593). Some of these names are subspecies names but some are simply duplicates. Cleaning up names for something less common than a seagull will be even more difficult. 

![](/post/2019-02-15-hunger-mapping_files/larusArgentus.png)


# Conclusions 

Hunger mapping would be greatly aided by well-curated and clean dataset of regional checklists. Unfortunately, checklists datasets are currently very disorganized. Even if a checklist for a certain group exists in one region does not mean it will exist in another region. In the future GBIF occurrence datasets may act as default checklists. 

Still with a little bit of domain knowledge and some common sense we can roughly map where we probably want to prioritize funding in the future. Looking at the animal genus counts of the countries in Asia and Africa, shows that the [BIFA](https://www.gbif.org/programme/82629/bifa-biodiversity-information-fund-for-asia) and [BID](https://www.gbif.org/programme/82243/bid-biodiversity-information-for-development) projects are money well spent. 





<!--more-->
