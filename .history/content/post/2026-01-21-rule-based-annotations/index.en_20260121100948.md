---
title: Rule-based Annotations
author: John Waller
date: '2026-01-21'
slug: []
categories:
  - GBIF
tags: []
lastmod: '2026-01-21T15:51:13+02:00'
draft: yes
keywords: []
description: ''
authors: ''
comment: no
toc: no
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

= Rule Based Annotations

include::ROOT::partial$under-construction.adoc[]

NOTE: This is an **experimental feature**, and the implementation may change throughout 2026.  The feature is currently available for preview on the GBIF test environment, https://www.gbif-test.org/[GBIF-Test.org].

== Summary

Rule based annotations is an experimental tool being developed within GBIF that will allow users to mark certain occurrence data as suspicious. The main goal of the project is to facilitate occurrence data cleaning and user feedback.

=== Rules

A basic rule in our system looks like this.

`rule` ->  `taxon` in `geo-polygon` are `controlled vocab`

In our system a `geo-polygon` is a https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry[Well-Known Text] (WKT) object. 

.simple example rules
[width="100%",options="footer"]
|====================
|`rule` -> *Lions* in *Greenland* are *suspicious*
|`rule` -> *Penguins* in *Norway* are *suspicious*
|`rule` -> *Lions* in *Ocean* are *suspicious*
|====================

=== How to make rules

To make a simple rule : 

1. Visit https://labs.gbif.org/annotations/#/[labs.gbif.org/annotations]

2. Log in with your GBIF username and password.

3. Select the taxon you want to make rules for. 

4. Draw a polygon of a region you want to make the rule about.

5. Save the rule to GBIF. 

.Example rule marking future and past occurrence records of class Amphibia on Iceland as Suspicious.
image::annotations/make_rule.png[width=80%]

== User Guide

=== Using the rule based annotations user interface

The rule based annotations user interface can be found at https://labs.gbif.org/annotations/#/[labs.gbif.org/annotations]. Rules are required to be linked to a taxon in the GBIF taxonomy. Users can search for taxa using the search box in the top left corner of the page. Once a taxon is selected, users can create rules by drawing polygons on the map. 

.The user interface for creating rule based annotations.
image::annotations/select_species.png[width=80%]

The default annotation type for making rules is suspicious. This is because the annotation system is primarily intended to be a data cleaning tool. It is possible to select another annotation type after clicking "add more complexity" in the save to GBIF dialogue pop up. 

We believe that is easier to mark areas as suspicious rather than trying to create uncontroversial native range maps. So we discourage users from using other annotations types unless absolutely necessary.

Users can view and edit their rules by clicking on their username in the top right corner of the map page. 

.Example user page showing all rules created by user jwaller.
image::annotations/jwaller_page.png[width=80%]

https://labs.gbif.org/annotations/#/user/jwaller[jwaller rules page]

=== Inverting polygons 

It is often quite useful invert a selection, so that everywhere but the polygon area is marked as suspicious. 

.This species is only found in Mexico, so all occurrences outside of Mexico are suspicious.
image::annotations/inverted_polygon.png[width=80%]

This can be done after drawing the polygon by clicking the "invert polygon" button in the edit polygon drop down. 

=== Complex rules 

Sometimes it is useful to add more complexity to a rule. For example, you might want to mark all occurrences of a taxon in a polygon as suspicious except for occurrences that are from a certain dataset or have a certain basis of record. 

.There are no extant Amphibian populations in Greenland but there are fossil records.
image::annotations/not_fossil.png[width=80%]

This is possible by using the "add more complexity" button in the save to GBIF dialogue pop up. 

.The complex rules dialogue.
image::annotations/complex_rules_dialogue.png[width=80%]

With complex rules user can restrict the rule to only apply to certain `basisOfRecord`, `datasetKey`, or year ranges. 

=== Voting

For downstream users, deciding which `rule` and `project` to use might become challenging without some quality control. Currently, we have implemented a simple upvote-downvote system for rules. With voting users could see what annotations are supported by the broader community, and create cleaning scripts that are only use annotations supported by the community.

=== Higher taxonomy

Creating rules for every species in a group can be slow and inefficient. For this reason, our systems allows users to create rules using higher taxonomy. 

For example, it is well known that there are no Amphibians in Antartica, so rather than creating a seperate rule for every species, one can write one rule for the whole group. 

`rule` -> *Amphibians* in *Antarctica* are *Suspicious*

