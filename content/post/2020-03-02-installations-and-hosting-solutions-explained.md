---
title: Which tools can I use to share my data on GBIF?
author: Marie Grosjean
date: '2020-03-02'
slug: installations-and-hosting-solutions-explained
categories:
  - GBIF
  - Publishing
tags:
  - publish
  - GBIF
  - installation
  - IPT
  - symbiota
  - API
  - hosting
  - earthcape
  - BioCASe
lastmod: '2020-02-20'
draft: no
keywords: ['installation', 'hosting', 'IPT', 'API', 'BioCASe', 'publishing', 'GBIF']
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

As you probably already know, [GBIF.org](https://www.gbif.org) doesn't host any data. The system relies on each data provider making their data available online in a GBIF-supported format. It also relies on organization letting GBIF know where to find these data (in other words registering the data).
But how to do just that?

The good news is that there are several GBIF-compatible systems. They will export or make available the data for you in the correct format and several provide means to register them as datasets on GBIF. 

In this blogpost, I will try to give a very brief overview of these tools and how they work with GBIF.

> If you are new to GBIF you might want to read our [Quick guide to publishing data through GBIF.org](https://www.gbif.org/publishing-data) first. In addition to that, I would also suggest that you take a look at our documentation concerning [data hosting](https://www.gbif.org/article/4qfLORxmM8kYOIwSYSMc2M/data-hosting).


# Types of entities on GBIF: datasets, organizations, nodes, installations

Before I start listing the various types of GBIF-compatible portals and systems, I need to explain some of the GBIF vocabulary.

We have four kind of entities on GBIF (or four kinds of web pages if you prefer):
* **Datasets** (for example: the [Prairie Fen Biodiversity Project](https://www.gbif.org/dataset/72c4d3c6-5b8d-49f5-bfbe-febd53849588)).
* These datasets are published by an **organization** - also called publisher (for example: the [Central Michigan University Herbarium](https://www.gbif.org/publisher/a353a5fa-6426-4ae2-907d-a9bc1639eec5)).
* Each organization is endorsed by a **node** (for example: the [U.S. Geological Survey](https://www.gbif.org/country/US/summary)) or a committee (for organizations in non-participating countries). Endorsement not only ensures suitable data is shared, but often helps make connections between people in the network who can assist each other.
* These datasets are also hosted by an **installation** (for example: [IPT - Hosted by iDigBio](https://www.gbif.org/participant/375)). An installation is attached to an organization. The type of an installation will depend on the type of publishing tool/portal/system you choose.

I tried to summarize all of this in the schema below:
![Schema one organization](https://github.com/gbif/data-blog/blob/master/content/post/2020-02-20-installations-and-hosting-solutions-explained/schema_entities_one_organization.jpeg)

But of course there are some variants to schema. The organization owning the data doesn't have to be the same as the organization hosting the data. For example, the [Prairie Fen Biodiversity Project](https://www.gbif.org/dataset/72c4d3c6-5b8d-49f5-bfbe-febd53849588) is published by the [Central Michigan University Herbarium](https://www.gbif.org/publisher/a353a5fa-6426-4ae2-907d-a9bc1639eec5) but hosted by [an installation](https://www.gbif.org/installation/ac2ad091-91eb-482d-a73f-061744da5dbf) attached to the [Arizona State University Biodiversity Knowledge Integration Center](https://www.gbif.org/publisher/96710dc8-fecb-440d-ae3e-c34ae8a9616f).

In that case, we will differentiate **Publishing Organization** and **Hosting organization** (see the schema below). Both organizations can also be endorsed by different nodes.

![Schema two organizations](https://github.com/gbif/data-blog/blob/master/content/post/2020-02-20-installations-and-hosting-solutions-explained/schema_entities_two_organizations.jpeg)

All of these entities are registered on GBIF. Amongst other things, this structure aims to represent everyone's contribution to the data and allow us to aggregate statistics at different levels.


# IPT: The Integrated Publishing Toolkit

The [IPT](https://www.gbif.org/ipt) is the tool developed and maintained by the GBIF secretariat. It is the most common type of installation connected to GBIF and provides the repository network for the marine community [OBIS](https://obis.org/).
IPTs can generate [Darwin Core Archives](https://dwc.tdwg.org/text/) for each dataset and register them on GBIF.

> The IPT was designed to share data on GBIF, it is not a data management system. However, some actual data management systems can export data to IPTs. This is the case for [Specify](https://www.sustain.specifysoftware.org), a software to manage specimen data for biological research collections.

## Setting up hosting organization

When setting up an IPT, you will be asked to associate it with a (hosting) organization.

## Setting up publishing organizations

1. When an organization is endorsed, helpdesk@gbif.org will send to the organization technical contact(s) an IPT token associated with this organization (also called "organization password").
2. The token will be needed to add the organization to an IPT. Only a IPT administrator can add an organization to an IPT. There is no limit on the number of organization registered in an IPT.
3. IPT users can then select the relevant publishing organization when creating a publishing a new dataset.

For more information, check out the [IPT documentation](https://github.com/gbif/ipt/wiki/IPT2ManualNotes.wiki).


# API-based systems

What I call "API-based systems" are tools/portals that rely on the [GBIF registry API](https://www.gbif.org/developer/registry) to share data on GBIF. Most of these tools are data-management systems. They usually have many features and being GBIF-compatible is only one of them. Here are some of these tools (in no particular order):
* [Earthcape](https://earthcape.com): Biodiversity database platform.
* [Symbiota](http://symbiota.org/docs/): Open source content management system for curating specimen- and observation-based biodiversity data.
* The [Living Atlases](https://living-atlases.gbif.org): Open source IT infrastructure for the aggregation and delivery of biodiversity data.
* Others: some organizations such as the [Naturalis Biodiversity Center](https://www.gbif.org/publisher/396d5f30-dea9-11db-8ab4-b8a03c50a862) also made their own system GBIF-compatible by using our API.

## Setting up hosting and publishing organizations

These systems generate and host [Darwin Core Archives](https://dwc.tdwg.org/text/) and register their endpoints on GBIF using the API. Because they use the registry API, all of them are associated with an API user account (= API username). This API username can be given permissions to register and update datasets on behalf of an organization or a node.

If a username is given permissions to register and update datasets for a node, all the datasets from the organizations endorsed by this Node are concerned (this is the case for the [NBN Atlas](https://nbnatlas.org) for example). However, in most cases, the permissions are set for one organization at the time.

The GBIF Secretariat handle these permissions. This means that organizations have to send an email to heldpesk@gbif.org to request that a specific API username is given the permission to register and update their datasets.

I tried to summarize all of this below:

![API-based installation](https://github.com/gbif/data-blog/blob/master/content/post/2020-02-20-installations-and-hosting-solutions-explained/schema_API_installation.jpeg)


# Other systems

Some systems expose databases online through open web protocols, which require setting up some specific wrappers. As for the API-based systems, being GBIF-compatible is only one of their features. Here are some of these tools:
* [BioCASe](https://www.biocase.org/index.shtml) (BioCASe and BioCASe Provider Software are more than that, see the next paragraph for some explanations)
* [DiGIR](http://digir.sourceforge.net) (still GBIF-compatible but has no active support)
* [TAPIR](https://www.tdwg.org/standards/tapir/) (still GBIF-compatible but has no active support)

## Setting up hosting and publishing organizations

In most cases, these protocols don't generate Darwin Core Archives. Instead, the GBIF system uses the defined protocol to query the data page by page and parse the result. For example [this](http://ww3.bgbm.org/biocase/pywrapper.cgi?dsa=BoBo&request=%3C%3Fxml+version%3D%271.0%27+encoding%3D%27UTF-8%27%3F%3E%0A%3Crequest+xmlns%3D%27http%3A%2F%2Fwww.biocase.org%2Fschemas%2Fprotocol%2F1.3%27%0A+++++++++xmlns%3Axsi%3D%27http%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchema-instance%27%0A+++++++++xsi%3AschemaLocation%3D%27http%3A%2F%2Fwww.biocase.org%2Fschemas%2Fprotocol%2F1.3+http%3A%2F%2Fwww.bgbm.org%2Fbiodivinf%2FSchema%2Fprotocol_1_3.xsd%27%3E%0A++%3Cheader%3E%0A++++%3Ctype%3Esearch%3C%2Ftype%3E%0A++%3C%2Fheader%3E%0A++%3Csearch%3E%0A++++%3CrequestFormat%3Ehttp%3A%2F%2Fwww.tdwg.org%2Fschemas%2Fabcd%2F2.06%3C%2FrequestFormat%3E%0A++++%3CresponseFormat+start%3D%220%22+limit%3D%221000%22%3Ehttp%3A%2F%2Fwww.tdwg.org%2Fschemas%2Fabcd%2F2.06%3C%2FresponseFormat%3E%0A++++%3Cfilter%3E%0A++++++%3Cand%3E%0A++++++++%3Cequals+path%3D%22%2FDataSets%2FDataSet%2FMetadata%2FDescription%2FRepresentation%2FTitle%22%3EBoBO+-+Botanic+Garden+and+Botanical+Museum+Berlin+Observations%3C%2Fequals%3E%0A++++++++%3Cand%3E%0A++++++++++%3CgreaterThanOrEquals+path%3D%22%2FDataSets%2FDataSet%2FUnits%2FUnit%2FIdentifications%2FIdentification%2FResult%2FTaxonIdentified%2FScientificName%2FFullScientificNameString%22%3EZaa%3C%2FgreaterThanOrEquals%3E%0A++++++++%3C%2Fand%3E%0A++++++%3C%2Fand%3E%0A++++%3C%2Ffilter%3E%0A++++%3Ccount%3Efalse%3C%2Fcount%3E%0A++%3C%2Fsearch%3E%0A%3C%2Frequest%3E%0A) is the result you get from querying the [BoBO - Botanic Garden and Botanical Museum Berlin Observations](https://www.gbif.org/dataset/e1beb83c-9b83-4d17-ac5e-d24e09507ec5) (scientific name >= 'Zaa'). This works pretty well for small datasets but can sometimes give inconsistent results for very large datasets as the ingestion process is much longer (days) and is more likely to be interrupted.

> Although the BioCASe the protocol does not generate Darwin Core Archives, the tool BioCASe Provider Software can be configured to expose ABCD-A (also called "BioCASe XML Archive") and Darwin Core Archive and GBIF support both formats. In other words, data providers can choose how to share their data on GBIF (either with the BioCASe protocol or in an archive). Check out the [BioCASe Provider Software documentation](https://www.biocase.org/products/provider_software/index.shtml) for more information.

In any case, the registration of these systems in GBIF is handled by the GBIF Secretariat.

This means that if you set up a BioCAse installation for example, you need to write to helpdesk@gbif.org and we will register the installation endpoint with the correct hosting organization.
Afterwards, we will launch the "synchronization" of this installation. Depending on the way the installation and wrappers are configures, this will create the corresponding datasets on GBIF under the same organization.
If the hosting organization differs from the publishing organization, you will have to notify helpdesk@gbif.org and we will make the update on our side manually.


# To go further

Here is a little summary of all the tools I link in this post:
* [IPT](https://www.gbif.org/ipt)
* [Specify](https://www.sustain.specifysoftware.org)
* [Earthcape](https://earthcape.com)
* [Symbiota](http://symbiota.org/docs/)
* [Living Atlases](https://living-atlases.gbif.org)
* [BioCASe](https://www.biocase.org/index.shtml)
* [DiGIR](http://digir.sourceforge.net)
* [TAPIR](https://www.tdwg.org/standards/tapir/)

Please let me know if you know if I forgot to include any GBIF-compatible system in this post.

Should we make an other blogpost to compare these tools? If so, which criteria would you like to see represented?
