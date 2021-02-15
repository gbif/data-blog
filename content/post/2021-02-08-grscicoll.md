---
title: The GBIF Registry of Scientific Collections (GRSciColl) in 2021
author: Marie Grosjean
date: '2021-02-08'
slug: grscicoll-2021
categories:
  - GBIF
  - GRSciColl
  - registry
tags:
  - GRSciColl
  - GBIF
  - Collection
  - Institution
  - iDigBio
  - IH
lastmod: '2021-02-08'
draft: yes
keywords: ['GRSciColl', 'institution', 'collection']
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

The [GBIF Registry of Scientific Collections](https://www.gbif.org/grscicoll), also known as GRSciColl, has been [available on GBIF.org since 2019](https://www.gbif.org/news/5kyAslpqTVxYqZTwYn1cub/gbif-provides-new-home-for-the-global-registry-of-scientific-collections) but it recently got some more attention when we connected it to GBIF occurrences.
Now is the perfect time to share a bit of GRSciColl history and what we plan for its future.

# A brief history of GRSciColl

First of all, here are a few facts about GrSciColl today, at the start of 2021:

* > This registry of scientific collections builds on GRSciColl, a comprehensive, community-curated clearinghouse of collections information originally developed by Consortium of the Barcode of Life (CBOL). See: https://www.gbif.org/grscicoll
* GRSciColl is embedded in the GBIF registry, available in the API and on the web, and can be edited by trusted data managers.
* It currently hosts [8,251 institution entries](https://www.gbif.org/grscicoll/institution/search) as well as [6,306 collection](https://www.gbif.org/grscicoll/collection/search) and [16,514 staff member](https://www.gbif.org/grscicoll/person/search) entries.
* About half of the institutions, 67% of the collections and 77% of the staff entries are synchronized with [Index Herbariorum](http://sweetgum.nybg.org/science/ih/).
* In addition to that, over 1,500 collections were created or updated by [iDigBio](https://www.idigbio.org/portal/collections).
* Whenever possible, institutions and collections are linked to GBIF occurrences. See for example, [the National Fish Collection of South African Institute for Aquatic Biodiversity](https://www.gbif.org/grscicoll/collection/a7d9ed64-4668-41b3-a862-27d87c50bfed/metrics).
* Currently, around 141 million (72%) [specimen-related occurrence records](https://www.gbif.org/occurrence/search?basis_of_record=PRESERVED_SPECIMEN&basis_of_record=FOSSIL_SPECIMEN&basis_of_record=LIVING_SPECIMEN&occurrence_status=present) link to a GRSciColl entity.

But GRSciColl was quite different when we inherited it. Some of these features are the result of our work for the past two years.
I will detail some of the GRSciColl history below. But first, I thought it would be nice to have a little visual so I summoned my doodling skills and made this:

![GRSciColl doodles](https://github.com/gbif/data-blog/blob/master/content/post/2021-02-08-grscicoll/GRSciColl.PNG)

1. In 2018 GBIF inherited GRSciColl from the [Smithsonian Institution](https://www.si.edu). It took some adjustment and work with the Smithsonian but we integrated into our registry and eventually made it available on GBIF.org (in 2019). Our first milestone!
2. In 2019, we reached out to the community: we welcomed volunteer editors and started working with Index Herbariorum (IH) and iDigBio on connecting them to GRSciColl.
3. By the beginning of 2020, GRSciColl was synchronizing with IH. Our second milestone! (thank you IH)
4. Not directly related to the technical developments of GRSciColl, but early 2020 also marks the [Advancing the Catalogue of the World’s Natural History Collections consultation](https://www.gbif.org/news/6TvOkvpPlxRm5vHxljYNN5/virtual-workshop-advancing-the-catalogue-of-the-worlds-natural-history-collections).
5. The same year, we finished importing the iDigBio collection data into GRSciColl and iDigBio started using our registry. An other milestone! (thank you iDigBio)
6. And we also linked occurrences from GBIF.org with institution and collection codes and identifiers with GRSciColl. More information [here](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll). Our latest milestone!

# Remaining challenges and future plans

GRSciColl is still a work in progress, and many challenges remain. I won't go into too many details (this is still a blogpost), but some of the areas we would like to address next include:
1. Cleaning the data content: GRSciColl contains many duplicate entries that need cleaning. We are working on implementing tools and function to help data content manager identify and clean those dupicates more efficiently.
2. Making it easier for institutions to improve and maintain their content: at the moment most updates are done thanks to IH synchronozation or email requests. We would like to:
    * explore synchronisation with the [NCBI BioCollections](https://ftp.ncbi.nih.gov/pub/taxonomy/biocollections/),
    * enable the possibility to choose EML files as master records,
    * set up an interface for more detailed update requests.
3. Redesign the current GRSciColl interface.

We also plan to later broaden the group of editors to establish the Editorial board.

If you would like to contribute to improving the GRSciColl content, don't hesitate to contact us at scientific-collections@gbif.org.
