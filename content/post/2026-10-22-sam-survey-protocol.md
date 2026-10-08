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




