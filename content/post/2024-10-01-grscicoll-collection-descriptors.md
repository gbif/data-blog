---
title: Making collection content discoverable when you don’t have occurrences published on GBIF
author: Marie Grosjean
date: '2024-10-01'
slug: grscicoll-collection-descriptors
categories:
  - GRSciColl
  - Publishing
tags:
  - publish
  - GBIF
  - collection descriptors
  - collections
  - institution
  - GRSciColl
  - Darwin core
  - Latimer core
lastmod: '2024-10-01'
draft: yes
keywords: ["Collection descriptors", "Latimer Core", "Darwin Core", "Institution match none", "GRsciColl", "collection", "institution", "GBIF"]
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

This blog post is a tutorial on how to upload collection descriptors in the [Global Registry of Scientific Collections](https://scientific-collections.gbif.org) (GRSciColl).

If you are someone working with a physical collection or work with people who do, you might be interested in making these collections findable. Ideally, the content of these collections would be digitized and made available online on the relevant platforms like GBIF.org, iDigBio.org, ALA.org.au, etc.
Sharing digital specimen records is a great way to ensure the discoverability of collection content. Not only users can find your collection but can also use individual records as relevant for their research. If you can publish occurrences on GBIF, please do it!

However, not everyone is able to share records at the specimen level on GBIF. For example, you might be working with a mineralogy collection where the data would be outside of the GBIF scope. Or perhaps you only have partial information about your collection: for example, you might only have the number of jars of amphibians or fish.

If that’s your case, you might be interested in packaging the data you have as collection descriptors and uploading them to the Global Registry of Scientific Collections (GRSciColl).

# A brief reminder of what GRScicoll is

The Global Registry of Scientific Collections (GRSciColl) is a registry of physical scientific collections (their content, location and contacts) and their associated institution.
GRSciColl has its own history, and scope. It was originally developed and maintained at the Smithsonian institution before GBIF inherited it. You can read more about how it [here](https://www.gbif.org/news/5kyAslpqTVxYqZTwYn1cub/gbif-provides-new-home-for-the-global-registry-of-scientific-collections) and [here](https://data-blog.gbif.org/post/grscicoll-2021/).
GRSciColl and GBIF have overlapping content: many of the collections listed in GRSciColl are also datasets on GBIF. However, the scope of GRSciColl goes beyond biodiversity data, for example GRSciColl contains mineralogy and archeology collections, but doesn’t include things like citizen science observation data.

You can find out more about GRSciColl and browse the GRSciColl data here: https://scientific-collections.gbif.org. You can also watch this short introduction below.

<iframe title="vimeo-player" src="https://vimeo.com/showcase/10709937/video/872824009" width="640" height="360" frameborder="0" allowfullscreen></iframe>

# What are collection descriptors in GRSciColl?

Collection descriptors were implemented as part of the [GRSciColl 2024 roadmap](https://github.com/gbif/registry/blob/dev/roadmap-grscicoll-2023-2024.md). They are meant to share structured information about collections. They can contain relevant details about collections and sub-collections as well as quantitative data which cannot be shared on collection pages (for example, the number of type specimens for a particular taxon). Some collection descriptors are used for indexing collections. This means that they improve collection discoverability. For example, a collection associated with dragonfly species names will be found by users looking for “Odonata” in the scientific name field of the collection search.

Here are some examples of searches based on collection descriptor:
* [Find some collections with specimens collected by M. J. Berkeley](https://scientific-collections.gbif.org/collection/search?recordedBy=M.%20J.%20Berkeley)
* [Find some fern collections hosted in French institutions](https://scientific-collections.gbif.org/collection/search?country=FR&taxonKey=59)

Currently, only a handful of collection descriptors is indexed and searchable: scientific name, country or area of covereage (of the specimen, this is based on the dwc:country term), recorded by and type status. However, we will be able to add filters for more standardized terms as more descriptors are shared in GRSciColl.

# How to format collection descriptors

Each GRSciColl collection entry can have one or several collection descriptors group. A group can correspond to descriptors for a particular aspect of the collection or a sub-collection. Each group requires:

* A title of the set of descriptors. For example, “Taxonomic breakdown of the algae sub-collection”.
* A description for the set of descriptors. For example, “These descriptors are based on the 2008 inventory of the algarium. This inventory focused mainly on type specimens”.
* A comma-separated file containing the descriptors where each column is a descriptor and each row a subset of the collection described. The header of the table is used to map its content to [Darwin Core](https://dwc.tdwg.org/terms) and [Latimer Core](https://ltc.tdwg.org/quick-reference/) (see more details below).

## The descriptor table

As mentioned above the descriptor tables are CSV files where each row is a subset of the collection (or a group of specimens) and each column is a descriptor.
When possible, the data should be mapped to the [Darwin Core](https://dwc.tdwg.org/terms) and [Latimer Core](https://ltc.tdwg.org/quick-reference/) standards but it possible to share data that isn’t mapped to any standard. When mapped to one of the standards, the header of the column should contain the prefix of the standard (`ltc:` for Latimer Core and `dwc:` for Darwin Core) as well as the name of the term.

A table could look like this fictional example:
| ltc:biomeType | dwc:scientificName  | dwc:country | Number of identified specimens at genus level|
| - | - | - | - |
| Freshwater | Perciformes | Colombia | 300 |
| Freshwater | Perciformes | Brazil | 145 |

In the example above, the last column couldn’t be mapped to any Darwin Core nor Latimer Core term so it was left with a descriptive title. This column won’t be indexed, and users won’t be able to search data based on its values, but it will be displayed along the other descriptors on the collection page. See an example of collection descriptor for the [NY collection](https://scientific-collections.gbif.org/collection/b2190553-4505-4fdd-8fff-065c8ca26f72) where not every column is mapped to a standard:

![NY collection descriptors]()

Note that tables might contain overlapping information or different descriptions for the same subset of specimens.

There isn’t any template to download as the descriptors can include a lot of headers. You are very welcome to download any table you like from GRSciColl and use it as your own template.
Here are some examples that I compiled from real data while working on the implementation of descriptors. They could certainly be mapped differently, and this is to help give an idea of the type of mapping we expect:
* [Example 1](https://github.com/gbif/registry/files/14419456/swisscollnet_ALTERNATIVE_dwcltc_part2_2a8835ad-4a2e-43df-b976-f924f76fe628.csv)
* [Example 2](https://github.com/gbif/registry/files/14419488/swisscollnet_dwcltc_3c41e738-b94e-4ed6-a9ae-f57c7baaf521.csv)
* [Example 3](https://github.com/gbif/registry/files/14419329/rnc_ALTERNATIVE_dwcltc_types_humbolt_a717e77c-ea99-4d81-83ff-81931e753ffc.csv)
* [Example 4](https://github.com/gbif/registry/files/14419363/rnc_dwcltc_geography_6eae4377-f8b4-41ac-a9c1-db5a81afde98.csv)

**NB:** The Latimer core term objectClassificationName is very convenient to describe collection subsets of collections that don’t necessarily have other ways of being grouped. For example, this is helpful for groups of non-monophyletic taxa (for example Algae). Ideally, the names used in this field should follow a controlled vocabulary. We haven’t set up one as we write though. If you need some guidance, I suggest following the DISSCO discipline vocabulary for collections.

# How to upload descriptors

Right now, only editors can upload descriptors to collections. If you aren’t a GRSciColl editor for the collection, institution or country of the collection and you would like to become one, please contact us at scientific-collections@gbif.org. You will need a GBIF account associated with your institutional email address.

Once you have your table of descriptors ready, the process to upload them is relatively straightforward.
1. Go to the collection page that you would like to work on from https://scientific-collections.gbif.org
2. Click on the edit button (you will be redirected to the editing interface)
3. Click on the “Descriptor group” tab (see the screenshot below)
![Descriptor Group tab]()
4. Click Add and fill in the title and description of the descriptor group and choose your CSV file.
5. Then click “add”.

Not that the API can also be used to upload collection descriptors programmatically. It is documented here. Please contact us at scientific-collections@gbif.org if you have any question.

# Updating descriptors

You can update collection descriptors by clicking on the “edit” button of an existing descriptor group and replacing the table by a new one.

# Improving collection descriptors and how to contribute

The collection descriptors are very new on GRSciColl and we would like to make them as useful to the community as possible. If you would like to give your input, please consider joining [our mailing list](https://lists.gbif.org/mailman/listinfo/scientific-collections) or write a post on the [GRSciColl section of our forum](https://discourse.gbif.org/c/grscicoll/29) or leave us a comment here.

Here are a few points where we would like your input:
* Some descriptors would benefit from controlled vocabularies (like the ltc:objectClassificationName). Do you have suggestions or recommendations of exiting vocabularies that we could use? Or specific values that you would like to see?
* We would like to index more descriptors as they are made available on GRSciColl. What type of descriptors would you like to share and see indexed?
* If you have unmappable data and you would like to see them indexed in GRSciColl, please consider reaching out to the Latimer Core maintenance group or open a ticket in their GitHub repository.

Thank you very much! Happy collection describing.

