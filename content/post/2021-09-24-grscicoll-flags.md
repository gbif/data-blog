---
title: What are the flags "Collection match fuzzy", "Collection match none", "Institution match fuzzy", "Institution match none" and how to remove them?
author: Marie Grosjean
date: '2021-10-11'
slug: grscicoll-flags
categories:
  - GBIF
  - Publishing
tags:
  - publish
  - GBIF
  - occurrence
  - sampling-event
  - collections
  - institution
  - GRSciColl
  - flags
lastmod: '2019-12-05'
draft: no
keywords: ["Collection match fuzzy", "Collection match none", "Institution match fuzzy", "Institution match none", "GRsciColl", "collection", "institution", "GBIF"]
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

You are a data publisher of occurrence data through GBIF.org, care about your data quality, and wonder what to do about the issue flags that show up on your occurrences. You might have noticed a new type flag this year relating to collection and institution codes and identifiers. They are the result of our attempt at linking specimens records to [our Registry of Scientific Collections (GRSciColl)](https://www.gbif.org/grscicoll).

We want to be able to group specimens and combine metrics at the collection and institution levels (which can be independant from the way there were published on GBIF).

# What is GRSciColl?

[GRSciColl](https://www.gbif.org/grscicoll), the Registry of Scientific Collections, is a community-curated clearinghouse of collections information. It includes data about physical collections such as: a high-level summary of what they contain, where they are located, who can tell you more about these collections and how to contact them. As well as their institution and associated staff members.

GBIF.org, on the other hand, provides access to digitized data of occurrences (specimens *and* observations) from datasets that are published by institutions and organizations. One website, but two separate data stores behind it that are connected with each other at the occurrence level.

Institutions can use GRSciColl to be more visible and advertise their collections, both digitized and undigitized. GRSciColl offers the possibility to aggregate and organize the specimen-related occurrences published through GBIF.org idependently of the way there are showing at GBIF.org. So a large GBIF dataset can be presented as several collections on GRSciColl and the other way around.
Our editing system (https://registry.gbif.org) was designed to enable anyone to suggest changes that will then be reviewed and applied by the relevant people. Institutions can also request to be made editor so they can maintain their own entries.

If you would like to know more about GRSciColl and learn about other ways it can be used by the community, please check [this 5 min video from June 2021](https://player.vimeo.com/video/564594528?h=745ac06824).
<iframe title="vimeo-player" src="https://player.vimeo.com/video/564594528?h=745ac06824" width="640" height="360" frameborder="0" allowfullscreen></iframe>

# How are specimens occurrences linked to GRSciColl?

Our system interprets every new occurrence published on GBIF. If this occurrence has a value for any of the following terms, the interpretation will attempt to link it to a GRSciColl entry by using [the GRSciColl lookup service](https://www.gbif.org/developer/registry#lookup):
* `institutionCode`
* `collectionCode`
* `institutionID`
* `collectionID`

[The GRSciColl lookup service](https://www.gbif.org/developer/registry#lookup) attempts to find which GRSciColl entries match the codes and identifiers given as input. Anyone can use this service to check institution and collection codes and identifiers in GRSciColl. During the occurrence interpretation, the system will use the publisher country to help choose a match in GRSciColl in cases where there are more than one candidate.

The flags shown on the occurrences are the ones generated by the lookup service. Because codes aren't necessarily unique in GRSciColl, all matches based solely on codes (without identifiers) are flagged as fuzzy matches (more information in the third part of this post).

**NB**: Although the system attempts to link all the occurrences associated with institution and collection codes and identifiers, only spcimen-related occurrences are flagged. Those are the occurrences with the basis of record: `Preserved Specimen`, `Fossil Specimen`, `Living Specimen`, `Material Sample`.

# How to remove the GRSciColl related flags from specimen records: Step by Step

Each flag is decribed in the GRSciColl section of [our GBIF issues and flags blog post](https://data-blog.gbif.org/post/issues-and-flags/).
In general, the way to removed those flags is to use identifiers in the `institutionID` and `collectionID` fields. Here is how to do it:

1. Check if your institutions and collections are on [GRSciColl](https://www.gbif.org/grscicoll).
    * If an institution or a collection is missing, you can add them by clicking on the "Suggest a new institution" or "Suggest a new collection" buttons.
2. Check what kind of identifiers and codes are on your institution and collection pages.
    * Note that you can suggest alternative codes by clicking on the "suggest a change button". You can also ask to add some external identifiers such as ROR and GRID identifiers to your institution and collection GRSciColl pages by contacting scientific-collections@gbif.org.
3. In your dataset:
    * Make sure that the values in the `collectionCode` and `institutionCode` fields correspond to codes or alternative codes on your institution and collection GRSciColl pages.
    * Make sure that the values in the `collectionID` and `institutionID` fields correspond to the identifiers on your institution and collection GRSciColl pages.
4. Don't forget to push the changes to GBIF by publishing the changes you made in your dataset.

Your GRSciColl pages might have zero or many identifiers. **So which identifier should you choose?** We actually have [an FAQ answering that question](https://www.gbif.org/faq?question=how-can-i-improve-the-matching-of-occurrence-records-with-grscicoll). Here is the relevant part:

> There can be several identifiers to choose from, and GBIF recommends in priority order:
> 
> 1. An in-house generated LSID if available (for example: urn:lsid:biocol.org:col:34984),
> 
> 2. A GRSciColl ID or GRSciColl URI (for example: http://grscicoll.org/institution/south-african-institute-aquatic-biodiversity)
> 
> 3. If no others exist, please use the GBIF UUID from the page URL (for example: a90ba963-9569-4b96-8d56-452aa7b83f75 for the URL https://www.gbif.org/grscicoll/institution/a90ba963-9569-4b96-8d56-452aa7b83f75)


I am adding the screenshot below to illustrate where to find them.
![Where are the GRSciColl identifiers to choose](/post/2021-09-24-grscicoll-flags/SANBI_ID_example.png)


I also made [a silent video](https://player.vimeo.com/video/625476118?h=e794d87abd) to illustrate the steps I mentioned above:
<iframe src="https://player.vimeo.com/video/625476118?h=e794d87abd" width="640" height="360" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

**NB**: The version of GRSciColl that we use to interpret the occurrence record is refreshed once a week. This means that if you made any change in GRSciColl, you will have to wait a bit before it translates into a change in the occurrence interpretation.

Hopefully, you now have all the tools to remove the GRSciColl-related flags from your occurrences! Don't hesitate to contact scientific-collections@gbif.org if you have any question or comment on the blogpost.