---
title: Good SAMaritans – How to model survey targets in Survey and Monitoring (SAM) datasets
author: Marie Grosjean and Kate Ingenloff 
date: '2026-08-13'
slug: sam-survey-scope
categories:
  - GBIF
tags:
  - SAM
  - Humboldt
  - DwC-DP
  - Darwin Core
  - DwC-A
  - publish
lastmod: '2026-08-13'
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

Welcome to the second installment of our series on publishing Survey and Monitoring (“SAM”) data to GBIF where I’m simultaneously learning how to do it and teaching you at the same time. I am still supervised by [**Kate Ingenloff**](https://orcid.org/0000-0001-5942-9053), our survey data mapping expert.

If you haven’t already, you can read our first post: https://data-blog.gbif.org/post/sam-introduction/ in which I introduce our mascot, Sam the Secretary bird. If you prefer learning the art of publishing SAM data without fluffy writing and doodles, I recommend reading Kate’s comprehensive guide here: https://doi.org/10.35035/doc-ynvs-eh84.

Today, we will talk about survey scopes and targets and how to model them as a Darwin Core Archive (DwC-A) and a Darwin Core Data Package (DwC-DP).

I am not a field ecologist. The closest I get to conducting surveys is foraging mushrooms (mostly for frying and soups, please share recipes.) But what I know is that what you look for and what you actually find are often different in the field. Here Sam is illustrating the concept for us:

Today, we will talk about **survey scopes and targets** and how to model them as a Darwin Core Archive (DwC-A) and a Darwin Core Data Package (DwC-DP).

I am not a field ecologist. The closest I get to conducting surveys is foraging mushrooms (mostly for frying and soups, please share recipes.) But what I know is that what you look for and what you actually find are often different in the field. Here Sam is illustrating the concept for us:


<img align="center" src="/post/2026-08-13-sam-survey-scope/expectationVSreality.PNG" alt="Field work's expectations vs reality">

“What you look for” in Darwin Core language can translate to either “survey scope” or “survey target” depending on the model you choose to work with. We will come back to the terms for each model later. For now, I will call it “survey scope” (as this matches wording in Kate’s guide).

> 	Reporting survey scope (when there is one) is as important as reporting what was found during a survey. Without that information, it is impossible to infer absences or to even assess whether absences can be inferred.

Now, what exactly do you need to report when it comes to survey scope and how?

First, you need to think about what is **included** versus **excluded** in the survey. 

Let’s continue with the example of Sam’s butterfly survey introduced in the first post (here is a flashback for context).
<img align="center" src="/post/2026-07-15-sam-intoduction/sam_doom_machine.PNG" alt="Second sampling protocol"/>

Sam was looking for two species of butterfly: this is the **taxonomic scope** of the survey. After an initial failed attempt at using a net, Sam acquired a portable doomsday vacuum (called a LepiPro Proton Pack) to catch flying insects. With that protocol, Sam didn’t specifically exclude any species from the survey.

What Sam did exclude from his survey were eggs, caterpillars and chrysalids. Sam only caught imago specimens (the adults). Which brings us to our second category of scope, **organismal scope**, which entails

•	The **life stage** of an organism (like Sam only looking for imago specimens)
•	The **growth form** of an organism (surveying shrubs in a forest for example)
•	The **degree of establishment** of an organism (like only surveying native species).

<img align="center" src="/post/2026-08-13-sam-survey-scope/no_caterpillar.PNG" alt="No caterpillar" width="300"/>

You don’t know because I haven’t told you yet, but Sam also excluded agricultural lands from the survey. This last bit of information can be categorized as **habitat scope**.

> 	Note, survey scopes or targets should be clearly identified before the survey is conducted.

Sam, with the LepiPro Proton Pack, caught many other species too: some mosquitoes, horseflies, midges, hoverflies, dragonflies, etc. These are called **bycatch**; they are specimens or occurrences that weren’t part of the survey scope and were captured or observed during the survey. If Sam were to capture one of his target butterfly species as a chrysalid, this could also be reported as bycatch as it is outside the organismal scope targeted for sampling.

Reporting bycatch can be helpful (in this case, I am sure the disease vector community would be quite happy) but they must be reported as such. The absence of these taxa cannot be inferred in the context of the survey.

<img align="center" src="/post/2026-08-13-sam-survey-scope/bycatch.png" alt="Bycatch" width="300"/>

Once again, I have prepared a printable cheat sheet for you which you can download [here](https://gbif.box.com/s/cx43pof5orsipz862thty8p6bmpxjc4q) as well as an example filled with Sam’s data.

<img src="/post/2026-08-13-sam-survey-scope/sam_survey_scope.png" alt="Sam's survey scope cheatsheet"/>

In DwC-A, the Humboldt extension has a series of terms you can use for reporting the survey scope (targetTaxonomicScope, excludedTaxonomicScope, etc.). You can refer directly to the Humboldt documentation [here](https://eco.tdwg.org/terms/) or [this chapter](https://docs.gbif.org/guide-publishing-survey-data/en/#scope-and-completeness) in Kate’s guide.

DwC-DP has two dedicated tables (“classes”) for reporting survey scope: [survey target](https://gbif.github.io/dwc-dp/qrg/#Survey%20Target) and [survey target descriptor](https://gbif.github.io/dwc-dp/qrg/#Survey%20Target%20Descriptor) (plus [one table](https://gbif.github.io/dwc-dp/qrg/#Survey%20Survey%20Target) to join the information to the survey).
At this point, looking at the DwC-DP documentation might induce some heart palpitations and panic (those are typical symptoms of data modelling).

<img align="center" src="/post/2026-08-13-sam-survey-scope/panic.png" alt="Panic!" width="300"/>

Luckily, Kate has us covered! She generated two complete examples on how to model Sam’s data with both DwC-A and DwC-DP (you can see it bigger [here](https://github.com/gbif/data-blog/blob/master/content/post/2026-08-13-sam-survey-scope/model_Sam_survey_scope.png)).

<img src="/post/2026-08-13-sam-survey-scope/sam_survey_scope.png" alt="Sam's data in DwC-A and DwC-DP"/>

Don’t miss our next instalment in the Good SAMaritans series! We will talk about how to model and share survey sites, dates and time. I hope you are looking forward!