image::annotations/higher_taxon_rules.png[width=80%]

Now users making rules for lower level groups or species don't need to worry about making rules about Antarctica. 

=== Using projects 

A `project` is a collection of rules. Projects are intended to allow for collaboration between users and logical grouping of rules. Projects can be created and browsed using the user interface at https://labs.gbif.org/annotations/#/projects[labs.gbif.org/annotations/projects]. 

.Example project page.
image::annotations/project_page.png[width=80%]

Only members of a project can create and edit rules within that project. However, all projects and their rules are publicly available for browsing and use. Any user can create a project and invite other GBIF users to collaborate.

=== Cleaning GBIF downloads with R

User created rules can be used to clean GBIF downloads with the R package `gbifrules`.

[source,R]
----
library(gbifrules)
library(rgbif)
# Ambystoma mexicanum
# occ_download(
# pred(taxonKey,"2431950"),
# pred_default(),
# format = "SIMPLE_CSV"
# )
 
# Download and clean
d <- occ_download_get('0051416-241126133413365') %>%
occ_download_import()

clean_download(d) 
----

[source, console]
----
── Cleaning Summary ──────────────────────────────────────────────────────────────────────────────
• Records in original download: 720,217
✖ Suspicious records removed: 14 (0.0019%)
✔ Records remaining: 720,203

Kept: ██████████████████████████████
----

https://github.com/gbif/occurrence-annotation/tree/main/r-package/gbifrules[The R package gbifrules] provides functions to clean GBIF downloads using user created rules. 

== Background and Motivation

=== Data cleaning

https://www.gbif.org/[The Global Biodiversity Information Facility] (GBIF) is a vital data infrastructure for researchers, conservationists, and policymakers across the globe. The quality of GBIF mediated records is crucial for their "fitness for use", and data cleaning is an essential process to most end uses.

.Lions in Europe and North America? It is common for GBIF maps to be confusing for users. Most GBIF users are not interested in records from zoos, fossils, or locations that might just be wrong, and GBIF mediated data is often not consistently rich enough to filter unwanted records.
image::annotations/lions.png[width=80%]

=== Fixing at source

A competing viewpoint with regard to data cleaning is to "fix at source". Fixing GBIF occurrence data at the source, such as reaching out to data publishers to address issues and errors in their datasets, is an ideal approach in theory. However, in practice, this approach often encounters challenges, primarily because publishers may not respond to emails or communication attempts. Rule-based annotations can contribute to rectifying data problems at their origin as well. Additionally, it is often the case that records do not need to be fixed, but merely are not acceptable for a certain application, such as species distribution mapping.

Automated solutions, like CoordinateCleaner, while valuable tools for data cleaning, may be considered incomplete in certain contexts due to their limited flexibility and potential to miss edge cases. A rule-based annotation system, on the other hand, allows users to make data quality decisions that fit their use case in a more granular way.

== Road Map

Initiated tasks with ✓.

=== User Interface ✓

