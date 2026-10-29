---
title: Good SAMaritans – How to model efforts in Survey and Monitoring (SAM) datasets
author: Marie Grosjean and Kate Ingenloff 
date: '2026-11-16'
slug: sam-survey-effort
categories:
  - GBIF
tags:
  - SAM
  - Humboldt
  - DwC-DP
  - Darwin Core
  - DwC-A
  - publish
  - sampling efforts
  - survey efforts
  - effort
lastmod: '2026-09-07'
keywords: ['Survey And Monitoring', 'Humboldt Extension', 'Data modelling']
description: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: yes
draft: yes
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

Hello old and new friends! Welcome back to our blog post series where you (and I) learn how to format and publish Survey and Monitoring (SAM) data to GBIF. 

Thanks to our SAM data mapping guru, [Kate Ingenloff](https://orcid.org/0000-0001-5942-9053), and our goofy mascot, Sam the secretary bird, I feel like I have leveled up already!

We have had an [introduction to SAM data](https://data-blog.gbif.org/post/sam-introduction/), seen [how to model survey targets](https://data-blog.gbif.org/post/sam-survey-scope/), worked on [space and time](https://data-blog.gbif.org/post/sam-survey-where-when/) and learnt [how to report survey protocols](https://data-blog.gbif.org/post/sam-survey-protocol/).

> **NB**: If you would like more in-depth and formal approach to modelling and publishing SAM data, please check out [Kate’s comprehensive guide](https://docs.gbif.org/guide-publishing-survey-data/en/).

But we have a lot more to learn and how befitting, since today’s topic is about survey effort!

<img align="center" src="/post/2026-11-16-sam-survey-effort/sam_efforts.png" alt="Sam is putting a lot of effort in their work" width="450"/>

Contrary to what a certain sci-fi pop-culture small, green humanoid alien will have you believe, the world isn’t a binary place when it comes to putting in effort.

<img align="center" src="/post/2026-11-16-sam-survey-effort/do_or_do_not.png" alt="Yoda delivering his famous quote" width="500"/>

In survey and monitoring, not only is there “trying” but there are degrees of trying. In fact, reporting how much effort you put into surveying a site is essential for interpreting measures of abundance and completeness.  Not only that, but users need to know about survey efforts to decide if a dataset is fit for purpose and for comparing information across studies.

<img align="center" src="/post/2026-11-16-sam-survey-effort/yoda_interrupted.png" alt="Yoda gets interrupted" width="500"/>

Two surveys may follow the same protocol at the same site, but have been completed using different levels of survey effort. For example, in one case you may have one surveyor catching butterflies and, in the other case, ten students.

(Now, there is a risk that the group of students may spontaneously start to work on a choreography for the [Dance You PhD competition](https://www.science.org/content/page/about-annual-science-aaas-dance-your-ph-d-contest). In which case, they might not fare as well as one surveyor alone.)

(But dance parties non-withstanding, chances are that the group of students will catch more butterflies than the lone surveyor. This is important to know when you compare results).

<img align="center" src="/post/2026-11-16-sam-survey-effort/trying_is_the_way.png" alt="Trying is the way">

Before we get into the specific fields for reporting sampling and survey efforts, I want to highlight the other concepts to keep in mind.

The first and obvious one is that survey effort is always dependent on a protocol. In fact, the protocol tells us a lot about the effort put into a survey, for example, whether baits are used, size of the nets, the time allocated for sampling, the number of pizza breaks, etc.

Protocol description and sampling effort work in synergy, like bread and cheese.

<img align="center" src="/post/2026-11-16-sam-survey-effort/perfect_synergy.png" alt="The perfect synergy" width="300"/>

Another aspect of effort is the **size** of what you are surveying. Depending on the type of surveys, this might correspond to the length of a transect or the size of an area or even a volume filtered. Similarly, you should also report the **duration** of the survey event.

Note that there are some specific fields relating to **area** surveyed but you can always use the **sample size** values and unit to convey more complex data.

For example, since Sam is using a special kind of portable vacuum (the LepiPro Proton Pack) to suck in all flying insects, Sam could report the volume of air sucked in as a sample size. This is of course assuming that the flow sensor works properly (they are notoriously unreliable). If they’re not working, Sam might have to leave the quantity fields blank and instead make a comment in the event remarks field about the malfunctioning sensors.

<img align="center" src="/post/2026-11-16-sam-survey-effort/sad_sam.png" alt="Another sensor malfunction" width="300"/>

Finally, if the information is available, we should always give credit to who (or what) put in the effort to make a survey happen. Sam could record their own name and OrcID.

If you need help with modelling protocols and everything relating to survey sites and duration, I suggest checking our previous posts or Kate’s guide mentioned at the start of this post.

When it comes to reporting efforts, you need to specify:

* The **effort** **value** and **units** (for example, the number of traps deployed for a site)
* The **method** used to determine the **survey efforts**, this is an extension of the protocol, but it focuses specifically on the elements relating to effort. You can be very specific here with numbers, durations, etc.
* The** people who performed the survey** and sampling (this gives the credit to people who work in the fields but also helps aggregation and comparison, if the same person performed the sampling, you would expect less variation).

As usual, is [here](https://gbif.box.com/s/a7rqbtdhl2l1utu2rgpltkup43mj8j7y) a downloadable cheat sheet I prepared as well as Sam’s sheet.

<img align="center" src="/post/2026-11-16-sam-survey-effort/sam_survey_how_much.png" alt="Sam's cheatsheet">

In **Darwin Core Archives** (DwC-A), all the sampling efforts can be modelled in the Humboldt extension, see the following terms:

* [samplingEffortValue](https://rs.tdwg.org/eco/terms/samplingEffortValue) and [samplingEffortUnit](https://rs.tdwg.org/eco/terms/samplingEffortUnit)
* [samplingEffortProtocol](https://rs.tdwg.org/eco/terms/samplingEffortProtocol) should be used to report the method of the survey efforts
* [samplingPerformedBy](https://rs.tdwg.org/eco/terms/samplingPerformedBy) should be to report people who performed the survey

But sometimes life gets in the way and there is no time for the Humboldt extension. If that’s your case, I suggest using at least the [samplingEffort](https://dwc.tdwg.org/terms/#dwc:samplingEffort) term in the event table and the [recordedBy](https://dwc.tdwg.org/terms/#dwc:recordedBy) term in the occurrence table.

Once again, the information the **Darwin Core Data Package** (DwC-DP) will be similar. Everything can be included the survey table. The mapping is the same as DwC-A since the [survey](https://gbif.github.io/dwc-dp/qrg/#Survey) table includes the Humbolt extension terms.

> Note that you can decide to make your DwC-DP a bit more complicated (or smart depending on how you look at it) should you wish to.
> You can do so in two ways:
>
> * Adding an [Agent](https://gbif.github.io/dwc-dp/qrg/#Agent) and a [Survey Agent Role](https://gbif.github.io/dwc-dp/qrg/#Survey%20Agent%20Role) tables
> * Adding a [Protocol](https://gbif.github.io/dwc-dp/qrg/#Protocol) table.
>
> 
> If you were to make an Agent table, you would need to list in the table the people that performed the survey (one per row) and link these people to the Survey table by specifying their role in the Survey Agent Role table.
>
> 
> **When would you want to use Agent and Survey Agent Role tables?** I suggest making a separate agent table if you meet one of these criteria:
>
> 
> * You would like to **enrich** the information associated with your agents (surveyors), for example, list everyone together with their ORCIDs
> * You might want to specify **other types of agents** (for example list people or programs involved in identifying the species in the samples)
> * You want to be **specific** about the **role** of each agent in the survey.
> * You really like relational databases and would like to make the modelling as efficient as possible.
> 
> You could also make a protocol table containing the effort protocol. You would need to make one row for each distinct protocol and link the surveys to the protocols via the [samplingEffortProtocol_fk](https://gbif.github.io/dwc-dp/qrg/#Survey__samplingEffortProtocol_fk) field.
>
> **When would you want to use a protocol table?** I suggest making a separate protocol table if you meet one of these criteria:
> * You would like to share multiple types of protocols (for example survey or georeferencing protocols)
> * You would like to share protocol remarks
> * You really like relational databases and would like to make the modelling as efficient as possible.

You can now check Kate’s examples.

Here is the DwC-A version:

<img align="center" src="/post/2026-11-16-sam-survey-effort/effort_in_DwCA.png" alt="Survey efforts in DwC-A">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-11-16-sam-survey-effort/effort_in_DwCA.png).

Here is the DwC-DP version:

<img align="center" src="/post/2026-11-16-sam-survey-effort/effort_in_DwCDP.png" alt="Survey efforts in DwC-DP">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-11-16-sam-survey-effort/effort_in_DwCDP.png).

Join us next time as we will be explaining how to model the physical samples collected during a survey.



