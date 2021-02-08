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

GrSciColl is still a work in progress, and many challenges remain. I won't go into too many details (this is still a blogpost), but some of the areas we would like to address next include:

* Clean duplicates. 
* Make it easier for institutions to improve and maintain their content (e.g. reusing dataset metadata).
* Integrate the TDWG collection descriptors.
* Explore synchronisation with the [NCBI BioCollections](https://ftp.ncbi.nih.gov/pub/taxonomy/biocollections/)
* Integrate external identifiers such as [GRID](https://grid.ac) and [ROR](https://ror.org).

To address these issues, we will need help. We can't do it alone.
This is why we would like to establish a Global editorial team for GRSciColl. The role of this team will be to:
* establish editorial guidelines,
* give feedback on the development of new features aiming to improve GRSciColl,
* advise on the planing and strategy to develop GRSciColl, for example the integration of TDWG collection descriptors.

We will be contacting members of the Natural History Collections and GBIF community in the next few weeks. But don't wait for us to contact you!
If you would like to contribute to this editorial team, don't hesitate to contact us at scientific-collections@gbif.org.