A UI [https://labs.gbif.org/annotations/#/] has been developed to facilitate the process of creating rule-based annotations on a map. 

=== API backend ✓

The main underlying technology that will be power the annotations will be a database store and a restful API interface.

https://github.com/gbif/occurrence-annotation[Backend GitHub]

// This is not public. We can add to https://techdocs.gbif.org/en/openapi/v1/occurrence (marked as experimental)
// if required. — Matt.
// http://prodws-vh.gbif.org:8124/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config[API Docs]

=== R package `gbifrules` ✓

Initially the main way to interact with the annotations store, will be through the API or R.

The R package can be found https://github.com/gbif/occurrence-annotation/tree/main/r-package/gbifrules[here].

=== Introduction to internal and external pilot users ✓

Before releasing the rule based annotations tool to the public, it underwent internal and external piloting phases. In this phase, pilot users were given a small annotations task within a group they were familiar with. The pilot users were then interviewed about their overall experience, particularly if there were any "rules" or annotations they wanted to make but could not.

=== Data paper ✓

After the internal and external review we are writing a data paper that presents the annotations tool to a wider audience. The internal and external pilot users are encouraged to be co-authors on the paper. The "data" part of the data paper is an export of rules that were created during the pilot phase.



// === Controlled vocabulary

// One of the key ways to increase usability and complexity is to introduce a controlled vocabulary.

// ."Penguins released in Norway". While the most accurate description of this event is the sentence above, a more useful rule might be "Penguins in Norway are suspicious".
// image::annotations/penguins.png[]

// Using a small controlled vocabulary over in an annotation system offers several advantages to downstream users. Finding the right level of granularity and flexibility within the controlled vocabulary is key to reaping the benefits while accommodating the specific needs of the annotation user.

// .Example annotation that marks any occurrences of lions in Greenland as suspicious.
// image::annotations/lions-greenland.png[width=80%]

// === Focus on location

// We've made a deliberate choice to concentrate on *location* rule-based annotations for biodiversity occurrences. This decision stems from our goal to streamline and focus our efforts while addressing the most https://github.com/gbif/portal-feedback/issues?q=is%3Aissue+location+[a prevalent type of feedback we receive at GBIF].

// It's important to note, however, that the concept of rule-based annotations is inherently extensible. While our initial focus centres on location data, the same framework and principles can be applied to other areas of data quality improvement within the GBIF context. This adaptability allows us to remain responsive to evolving user needs and feedback, ensuring that our efforts can be broadened to encompass other data quality challenges in the future. Ultimately, our aim is to create a flexible and scalable solution that can continue to benefit the biodiversity community as a whole.

// === Comparison with similar projects

// Other efforts exist to catalogue the ranges of the living world:

// * https://www.iucnredlist.org/resources/spatial-data-download[IUCN range maps]
// * https://mol.org/[Map of life]
// * https://www.inaturalist.org/pages/atlases[iNaturalist atlases]

// While these efforts are useful and well-developed, none of them are expressly focused on data quality. Namely, none of these systems allow users to easily state with a simple controlled vocabulary and rules where occurrences for a species are likely and unlikely.

// .Our system allows users to annotate at an granular scale. For example, this annotation marks all occurrences that happen to be near this greenhouse as "managed".
// image::annotations/greenhouse-managed.png[]

// 3. Next apply your label.

// .Controlled vocabulary for locations
// [width="100%",options="header,footer"]
// |====================
// |  term | definition
// | Native| Refers to the natural geographic range where a species or organism historically evolved and occurs without human intervention.
// | Introduced | Refers to the geographic area where non-native organisms have been intentionally or accidentally introduced and established
// | Managed    | Encompasses the geographic area where specific species are actively controlled, conserved, or manipulated by human intervention.
// | Former     |  Denotes the historical geographic area where a species once naturally occurred but no longer does due to various factors.
// | Vagrant    | Describes sporadic occurrences of a species far outside its usual habitat or distribution, often due to rare or accidental dispersal events.
// | Suspicious | Occurrences occurring in the designated area might be in error in some way.
// |====================

// If vocabulary this doesn't work for you, please pick the closest fitting, and request additional vocabulary in your feedback.

// Create your rule.



// ==== Rule extensions

// We have found in initial testing that only being able to annotate land areas (a geo-polygon) is restrictive, so it is anticipated that certain extensions to this basic formula might be supported.

// For example, often occurrence records can be suspicious but still be in a somewhat plausible location. A natural way to handle such cases would be to allow for rules with GBIF `datasetKey`.

// `rule` ->  `taxon` in `geo-polygon` and `datasetKey` are `controlled vocab`

// For example,

// `rule` -> *Lions* in *South Africa* and *datasetKey* are *suspicious*

// Another natural extension might be GBIF `basisOfRecord`.

// For example, https://data-blog.gbif.org/post/country-centroids/[country centroid] locations are often only suspicious for museum specimens, so a user could define a rule that captures this knowledge.

// `rule` -> *Lions* in *Centroid of South Africa* and *Preserved Specimen* are *suspicious*

// "Centroid of South Africa" would, of course, be defined by some WKT object like a circle or a polygon.

// Finally, there might be other fields that might make good qualifiers/extensions, like `year`.

// === Rulesets

// A `ruleset` is a collection of `rules`.

// For example, a `ruleset`  could be "Annotations of the Genus Leo", and it could look something like the table below.

// .Example ruleset
// [width="100%",options="footer"]
// |====================
// |`rule` -> *Lions* in *Greenland* are *Suspicious*
// |`rule` -> *Lions* in *Ocean* are *Suspicious*
// |`rule` -> *Lions* in *South Africa* are *Native*
// |`rule` -> *Lions* in *WKT polygon of National Park* are *Native*
// |`rule` -> *Lions* in *WKT polygon of Zoo* are *Managed*
// |`rule` -> *Lions* in *Centroid of SA* and *Preserved Specimens* are *Suspicious*
// |====================


// === Sharing rules

// It is also anticipated that a desirable feature would allow users to "borrow" `rule` or geo-polygon from another `ruleset` and assign a new taxonKey or add a rule extension. This will reduce the storage strains on GBIF and prevent duplicate work.

// For example, a common `rule` might be to mark something in the ocean as suspicious. A user should be able apply this rule to a new taxonKey without creating a new ocean polygon every time.

// === Exceptions to rules

// Creating cast-down annotations can be hard due to several reasons related to the nature of the task and **exceptions to the rule**. An exclusion rule could be efficient for higher level downcasting of rules.

// For example, a rule could exclude a certain group

// `rule` -> `taxon` in `geo-polygon` are `controlled vocabulary` except `taxon x`

// `rule` -> *Amphibians* in *Antarctica* are *Suspicious* except **Antartic frogs**

// .https://edition.cnn.com/2020/04/23/world/antarctica-first-frog-species-scn/index.html[Frog article]
// image::annotations/frogs.png[]

// A work around to *rule exceptions* could of course be rules that simply *conflict*.

// === Conflicting rules

// Inevitably, there are going to be rules created in our system that conflict. For example, a user might mark and area as "Native", while another user will mark the same area as "Suspicious".

// In our rule-based system, unlike perhaps other platforms, we are not striving to create a single ground truth. We aim only to have a collection of useful opinions, and we leave it to the end user to decide what to do with the information.

// === Rules with more than one taxon

// It might be efficient in some circumstances to express rules with more than one taxon:

// rule -> `taxon_1` + `taxon_2` `...` in `geo-polygon` are `controlled vocabulary`

// One useful example would be marking all https://www.marinespecies.org/[marine species] on land as suspicious.

// rule -> *Marine species on WORMS list* in *Land Polygon* are *Suspicious*

// === Controlled vocabulary

// We might consider using the preexisting vocabulary, although we are attempting to annotate land area (ranges) more than we are attempting annotate occurrence records.

// https://registry.gbif.org/vocabulary/DegreeOfEstablishment/concepts

// Below is the working controlled vocabulary for location-based annotations.

// .Controlled vocabulary for locations
// [width="100%",options="header,footer"]
// |====================
// |  term | definition
// | Native| Refers to the natural geographic range where a species or organism historically evolved and occurs without human intervention.
// | Introduced | Refers to the geographic area where non-native organisms have been intentionally or accidentally introduced and established
// | Managed    | Encompasses the geographic area where specific species are actively controlled, conserved, or manipulated by human intervention.
// | former     |  Denotes the historical geographic area where a species once naturally occurred but no longer does due to various factors.
// | Vagrant    | Describes sporadic occurrences of a species far outside its usual habitat or distribution, often due to rare or accidental dispersal events.
// | Suspicious | Occurrences occuring in the designated area might be in error in some way.
// |====================

// This vocabulary is meant to be a compromise between modeling species ranges and establishment means accurately, while not being overly complex.

// .Example mappings
// [width="100%",options="header,footer"]
// |====================
// |concept    | example
// |native	    | extant
// |native	    | endemic
// |native	    | indigenous
// |native	    | breeding
// |native	    | non-breeding
// |introduced	| assisted colonization
// |introduced	| invasive
// |introduced	| non native range
// |managed	| location is captive range
// |managed	| location is botanical garden
// |managed	| location is zoo
// |managed	| cultivated in glasshouse
// |suspicious	| location is in the ocean
// |suspicious	| zero-zero coordinate
// |suspicious	| centroid
// |suspicious	| area too far north for taxon
// |suspicious	| area too high elevation for taxon
// |suspicious	| area is natural history museum
// |former	    | fossil range
// |former	    | extinct
// |former	    | historic
// |vagrant    | migrant
// |====================

// The current vocabulary might change in the future. Namely, there has been some discussion introducing hierarchy such that perhaps certain terms map to `present` or `absent` for example.

// .A burning question at this point might be why not annotate occurrences directly?
// === Why not annotate occurrences directly?

// Annotating land areas (and extensions) provide at least two advantages over annotating occurrences:

// 1. Avoids the use of https://www.gbif.org/news/2M3n65fHOhvq4ek5oVOskc/new-processing-routine-improves-stability-of-gbif-occurrence-ids[unstable gbifIds].
// 2. Allows for future occurrences to benefit from the annotation.







<!--more-->
