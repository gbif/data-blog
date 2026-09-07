---
title: Good SAMaritans – Modelling survey location, date and time when publishing Survey and Monitoring data to GBIF
author: Marie Grosjean and Kate Ingenloff 
date: '2026-09-07'
slug: sam-survey-where-when
categories:
  - GBIF
tags:
  - SAM
  - Humboldt
  - DwC-DP
  - Darwin Core
  - DwC-A
  - publish
  - site
  - date
  - location
  - time
lastmod: '2026-09-07'
keywords: ['Survey And Monitoring', 'Humboldt Extension', 'Data modelling']
description: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: no
draft: no
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


If you are new here, this post is the third installment in a series about modelling and sharing survey and monitoring (SAM) data on GBIF. My name is Marie, I am a Data Administrator at the GBIF Secretariat, and I am learning how to work with SAM data.

In my quest to improve my SAM data literacy skills, I am accompanied by the wise [**Kate Ingenloff**](https://orcid.org/0000-0001-5942-9053), our SAM data modelling expert as well as Sam the Secretary Bird, our silly mascot. You can get to know Sam in [our first two blog posts](https://data-blog.gbif.org/tags/sam/).

This series is meant to be a fun and entertaining introduction to sharing SAM data. If you prefer a comprehensive, straight to the point approach to the topic, Kate wrote an [excellent guide](https://doi.org/10.35035/doc-ynvs-eh84) although the guide is not yet updated with guidance for the Darwin Core Data Package whereas you will get guidance for sharing data with Darwin Core Archives and Data Packages here).

Now, today we are going to talk about space and time!

<img align="center" src="/post/2026-09-07-sam-survey-where-when/Sam_meets_TARDIS.png" alt="Sam meets the TARDIS">

Those of you who are sci-fi/pop-culture enthusiasts might recognize the TARDIS from Doctor Who, a time and space travelling machine. I just put it there for the joke (and for fellow Dr Who fans). We aren’t going to talk about time travel today. Mostly because we can only time travel forward and also because I wouldn’t know how to model time-travel surveys in the Darwin Core Data Standard.

What we are going to talk about are survey sites and temporal scope. In other words:

> A survey site refers to the location at which observations are made or samples and/or measurements are taken (https://docs.gbif.org/guide-publishing-survey-data/en/#survey-site).

> Temporal scope refers to the date and time of a survey, and how long the survey lasted (https://docs.gbif.org/guide-publishing-survey-data/en/#survey-date-and-time).

I used to think that all field ecologists needed to report was a set of coordinates for the area they surveyed and the date they conducted the survey. But I have since realized the error of my ways. Coordinates are great (and we need them) but they don’t tell the whole story. 

Maps and biogeographic layers can help researchers infer the types of habitats surveyed, but it isn’t enough. For example, depending on the time of the year, the  day and the protocols used, one set of coordinates can correspond to very different habitats surveyed and information collected.

<img align="center" src="/post/2026-09-07-sam-survey-where-when/same_location_different_habitats.png" alt="Same location, different habitats">

If you have been following our series, you know that our mascot, Sam, is conducting a butterfly survey. Luckily for that squirrel, Sam’s protocol doesn’t involve any gut sampling (it does involve sucking all flying insects with a doomsday vacuum called the LepiPro Proton Pack). But whether Sam surveys a large spring meadow or a small decaying log will significantly change the likelihood of catching the target species.

So, what information should be reported when it comes to sites surveyed?

You can start by reporting the **sites locality**. In other words, the location on the map: the **country**, **locality** name and ideally **coordinates** (decimal latitude and longitude). Each locality should be identified with a unique identifier, when possible (**locationID**), this is especially helpful when locations are surveyed multiple times. Here is the map of Sam’s survey sites:

<img align="center" src="/post/2026-09-07-sam-survey-where-when/localities_surveyed.png" alt="Map of localities surveyed" width="300"/>

You should also **describe** each site, this includes reporting:

* **habitat**
* **weather**
* **extreme conditions**

Sam has in fact attempted to survey a site during a snowstorm and didn’t find any butterflies. Reporting the information helps assess whether the absence of the species can be inferred.

<img align="center" src="/post/2026-07-15-sam-intoduction/sam_snow.PNG" alt="Bad weather for butterfly catching" width="300"/>

The **size** of the **area** sampled should also be reported. How much ground (or volume) was surveyed? There are several ways to report this depending on the type of survey and methods useds: transect length, volume of water filtered, etc. 

Consider Sam’s surveys. For all but the very first one, Sam surveyed each one square kilometer plot (about 10,763 square feet) three times, which means that the total area surveyed was three times the size of the original plots (3 km2).

<img align="center" src="/post/2026-09-07-sam-survey-where-when/area_scope_vs_total_survey.png" alt="Total area surveyed">

It’s also important to report the geospatial scope of the survey (if it’s available). This scope indicates the whole area of interest and can be helpful to data users in understanding the whole story of your data.

For example, Sam is surveying five sites on a small island with an area of 23 km2 called Survey Island. These sites are meant to understand the biodiversity on the island as a whole. So, Survey Island itself is the geographic scope of Sam’s survey. This is reported as an area (23 km2) in Sam’s data. 
In other words, these five square kilometers represent the sampling of the larger 23 sq km area of interest.

<img align="center" src="/post/2026-09-07-sam-survey-where-when/localities_surveyed.png" alt="Map of localities surveyed" width="300"/>

When possible, it is helpful to report vegetation cover. There are several methods for reporting **vegetation cover** (more information is available at the end of the post).

Rocky terrains with almost zero vegetation cover might not be the best place to survey butterflies.

<img align="center" src="/post/2026-09-07-sam-survey-where-when/rocky_terrain.png" alt="Sam rocks">

Of course, the information about **when** the sites were surveyed is essential!

The survey **date**, **time** and **duration** should also be reported.

As for our previous posts, I have prepared a [printable cheat](https://gbif.box.com/s/xyh5jj0x5yxpyqoveg7wfma75qsylybp) sheet for you.

<img align="center" src="/post/2026-09-07-sam-survey-where-when/sam_survey_where_when.png" alt="Sam's cheatsheet - when & where">

In a **Darwin Core Archive** (DwC-A), modelling the survey site information requires a combination of event core and Humbolt extension fields. Please see [this chapter](https://docs.gbif.org/guide-publishing-survey-data/en/#survey-site) in Kate’s guide for more details.

In the **event** table, you will be able to map the following information:

* **Locality**: [locationID](https://dwc.tdwg.org/terms/#dwc:locationID), [countryCode](https://dwc.tdwg.org/terms/#dwc:countryCode), coordinates ([decimalLatitude](https://dwc.tdwg.org/terms/#dwc:decimalLatitude), [decimalLongitude](https://dwc.tdwg.org/terms/#dwc:decimalLongitude), [geodeticDatum](https://dwc.tdwg.org/terms/#dwc:geodeticDatum)), [locality](https://dwc.tdwg.org/terms/#dwc:locality) (you can enrich the information such as the geometry of the site with the [location-related terms](https://dwc.tdwg.org/terms/#location)).
*	**Habitat**: [habitat](https://dwc.tdwg.org/terms/#dwc:habitat)
*	**Event date and time**: [eventDate](https://rs.tdwg.org/dwc/terms/eventDate) and [eventTime](https://dwc.tdwg.org/list/#dwc_eventTime)

In the **Humboldt extension** table, you can map the following:
*	**Weather**: [reportedWeather](https://eco.tdwg.org/list/#eco_reportedWeather)
*	**Extreme conditions**: [reportedExtremeConditions](https://rs.tdwg.org/eco/terms/reportedExtremeConditions)
*	**Geospatial scope**: [geospatialScopeAreaValue](https://rs.tdwg.org/eco/terms/geospatialScopeAreaValue) and [geospatialScopeAreaUnit](https://rs.tdwg.org/eco/terms/geospatialScopeAreaUnit)
*	**Total area sampled**: [totalAreaSampledValue](https://rs.tdwg.org/eco/terms/totalAreaSampledValue) and [totalAreaSampledUnit](https://rs.tdwg.org/eco/terms/totalAreaSampledUnit)
*	**Event duration**: [eventDurationValue](https://rs.tdwg.org/eco/terms/eventDurationValue) and [eventDurationUnit](https://rs.tdwg.org/eco/terms/eventDurationUnit)

If you intend to report vegetation cover, you can do so with the [verbatimSiteDescriptions](https://rs.tdwg.org/eco/terms/verbatimSiteDescriptions) term in the Humboldt extension or in another extension such as [measurmentOrFacts](https://rs.gbif.org/extension/dwc/measurements_or_facts_2025-07-10.xml) or [relevé](https://docs.gbif.org/guide-publishing-survey-data/en/#relevé-extension). See [this chapter](https://docs.gbif.org/guide-publishing-survey-data/en/#vegetation-cover) in Kate’s guide for more information.

In the **Darwin Core Data Package** (DwC-DP), you will also need at least two tables: the [**event**](https://gbif.github.io/dwc-dp/qrg/#Event) and the [**survey**](https://gbif.github.io/dwc-dp/qrg/#Survey) table. In this instance, the mapping is the same as with DwC-A as the event tables in both models contain the same terms and the survey table includes the Humbolt extension terms.

Kate modelled Sam’s data for us both in DwC-A and DwC-DP, thank you Kate!

Here is how the Survey Island sites were modelled in DwC-A:

<img align="center" src="/post/2026-09-07-sam-survey-where-when/sites_in_DwCA.png" alt="Locations in DwC-A">
(See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-09-07-sam-survey-where-when/sites_in_DwCA.png))

Here is how the Survey sites were modelled in DwC-DP:

<img align="center" src="/post/2026-09-07-sam-survey-where-when/sites_in_DwCDP.png" alt="Locations in DwC-DP">
(See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-09-07-sam-survey-where-when/sites_in_DwCDP.png))

And here is how is the date and duration part of the data both in DwC-A and DwC-DP:

<img align="center" src="/post/2026-09-07-sam-survey-where-when/date_in_DwCA_and_DwCDP.png" alt="Dates and time in DwC-A and DwC-DP">
(See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-09-07-sam-survey-where-when/date_in_DwCA_and_DwCDP.png))

The next installment in the series will be about survey protocols, I hope you are looking forward to it!
