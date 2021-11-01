---
title: Identifying potentially related records - How does the GBIF data-clustring feature work?
author: Tim Robertson and Marie Grosjean
date: '1990-12-05'
slug: clustering-occurrences
categories:
  - GBIF
  - data use
tags:
  - GBIF
  - specimen
  - occurrence
lastmod: '2019-12-05'
draft: yes
keywords: ['occurrence', 'specimen', 'clustering', 'duplicate']
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

The data available GBIF include many duplicate records.
This is something that users might be familiar with. You download data from GBIF, analyze them and realize that some records have the same exact date, scientific name, catalogue number and location but come from two different publishers or have silighly different attributes.
Sometimes [an observation was recorded in two different systems](https://discourse.gbif.org/t/duplicate-observations-across-datasets/3069), sometimes a specimen was digitized twice, sometimes a record has been enriched with genetic information and republished via a different platform... There can be many reasons why these duplicates appear on GBIF.

This is why last year, we released an experimental data-clustering feature aiming to identifying potentially related records on GBIF.
The goal is not only to detect potential duplicates but also find related type specimens and expose links between records from different sources (Natural History collections, DNA-derived sequences and materials examined in taxonomic treatments).

Records that have been included in a cluster can be found with the "is in cluster" filter in [the occurrence search](https://www.gbif.org/occurrence/search?advanced=1&occurrence_status=present&is_in_cluster=true). Each occurrence page belonging to a cluster will have a "CLUSTER" tab displaying the potentially related records. You can read [this news item](https://www.gbif.org/news/4U1dz8LygQvqIywiRIRpAU/new-data-clustering-feature-aims-to-improve-data-quality-and-reveal-cross-dataset-connections) for more information and some exciting examples.

# How does the GBIF data-clustering feature work?

## 1. Select candidates

Comparing nearly billion records with each other is very resource intensive and quite impractical so the first step of the data-clustering process is to select and group candidate records.

The system first creates a series of "hashes" for each record based on specified fields. All records sharing a hash are candidates to compare against each other.

For example, one of the "hashes" used is based on the species key, rounded coordinates, year, month and day. This means that the records that share the same values for those fields will be grouped together in the candidate table.

The fields used to identify and group the candidates are a subset of that is used later on for comparing them (see the table below in the post).
See [this link](https://github.com/gbif/pipelines/blob/dev/gbif/pipelines/clustering-gbif/src/main/scala/org/gbif/pipelines/clustering/Cluster.scala) to check the details.


## 2. Compare candidates with each other and assess whether they are potentially related

In this second step, the system will compare the candidate records with each other and generate assertions. The combinations of assertion will determine whether two records are potentially related.
In the table below, I summarize how those assertions are made but if you would like more details, you can check the code available [here](https://github.com/gbif/pipelines/blob/dev/sdks/core/src/main/java/org/gbif/pipelines/core/parsers/clustering/OccurrenceRelationships.java#L26).

| Assertion | fields checked | condition checked |
|-----|----|----|
| Same specimen | `taxonKey`, `typeSatus` | same taxonKey between records and type status = Holotype for both records |
| Typification relation | `scientificName`, `typeStatus` | same between records|
| Same accepted species | `speciesKey` | same between records |
| Same date | `eventDate` or `day`, `month`, `year` | same between records |
| Approximate date | `day`, `month`, `year` | dates one day apart |
| Different date | `eventDate` | differs between records |
| Non conflicting date | `eventDate`, `day`, `month`, `year` | no date on either record |
| Same recorder name | `recordedBy` | same between records |
| Same coordinates | `decimalLatitude`, `decimalLongitude` | same between records | 
| Non conflicting Coordinates | `decimalLatitude`, `decimalLongitude` | no coordinate on one or both sides |
| Within 200 m | `decimalLatitude`, `decimalLongitude` | distance <= 0.200 |
| Within 2 km | `decimalLatitude`, `decimalLongitude` | distance <= 2.00 |
| Same country | `countryCode` | same between records |
| Non conflicting country | `countryCode` | country only on one record |
| Different country | `countryCode` | differ between records|
| Identifiers overlap | `occurrenceID`, `fieldNumber`, `recordNumber`, `otherCatalogueNumber`, grouped(`institutionCode`:`collectionCode`:`catalogueNumber`), grouped(`institutionCode`:`catalogueNumber`) | checks any overlap of identifiers between records|

See below, the combinations of assertions that can create a cluster. If a group of occurrences share the combinations of assertions for any given column, they will be clustered together.

| Assertion | | | | | | | | | | | | | |
|--|--|--|--|--|--|--|--|--|--|--|--|--|--|
| Same specimen | | | | | | | | | | | x | |
| Typification relation | | | | | | | | | | | | x |
| Same accepted species | x | x | x | x | x | x | x | x | x | x | | |
| Same date | x | x | | | x | | x | | | | | |
| Approximate date | | | | | | | | | x | x | | |
| Different date | | | | | | | | | | | | |
| Non conflicting date | | | x | x | | x | | x | | | | |
| Same recorder name | | | | | | | | | x | x | | |
| Same coordinates | x | | x | | | | | | x | | | |
| Non conflicting Coordinates | | | | | | | x | x | | | | |
| Within 200 m | | x | | x | | | | | | | | |
| Within 2 km | | | | | x | x | | | | x | | |
| Same country | | | | | | | | | | | | |
| Non conflicting country | | | | | | | | | | | | |
| Different country | | | | | | | | | | | | |
| Identifiers overlap | | | x | x | x | x | x | x | | | | |


> **NB**: Any group of occurrence associated with the assertion `Different date` or `Different country` will not be clustered together.

# Why some occurrences aren't clustered

It is possible that some occurrences check one of the combinations of assertions but aren't shown as clustered yet. This could be the case for several reasons:

1. The occurrences are newly published. Right now, the clustering process is quite resource intensive and doesn't run automatically. We need to trigger it manually. This means that it can take a few weeks before newly published occurrences get clustered.
2. The "duplicates" come from the same dataset. The clustering algorithm only compares occurrences across datasets not within datasets.
3. There can be a delays between the moment the occurrences are clustered and the moment they become searchable with the "is in cluster" filter (this is due to some technical reasons a bit too long to explain in this post.)

There could be any number of unforeseen reasons. If in doubt, please contact us to helpdesk@gbif.org.

# What you can do to improve linkage with other occurrences?

If for one reason or another, you need to publish on GBIF occurrences for observations or specimens that you know are already on GBIF, how best to do it?

1. Make sure that you reuse the same identifiers as much as possible. Same catalogue numbers, occurrenceID, etc.
2. Use the [associatedOccurrences](https://dwc.tdwg.org/terms/#dwc:associatedOccurrences) and [occurrenceRemarks](https://dwc.tdwg.org/terms/#dwc:occurrenceRemarks). Those won't help the clustering function but it will help users understand the relationships between occurrences.
