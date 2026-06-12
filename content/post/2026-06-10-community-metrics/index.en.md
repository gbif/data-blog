---
title: Community Metrics
author: Andrew Rodrigues and John Waller 
date: '2026-06-10'
slug: community-metrics
categories:
  - GBIF
tags: []
lastmod: '2026-06-10T11:23:00+02:00'
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

Data gaps are a common topic of discussion when talking about GBIF.  Taxonomic, geographic and temporal gaps in the global dataset exist, yet we still have problems quantifying those gaps and measuring how well we are doing in filling those gaps.  GBIF already provides [analytics](https://www.gbif.org/analytics/global) and [country reports](https://analytics-files.gbif.org/country/AR/GBIF_CountryReport_AR.pdf) both of which provide useful information on data publishing activities but give little signal to what extent these data publishing activities are filling **data gaps**. With that in mind, we have started to think about  how we can provide more nuanced analytics that quantify gaps and identify where priorities for data mobilisation might start. 

> **[Explore our proposal](https://labs.gbif.org/country-metrics)**

<!--more-->

## Background

This responds directly to [Target 21 of the Kunming-Montreal Global biodiversity framework on data and information](https://www.cbd.int/gbf/targets/21) where the Growth in species occurrence records accessible through the Global Biodiversity Information Facility is already a complementary indicator that countries can use for reporting progress towards the target.

We have developed an initial set of national-level metrics to help identify and quantify data gaps. Built using our [new SQL download functionality](https://www.gbif.org/occurrence/download/sql#about). 

### Design Principles

These metrics were designed with the following principles in mind:

1. They should be easy to interpret 
2. They should be useful 
3. They should not require third-party data (Outside of GBIF) 

## Available Metrics

### Summary metrics

Similar to what is found in country reports, these provide summary statistics of data publishing and use in terms of numbers of occurrences, publishers, datasets, species and citations.

### Occurrence records over time

Growth in the number of occurrences over time by taxonomic group.  A toggle function allows users to show the same chart but only for data publishers in the country for a comparison of growth in data from national sources versus international sources.

<video controls autoplay loop muted playsinline width="100%" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="Occurrence-Records-Over-Time.mp4" type="video/mp4">
  <source src="Occurrence-Records-Over-Time.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

### Dataset scatter plot

Datasets are plotted by the number of occurrences by the number of species in the dataset. Datasets providing a low number of occurrences for a smaller number of species and vice versa can be easily identified.  Toggles allow for: switching between linear and log scales; switching to only datasets published in the country; and highlighting different dataset categories e.g. citizen science vs gridded data vs eDNA.

<video controls autoplay loop muted playsinline width="100%" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="Dataset-Scatter-Plot.mp4" type="video/mp4">
  <source src="Dataset-Scatter-Plot.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

### Species Accumulation Curves

Building on work in a [previous blog post](https://data-blog.gbif.org/post/2025-03-26-species-accumulations-curves-with-gbif-sql-downloads/), these show the cumulative number of species for which we have occurrence data over time. Without national species checklists, it will never be possible to say to what extent we are truly plateauing in terms of numbers of species but we do think there is still a signal in the data. For example, if you look at the curve for growth in Amphibian species for neighbouring Colombia and Venezuela between 1900 and 2026, there seems to be a much greater than expected discrepancy between numbers of species indicating real data gaps in Venezuela.

<video controls autoplay loop muted playsinline width="100%" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="Species Accumulation Curve.mp4" type="video/mp4">
  <source src="Species Accumulation Curve.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

### Species count maps

Countries are gridded using ISEA3H hexagonal grids at resolution 7 and observed species richness counts are provided for each grid cell.  In this example for Australia, you can see clear delimitations between marine and terrestrial habitats and high concentrations of species richness along the eastern coast.  It is important to note that these could reflect both real differences in biological diversity between areas, undersampling in certain regions or both.

<video controls autoplay loop muted playsinline width="100%" onloadedmetadata="this.playbackRate = 1.5;">
  <source src="Species-Count-Map.mp4" type="video/mp4">
  <source src="Species-Count-Map.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

### Summary Table

A table showing both growth in occurrences and respective number of species for taxonomic groups.  The goal is to provide metrics on how data mobilisation efforts over the past year have increased coverage for different taxonomic groups using mean and median occurrences per species and occurrence and species growth.

## Who are these aimed for?

These were originally designed with a view to supporting organisations interested in assessing data gaps at a national level or those looking to develop data mobilisation strategies that would support the filling of data gaps.  Ultimately, anybody who is interested in these kinds of analyses can use them and input in their development.

## Next steps

We acknowledge that these metrics are far from perfect. For example, it still remains difficult to determine true under sampling versus true differences in species richness as we still lack effective measures of sampling effort.  However we do think that these provide useful starting points for developing data mobilisation strategies and encourage you to be part of the process by [providing us with your feedback](https://github.com/gbif/CommunityMetrics).  We are interested in any new metrics that could be of interest, what sorts of formats this information could be provided in, and how we might turn these into time series indicators and thus something that could show how well we are doing at filling data gaps.  This will be in a demo version for the next year before we decide on its future, so please join and be part of the process!

