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

> **NOTE:** This is an **experimental feature**, and the implementation may change throughout 2026. The feature is currently available for preview on the GBIF test environment, [GBIF-Test.org](https://www.gbif-test.org/).

<!--more-->

## Summary

Rule based annotations is an experimental tool being developed within GBIF that will allow users to mark certain occurrence data as suspicious. The main goal of the project is to facilitate occurrence data cleaning and user feedback.

### Rules

A basic rule in our system looks like this:

`rule` → `taxon` in `geo-polygon` are `controlled vocab`

In our system a `geo-polygon` is a [Well-Known Text](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) (WKT) object.

**Simple example rules:**

| Rule |
|------|
| `rule` → **Amphibians** in **Greenland** are **suspicious** |
| `rule` → **Penguins** in **Norway** are **suspicious** |
| `rule` → **Lions** in **Ocean** are **suspicious** |

### How to make rules

To make a simple rule:

1. Visit [labs.gbif.org/annotations](https://labs.gbif.org/annotations/#/)

2. Log in with your GBIF username and password.

3. Select the taxon you want to make rules for.

4. Draw a polygon of a region you want to make the rule about.

5. Save the rule to GBIF.

<img src="make_rule.png" alt="Example rule marking future and past occurrence records of class Amphibia on Iceland as Suspicious" width="80%">

*Example rule marking future and past occurrence records of class Amphibia on Iceland as Suspicious.*

## User Guide

### Using the rule based annotations user interface

The rule based annotations user interface can be found at [labs.gbif.org/annotations](https://labs.gbif.org/annotations/#/). Rules are required to be linked to a taxon in the GBIF taxonomy. Users can search for taxa using the search box in the top left corner of the page. Once a taxon is selected, users can create rules by drawing polygons on the map.

<img src="select_species.png" alt="The user interface for creating rule based annotations" width="80%">

*The user interface for creating rule based annotations.*

The default annotation type for making rules is suspicious. This is because the annotation system is primarily intended to be a data cleaning tool. It is possible to select another annotation type after clicking "add more complexity" in the save to GBIF dialogue pop up.

We believe that is easier to mark areas as suspicious rather than trying to create uncontroversial native range maps. So we discourage users from using other annotations types unless absolutely necessary.

Users can view and edit their rules by clicking on their username in the top right corner of the map page.

<img src="jwaller_page.png" alt="Example user page showing all rules created by user jwaller" width="80%">

*Example user page showing all rules created by user jwaller.*

[jwaller rules page](https://labs.gbif.org/annotations/#/user/jwaller)

### Inverting polygons

It is often quite useful invert a selection, so that everywhere but the polygon area is marked as suspicious.

<img src="inverted_polygon.png" alt="This species is only found in Mexico, so all occurrences outside of Mexico are suspicious" width="80%">

*This species is only found in Mexico, so all occurrences outside of Mexico are suspicious.*

This can be done after drawing the polygon by clicking the "invert polygon" button in the edit polygon drop down.

### Complex rules

Sometimes it is useful to add more complexity to a rule. For example, you might want to mark all occurrences of a taxon in a polygon as suspicious except for occurrences that are from a certain dataset or have a certain basis of record.

<img src="not_fossil.png" alt="There are no extant Amphibian populations in Greenland but there are fossil records" width="80%">

*There are no extant Amphibian populations in Greenland but there are fossil records.*

This is possible by using the "add more complexity" button in the save to GBIF dialogue pop up.

<img src="complex_rules_dialogue.png" alt="The complex rules dialogue" width="80%">

*The complex rules dialogue.*

With complex rules user can restrict the rule to only apply to certain `basisOfRecord`, `datasetKey`, or year ranges.

### Voting

For downstream users, deciding which `rule` and `project` to use might become challenging without some quality control. Currently, we have implemented a simple upvote-downvote system for rules. With voting users could see what annotations are supported by the broader community, and create cleaning scripts that are only use annotations supported by the community.

### Higher taxonomy

Creating rules for every species in a group can be slow and inefficient. For this reason, our systems allows users to create rules using higher taxonomy.

For example, it is well known that there are no Amphibians in Antarctica, so rather than creating a separate rule for every species, one can write one rule for the whole group.

`rule` → **Amphibians** in **Antarctica** are **Suspicious**

<img src="higher_taxon_rules.png" alt="Higher taxonomy rules" width="80%">

Now users making rules for lower level groups or species don't need to worry about making rules about Antarctica.

### Using projects

A `project` is a collection of rules. Projects are intended to allow for collaboration between users and logical grouping of rules. Projects can be created and browsed using the user interface at [labs.gbif.org/annotations/projects](https://labs.gbif.org/annotations/#/projects).

<img src="project_page.png" alt="Example project page" width="80%">

*Example project page.*

Only members of a project can create and edit rules within that project. However, all projects and their rules are publicly available for browsing and use. Any user can create a project and invite other GBIF users to collaborate.

### Cleaning GBIF downloads with R

User created rules can be used to clean GBIF downloads with the R package `gbifrules`.

```r
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
```

```
── Cleaning Summary ──────────────────────────────────────────────────────────────────────────────
• Records in original download: 720,217
✖ Suspicious records removed: 14 (0.0019%)
✔ Records remaining: 720,203

Kept: ██████████████████████████████
```

[The R package gbifrules](https://github.com/gbif/occurrence-annotation/tree/main/r-package/gbifrules) provides functions to clean GBIF downloads using user created rules.

## Background and Motivation

### Data cleaning

[The Global Biodiversity Information Facility](https://www.gbif.org/) (GBIF) is a vital data infrastructure for researchers, conservationists, and policymakers across the globe. The quality of GBIF mediated records is crucial for their "fitness for use", and data cleaning is an essential process to most end uses.

<img src="lions.png" alt="Lions in Europe and North America?" width="80%">

*Lions in Europe and North America? It is common for GBIF maps to be confusing for users. Most GBIF users are not interested in records from zoos, fossils, or locations that might just be wrong, and GBIF mediated data is often not consistently rich enough to filter unwanted records.*

### Fixing at source

A competing viewpoint with regard to data cleaning is to "fix at source". Fixing GBIF occurrence data at the source, such as reaching out to data publishers to address issues and errors in their datasets, is an ideal approach in theory. However, in practice, this approach often encounters challenges, primarily because publishers may not respond to emails or communication attempts. Rule-based annotations can contribute to rectifying data problems at their origin as well. Additionally, it is often the case that records do not need to be fixed, but merely are not acceptable for a certain application, such as species distribution mapping.

Automated solutions, like CoordinateCleaner, while valuable tools for data cleaning, may be considered incomplete in certain contexts due to their limited flexibility and potential to miss edge cases. A rule-based annotation system, on the other hand, allows users to make data quality decisions that fit their use case in a more granular way.

## Road Map

Initiated tasks with ✓.

### User Interface ✓

A UI [https://labs.gbif.org/annotations/#/] has been developed to facilitate the process of creating rule-based annotations on a map.

### API backend ✓

The main underlying technology that will be power the annotations will be a database store and a restful API interface.

[Backend GitHub](https://github.com/gbif/occurrence-annotation)

### R package `gbifrules` ✓

Initially the main way to interact with the annotations store, will be through the API or R.

The R package can be found [here](https://github.com/gbif/occurrence-annotation/tree/main/r-package/gbifrules).

### Introduction to internal and external pilot users ✓

Before releasing the rule based annotations tool to the public, it underwent internal and external piloting phases. In this phase, pilot users were given a small annotations task within a group they were familiar with. The pilot users were then interviewed about their overall experience, particularly if there were any "rules" or annotations they wanted to make but could not.

### Data paper ✓

After the internal and external review we are writing a data paper that presents the annotations tool to a wider audience. The internal and external pilot users are encouraged to be co-authors on the paper. The "data" part of the data paper is an export of rules that were created during the pilot phase.
