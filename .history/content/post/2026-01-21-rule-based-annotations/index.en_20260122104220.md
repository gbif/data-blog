---
title: Introducing Rule-based Annotations
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

> **NOTE:** This is **experimental**, and the implementation may change. Please see the [GBIF technical documentation](https://techdocs.gbif.org/en/tools/rule-based-annotations) for the latest updates on the tool. GBIF makes no guarantees about the availability or stability of this tool.

**Rule based annotations** is an experimental tool that will allow users to mark certain occurrence data as suspicious. The main goal of the project is to facilitate data cleaning and user feedback to publishers.

<div style="text-align: center;">
  <big><strong><a href="https://labs.gbif.org/annotations/#/">Link to UI</a></strong></big>
</div><br>

<!--more-->

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording.webm" type="video/webm">
  Your browser does not support the video tag.
</video>


## How to make rules

To make a simple rule:

1. Visit [labs.gbif.org/annotations](https://labs.gbif.org/annotations/#/)

2. Log in with your GBIF username and password.

3. Select the taxon you want to make rules for.

4. Draw a polygon of a region you want to make the rule about.

5. Save the rule to GBIF.

<img src="make_rule.png" alt="Example rule marking future and past occurrence records of class Amphibia on Iceland as Suspicious" width="70%">

*Example rule marking **future** and **past** occurrence records of class Amphibia on Iceland as Suspicious.*

# User Guide

## Basic Rule Structure 

A basic **rule** in our system looks like this:

> **rule** → **taxon** in **geo-polygon** are **controlled vocab**

In our system a `geo-polygon` is a [Well-Known Text](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) (WKT) object.

**Simple example rules:**

| Rule |
|------|
| `rule` → **Amphibians** in **Greenland** are **suspicious** |
| `rule` → **Penguins** in **Norway** are **suspicious** |
| `rule` → **Lions** in **Ocean** are **suspicious** |

## Creating rules

Rules are required to be linked to a taxon in the GBIF taxonomy. Users can search for taxa using the search box in the top left corner of the page. Once a taxon is selected, users can create rules by drawing polygons on the map.

The default annotation type for making rules is **suspicious**. This is because the annotation system is primarily intended to be a data cleaning tool. It is possible to select another annotation type after clicking "add more complexity" in the save to GBIF dialogue pop up.

We believe that is easier to mark areas as suspicious rather than trying to create uncontroversial native range maps. So we discourage users from using other annotations types unless absolutely necessary.

Users can view and edit their rules by clicking on their username in the top right corner of the map page.

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording-table.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

<!-- <img src="jwaller_page.png" alt="Example user page showing all rules created by user jwaller" width="70%"> -->

*Example user page showing all rules created by user jwaller. Users can also edit rules on this page.*

[jwaller rules page](https://labs.gbif.org/annotations/#/user/jwaller)

## Inverting polygons

It is often quite useful invert a selection, so that everywhere but the polygon area is marked as suspicious.

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording-inverted.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

*This species is only found in Mexico, so all occurrences outside of Mexico are suspicious.*
<!-- 
<img src="inverted_polygon.png" alt="This species is only found in Mexico, so all occurrences outside of Mexico are suspicious" width="70%"> -->


This can be done after drawing the polygon by clicking the "invert polygon" button in the edit polygon drop down.

## Complex rules

Sometimes it is useful to **add more complexity** to a rule. For example, you might want to mark all occurrences of a taxon in a polygon as suspicious only for occurrences that are from a certain dataset or have a certain basis of record.

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording-complex.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

*There are no extant Amphibian populations in Svalbard, but there are legitimate fossil records.*
<!-- 
<img src="not_fossil.png" alt="There are no extant Amphibian populations in Greenland but there are fossil records" width="70%"> -->

This is possible by using the "add more complexity" button in the save to GBIF dialogue pop up.

<!-- <img src="complex_rules_dialogue.png" alt="The complex rules dialogue" width="70%"> -->
<!-- *The complex rules dialogue.* -->

With complex rules user can restrict the rule to only apply to certain **basisOfRecord**, **datasetKey**, or **year ranges**.

## Voting

For downstream users, deciding which **rules** to use might become challenging without some quality control. Currently, we have implemented a simple upvote-downvote system for rules. With voting users could see what annotations are supported by the broader community, and create cleaning scripts that are only use annotations supported by the community.

## Higher taxonomy

Creating rules for every species in a group can be slow and inefficient. For this reason, our systems allows users to create rules using higher taxonomy. For example, it is well known that there are no Amphibians in Antarctica, so rather than creating a separate rule for every species, one can write one rule for the whole group.

`rule` → **Amphibians** in **Antarctica** are **Suspicious**

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording-higherrank.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

*Rules can be created for higherrank taxa that can then be used to filter occurrence records on lower ranks. The UI allows users to toggle between viewing rules already created at a higher level.*

Now users making rules for lower level groups or species don't need to worry about making rules about Antarctica.

## Using projects

A **project** is a collection of rules. Projects are intended to allow for collaboration between users and logical grouping of rules. Projects can be created and browsed using the user interface at [labs.gbif.org/annotations/projects](https://labs.gbif.org/annotations/#/projects). Users can set the active project so that any rules they create are added to that project. Users can also add pre-existing rules to a project by editing the rule and selecting the project from a drop down menu.

<video controls autoplay loop muted playsinline width="800" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="screen-recording-project.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

*Projects can be created on user pages. Users can set the default project they want to create rules in.*

Only members of a project can create and edit rules within that project. However, all projects and their rules are publicly available for browsing and use. Any user can create a project and invite other GBIF users to collaborate.

## Cleaning GBIF downloads with R

User created rules can be used to clean GBIF downloads with the R package `gbifrules`. [The R package gbifrules](https://github.com/gbif/occurrence-annotation/tree/main/r-package/gbifrules) provides functions to clean GBIF downloads using user created rules.

```r
# Install 
pak::pak("gbif/occurrence-annotation/r-package/gbifrules")
```

```r
library(gbifrules)
library(rgbif)
# Ambystoma mexicanum
occ_download(
pred("taxonKey","2431950"),
pred_default(),
format = "SIMPLE_CSV"
)
 
# Download and clean
# Download was already made earlier 
d <- occ_download_get('0051416-241126133413365') %>%
occ_download_import()

clean_download(d)

# Or filter your data using only rules from a specific project:
clean_download(d, project_id = 1)
```

```
── Cleaning Summary ──────────────────────────────────────────────────────────────────────────────
• Records in original download: 720,217
✖ Suspicious records removed: 14 (0.0019%)
✔ Records remaining: 720,203

Kept: ██████████████████████████████
```

## Next Steps and Integration into GBIF.org

Currently, rules are only available through the [labs.gbif.org/annotations](https://labs.gbif.org/annotations/#/) interface and the R package `gbifrules`. Rules **do not** appear on or get used on the main GBIF.org site or appear on maps or filter occurrences in downloads. It is anticipated that if the tool becomes popular and widely used, that rules will be integrated into the main GBIF.org systems in the future. 

