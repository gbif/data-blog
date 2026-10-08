---
title: Good SAMaritans – How to model sampling protocols in Survey and Monitoring (SAM) datasets
author: Marie Grosjean and Kate Ingenloff 
date: '2026-10-22'
slug: sam-survey-protocol
categories:
  - GBIF
tags:
  - SAM
  - Humboldt
  - DwC-DP
  - Darwin Core
  - DwC-A
  - publish
  - protocols
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

Welcome back to our series of post where [Kate Ingenloff](https://orcid.org/0000-0001-5942-9053) and I teach you how to model and share Survey and Monitoring (SAM) data using Darwin Core with the goal of publishing the data to GBIF.

So far, we have published three posts in the series: [an introduction to SAM data](https://data-blog.gbif.org/post/sam-introduction/), [how to model survey targets](https://data-blog.gbif.org/post/sam-survey-scope/), and [how to model survey location and time](https://data-blog.gbif.org/post/sam-survey-where-when/).
We try to make this series fun and entertaining (mostly thanks to our mascot, Sam the Secretary Bird). If you prefer a more direct approach to the topic, please check [Kate’s comprehensive guide on publishing SAM data](https://docs.gbif.org/guide-publishing-survey-data/en/).

At the time this post is published, we are in October. I love October!

Where I live, I can forage mushrooms, make soups and pretend winter doesn’t exist yet. But one thing I like even more is Halloween!
I am not American, but I want an excuse to draw pumpkins and bats.

So, I have decided that today will be a little bit spooky when talking about… survey protocols!

<img align="center" src="/post/2026-10-22-sam-survey-protocol/spooky_protocol_feet.png" alt="Sam being spooky">

And to be honest, bad protocols can be a little bit scary. Let me tell you this tale:

<img align="center" src="/post/2026-10-22-sam-survey-protocol/tale_survey_island.png" alt="Spooky tale title">

> There was once a young bird named Sam who wanted to map two species of butterflies on a little isolated island. Sam selected five locations on the Island and set out to catch the winged creatures.
> 
> The adventure started on a beautiful summer day, but little did Sam know that it was doomed to fail. For, you see, Sam’s work was cursed.
> 
> It was a curse with many facets: budgetary restrictions, lack of planning, absent supervisor… and it manifested as a Dreadful Protocol. Such protocols are known to haunt students all over the globe and Sam was one the victims.
> 
> Sam was given a small net and five minutes to survey 1000 m2.
> 
> At the first site he visited, Sam ran and jumped but the sun was burning and not one butterfly was caught. Sam, in a fit of rage, cast the foul net away and, against all odds, managed to get better equipment and protocol. Four more sites were surveyed successfully.
> 
> But, when Sam came to the initial location, intending to complete the project before early autumn, a snowy blizzard swept across Survey Island. To this day, no one knows if the butterflies inhabit this part of the Island…

<img align="center" src="/post/2026-09-07-sam-survey-where-when/discarded_net.png" alt="The cursed net" width="300"/>

I wish I could write the entire post as a spooky tale but alas, I don’t have the talent needed. So, I will go back to my regular prose.

In the case of Sam’s example, we have two protocols: a terrible one and a better one. The better protocol consisted of Sam using a portable vacuum called the LepiPro Proton Pack 2025 to sample each square kilometer (about 10,763 square feet) three times within two eight hour days.

<img align="center" src="/post/2026-10-22-sam-survey-protocol/two_protocols.png" alt="The two protocols">

Both protocols are worth reporting as they allow future data users to evaluate whether the target species absence can be inferred.

It’s also a good idea to document protocols that don’t work. Just in case others might be tempted to try them.

<img align="center" src="/post/2026-08-13-sam-survey-scope/bycatch.png" alt="Bycatch" width="300"/>

Sharing **protocol** information means sharing the following when possible:

* The **name** of the **protocol** and its **description** (this is the most important, it will allow users to understand what you have been doing).
* The **references** to the methods used (allowing users to obtain more details and even replicate the methods).
* Whether **material** was collected and if so, what type of materials were collected, whether the samples/specimens collected are **vouchered** and, if they are, the **institutions** holding them.
* Whether **absences** are reported and if they are, which **taxa** are absent.
* Whether **abundances** are reported and if they are, whether the numbers reported are capped.


<img align="center" src="/post/2026-10-22-sam-survey-protocol/crow_stealing_samples.png" alt="Sample thief" width="400"/>

All of this is helpful when aggregating data from different datasets, when trying to reproduce results or when assessing whether absences can be inferred. Even just reporting the protocol helps, you don’t have to make everything perfect.

As for our previous posts, I have prepared a [printable cheat sheet](https://gbif.box.com/s/n8gqurn48r31qlj9ovmikk945ks4iv3p) for you. This is the version filled with Sam’s data:

<img align="center" src="/post/2026-10-22-sam-survey-protocol/sam_survey_how.png" alt="Sam cheatsheet on protocols">

In **Darwin Core Archives** (DwC-A), all the protocol information can be modelled in the Humboldt extension. But if you aren’t using the Humbolt extension, you should at least share the protocol in the [samplingProtocol](https://rs.tdwg.org/dwc/terms/samplingProtocol) term of the event table.

For all the other terms, see the following mapping in the **Humboldt extension**:

* The **protocol names**, **descriptions** and **references** should be in the [protocolNames](https://rs.tdwg.org/eco/terms/protocolNames), [protocolDescriptions](https://rs.tdwg.org/eco/terms/protocolDescriptions) and [protocolReferences](https://rs.tdwg.org/eco/terms/protocolReferences) respectively.
* The **material** and **voucher** information should be captured in the [hasMaterialSamples](https://rs.tdwg.org/eco/terms/hasMaterialSamples), [hasVouchers](https://rs.tdwg.org/eco/terms/hasVouchers) and [voucherInstitutions](https://rs.tdwg.org/eco/terms/voucherInstitutions) fields. See also this part of [Kate’s guide](https://docs.gbif.org/guide-publishing-survey-data/en/#material-samples).
* Information about **absence** and **abundance reporting** should be in [isAbsenceReported](https://rs.tdwg.org/eco/terms/isAbsenceReported), [absentTaxa](https://rs.tdwg.org/eco/terms/absentTaxa), [isAbundanceReported](https://rs.tdwg.org/eco/terms/isAbundanceReported) and [isAbundanceCapReported](https://rs.tdwg.org/eco/terms/isAbundanceCapReported) fields. See also [this part](https://docs.gbif.org/guide-publishing-survey-data/en/#absences) of Kate’s guide.


<img align="center" src="/post/2026-10-22-sam-survey-protocol/spider_eating_samples.png" alt="Sample glutton" width="300"/>

Modelling the information the **Darwin Core Data Package** (DwC-DP) will be similar. Everything can be included the survey table. The mapping is the same as DwC-A since the [survey](https://gbif.github.io/dwc-dp/qrg/#Survey) table includes the Humbolt extension terms.

> Note that DwC-DP also offers the possibility to model **protocol separately** from the survey table. You can report the protocol name, description, and references in a protocol table and link the protocol(s) to each survey using a join table called survey protocol.
> The advantage is that you won’t have to repeat the information for each line of your survey table. Furthermore, the protocol table allows you to capture other types of protocols such as georeferencing protocols.
> 
> **I suggest making a separate protocol table if you fulfil one or more of these points**:
> 
> * You would like to share multiple types of protocols
> * You would like to share protocol remarks
> * You really like relational databases and would like to make the modelling as efficient as possible.

With that in mind, please check how Kate modelled Sam’s data below:

See the **DwC-A model**, for the sake of readability, we decided to show you the Humboldt table split in two (the main protocol terms here and the extended ones below). Note that all the Humboldt terms should in reality be in one table.

<img align="center" src="/post/2026-10-22-sam-survey-protocol/protocol_in_DwCA_min.png" alt="Model main protocol terms - DwC-A">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-10-22-sam-survey-protocol/protocol_in_DwCA_min.png).

Here are the extended protocol terms:

<img align="center" src="/post/2026-10-22-sam-survey-protocol/protocol_in_DwCA_extended.png" alt="Model extended protocol terms - DwC-A">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-10-22-sam-survey-protocol/protocol_in_DwCA_extended.png).

Similarly, in the DwC-DP model, we chose to show you the survey table split in two:
<img align="center" src="/post/2026-10-22-sam-survey-protocol/protocol_in_DwCDP_min.png" alt="Model main protocol terms - DwC-DP">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-10-22-sam-survey-protocol/protocol_in_DwCDP_min.png).

Here are the extended protocol terms in the survey table:

<img align="center" src="/post/2026-10-22-sam-survey-protocol/protocol_in_DwCDP_extended.png" alt="Model extended protocol terms - DwC-DP">

See it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-10-22-sam-survey-protocol/protocol_in_DwCDP_extended.png).

Please join us next time as we talk about how model efforts!

