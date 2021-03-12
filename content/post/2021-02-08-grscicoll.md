---
title: The GBIF Registry of Scientific Collections (GRSciColl) in 2021
author: Marie Grosjean
date: '2021-03-12'
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
lastmod: '2021-03-12'
draft: no
keywords: ['GRSciColl', 'institution', 'collection']
description: ''
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

The [GBIF Registry of Scientific Collections](https://www.gbif.org/grscicoll), also known as GRSciColl, has been [available on GBIF.org since 2019](https://www.gbif.org/news/5kyAslpqTVxYqZTwYn1cub/gbif-provides-new-home-for-the-global-registry-of-scientific-collections) but it recently got some more attention when we connected it to GBIF occurrences.
Now is the perfect time to share a bit of GRSciColl history and what we plan for its future.

# A brief history of GRSciColl

First of all, here are a few facts about GrSciColl today, at the start of 2021:

* > This registry of scientific collections builds on GRSciColl, a comprehensive, community-curated clearinghouse of collections information originally developed by Consortium of the Barcode of Life (CBOL). See: https://www.gbif.org/grscicoll
* GRSciColl is embedded in the GBIF registry, available in the API and on the web, and can be edited by trusted data managers.
* It currently hosts [8,140 institution entries](https://www.gbif.org/grscicoll/institution/search) as well as [6,313 collection](https://www.gbif.org/grscicoll/collection/search) and [16,543 staff member](https://www.gbif.org/grscicoll/person/search) entries.
* About half of the institutions, 67% of the collections and 77% of the staff entries are synchronized with [Index Herbariorum](http://sweetgum.nybg.org/science/ih/).
* In addition to that, over 1,500 collections were created or updated by [iDigBio](https://www.idigbio.org/portal/collections).
* Whenever possible, institutions and collections are linked to GBIF occurrences. See for example, [the National Fish Collection of South African Institute for Aquatic Biodiversity](https://www.gbif.org/grscicoll/collection/a7d9ed64-4668-41b3-a862-27d87c50bfed/metrics).
* Currently, around 141 million (72%) [specimen-related occurrence records](https://www.gbif.org/occurrence/search?basis_of_record=PRESERVED_SPECIMEN&basis_of_record=FOSSIL_SPECIMEN&basis_of_record=LIVING_SPECIMEN&occurrence_status=present) link to a GRSciColl entity.
* Institutions can request access to our registry to update and maintain their own institution and collection entries.

But GRSciColl was quite different when we inherited it. Some of these features are the result of our work for the past two years.
I will detail some of the GRSciColl history below. But first, I thought it would be nice to have a little visual so I summoned my doodling skills and made this:

![GRSciColl doodles](/post/2021-02-08-grscicoll/GRSciColl.PNG)

1. In 2018 GBIF inherited GRSciColl from the [Smithsonian Institution](https://www.si.edu). It took some adjustment and work with the Smithsonian but we integrated into our registry and eventually made it available on GBIF.org (in 2019). Our first milestone!
2. In 2019, we reached out to the community: we welcomed volunteer editors and started working with Index Herbariorum (IH) and iDigBio on connecting them to GRSciColl.
3. By the beginning of 2020, GRSciColl was synchronizing with IH. Our second milestone! (thank you IH)
4. Not directly related to the technical developments of GRSciColl, but early 2020 also marks the [Advancing the Catalogue of the World’s Natural History Collections consultation](https://www.gbif.org/news/6TvOkvpPlxRm5vHxljYNN5/virtual-workshop-advancing-the-catalogue-of-the-worlds-natural-history-collections).
5. The same year, we finished importing the iDigBio collection data into GRSciColl and iDigBio started using our registry. An other milestone! (thank you iDigBio)
6. And we also linked occurrences from GBIF.org with institution and collection codes and identifiers with GRSciColl. More information [here](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll). Our latest milestone!

# Remaining challenges and future plans

GRSciColl is still a work in progress, and many challenges remain. I won't go into too many details (this is still a blogpost), but some of the areas we would like to address next include:
1. **Reduce the amount of duplicate records**: The connection with Index Herbariorum and import of iDigBio enriched the catalogue, but also increased the number of duplicate entities that can’t be automatically handled. We have been working on implementing tools and functions to help data content managers identify and clean those dupicates more efficiently. We would like to ask the community for help to assess and clean these duplicates. One way to pariticpate, is to review the potiential duplicates listed in [this GitHub repository](https://github.com/gbif/collections-duplicates).
2. **Allow anyone to propose changes**: Most of the update requests are handled by email. We aim to develop an interface allowing anyone to propose a change to any field and state whether they have authority to approve it. Changes are then to be reviewed and applied by the editorial team.
3. **Improve documentation.**
4. **Grow the pool of editors.**
5. **Define and implement the master data management solution**.
6. **Develop a richer user interface**.

For more information, you can consult our [2021 Roadmap](https://github.com/gbif/registry/blob/master/roadmap-grscicoll.md).

If you would like to contribute to improving the GRSciColl content, don't hesitate to contact us at scientific-collections@gbif.org.
