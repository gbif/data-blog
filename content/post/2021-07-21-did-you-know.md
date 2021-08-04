---
title: Did you know that...? - some of the lesser known functionalities around GBIF.org
author: Andrea Hahn
date: '2021-08-01'
slug: did-you-know
categories: 
  - GBIF
tags:
  - gbif
  - release notes
  - custom metrics
  - spatial filtering
  - user feedback
  - GitHub
  - tools
  - species matching tool
  - API
  - ingestion history
  - registry
  - GRSciColl
  - citation
  - DOI
  - CiteTheDOI
  - rollup metrics
  - Global Nodes Meeting 2021
lastmod: '2021-07-30T14:30:00+02:00'
draft: yes
keywords: []
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

During the first-ever virtual GBIF 2021 Global Nodes Meeting, GBIFS hosted a "game show": a one-hour "[battle of Nodes vs. helpdesk](https://gnm2021.gbif.org/sessions/friday-nodes-vs-helpdesk)". The not-so-hidden goal was to demonstrate some of the lesser known functionalities of GBIF.org through a fun, interactive session.

<iframe title="vimeo-player" src="https://player.vimeo.com/video/571521342" width=100% height="300" frameborder="0" allowfullscreen></iframe>

The following is a summary of the questions and answers from this session, plus some extras that did not manage to make it into the time frame of the event. The summary is following the layout and sequence of the interactive hour:

- **Did you know...**: a main question or task to be solved by a sub-group of participants, retreating to a breakout room for a few minutes before presenting the solution to the plenary - or, by presenting an incorrect or no answer, giving Marie as "GBIF helpdesk" the chance to win the point 

and

- **Poll**: a loosely related multiple-choice poll for the remaining participants in the main session room.

> In this summary, the poll questions are marked with (++)<br> for the most typical solution, (+)<br> for possible alternatives, (~) for technically correct, but non-recommended options, and (-)<br> for incorrect responses.


---

### Question 1: Warm-up 

[Session Video](https://player.vimeo.com/video/571521342#t=m7s5)

**Did you know there is a summary of the latest GBIF.org software and infrastructure changes?**  --- Where is it?

> **Suggested answer:**<br>
> [https://www.gbif.org/release-notes](https://www.gbif.org/release-notes)

<div> </div>

>**Poll:**  And where would _you_ start looking for this summary?<br>
><br>
> a) The free-text search, where else? (+) <br>
> b) The link is in the green footer section of all pages in GBIF.org (-)<br>
> c) I'll check the main menu of course! (++)<br>

Notes:<br>

* You find the release notes in the main menu of GBIF.org, under About >> Release notes. The free text search that you find in the header of all GBIF.org pages is another option if you know what you are looking for.

---

### Question 2: Custom metrics

[Session Video](https://player.vimeo.com/video/571521342#t=m12s5)

**Did you know that you can generate some custom data metrics yourself?**  --- Get an overview of the occurrence "issues & flags" for publishers in your country or area, per publisher

> **Suggested answer:**<br>
><br>
> _Step-by-step_: from the [occurrence search](https://www.gbif.org/occurrence/) page,<br>
> 1. filter for the "publishing country or area" in the "advanced" filter set<br>
> 2. in the table of results, switch to the "Metrics" tab<br>
> 3. select the "Custom" setting, and<br>
> 4. set the two relevant dimensions: "Publisher", and "Issues and Flags".<br>

<div> </div>

> **Poll:** What is the best starting point here?<br>
><br>
>	a) the occurrence search page (++)<br>
>	b) "my" country page (-)<br>
>	c) the GBIF API (+)<br>

Notes:<br>

* In the session recording ([video](https://gnm2021.gbif.org/sessions/friday-nodes-vs-helpdesk)), Marie demonstrates the solution using custom metrics generation from around [18:40].
* Alternative possibility suggested during the session [17:25]: Prepare the filter as for a download, and check the download feedback on issue labels.

<small> Similar to using the API to aggregate the same kind of information, the proposed alternative option is a bit more labor intensive for getting an overview. More to the point here though, it is specific to Issues and Flags, rather than other custom dimensions --- but useful to be aware of for this particular case.</small>

---

### Question 3: Spatial filtering

[Session Video](https://player.vimeo.com/video/571521342#t=m21s24)

**Did you know that you can download a species list for an area?** --- How would you generate a species list for the "Azores" island group?

>**Suggested answer:**<br>
><br>
> 1. Start from an [occurrence search](https://www.gbif.org/occurrence/search?occurrence_status=present&q=)<br>
> 2. In the "advanced" filter options, use the "Administrative areas (gadm.org)" filter, and select the entry for "Azores"<br>
_In this particular example, the gadm.org administrative areas filter is possibly the best filter option. But depending on the search interest and geographic area, other possibilities like polygon, bounding box, or drag-dropping a geojson shape file onto the occurrence map may be equally well or better suited_. <br>
> 3. Once the filter is applied, go to the download tab, log in, and select "species list" as the download format<br>
> 4. If you take "species" literally, you will still need to filter the downloaded result for taxonRank=SPECIES, as it includes identifications at all taxonomic ranks (taxon list)

<div> </div>

> **Poll:** How would you go about filtering data on the "Azores"?<br>
> <br>
> a) I draw a bounding box or polygon on a map (++)<br>
> b) I use the administrative areas (gadm.org) filter (++)<br>
>c) I use the country or area filter (-)<br>
> d) none of these! I have a better way! (great! we'd love to hear! does it involve geojson files?) (++)<br>

Notes:<br>

* A search by bounding box or polygon, depending on location and definition, is likely to include a wider area, in this case: the marine area between and around islands. Using the gadm.org filter will limit a search to the land area here.<br>
* If you want to use a geojson shape file as your filter, open the ["map" tab](https://www.gbif.org/occurrence/map) of the occurrence page, and drag-and-drop your shape file into the map.<br>
* Outside of the concrete example (Azores), all options in the poll - including the "country or area" filter - are equally possible and "correct", but fit some queries better than others.<br>

---

### Question 4: User feedback

[Session Video](https://player.vimeo.com/video/571521342#t=m28s35)

**Did you know that you can find GBIF user feedback that relates to a country or organization?** --- Find all feedback that is reported against a country of your choice!

>**Suggested answer:**<br>
><br>
> The GitHub repository that stores user feedback is called [/gbif/portal-feedback](https://github.com/gbif/portal-feedback). This is also where you can find the entry point to the "[issues](https://github.com/gbif/portal-feedback/issues)". The filter is pre-set to only show open issues.<br>
> <br>
> Countries are defined as labels of the ISO country codes – to search for all issues relating to Belgium, for example, add an additional filter as "[label:BE](https://github.com/gbif/portal-feedback/issues?q=is%3Aissue+is%3Aopen+label%3ABE)".

<div></div>

>**Poll:** Is all user feedback stored in GitHub?<br>
><br>
> a) Yes, all user feedback is stored in GitHub (-)<br>
> b) No. Sometimes it is sent to a dataset contact via email (++)<br>
> c) No. Sometimes it is forwarded to the feedback system of another organization (++)<br>
> d) No. Some users prefer to write to helpdesk@gbif.org (++)<br>
	
Notes:<br>

* For a brief demo of the GitHub GBIF feedback repository, also see the recording of "[Live demos: Group 2](https://gnm2021.gbif.org/sessions/tuesday-tools-to-support-nodes) - Tools to support nodes with data mobilization strategies" (from about [1:06:00]).<br>
* The GitHub repository does not document everything that gets reported: some people prefer the non-public helpdesk email address (option d) for communication over the publicly indexed GitHub repository; some issues are automatically redirected to other platforms' feedback systems (option c), provided a technical connection has been established; and if the feedback is on an individual record, the commenter can choose whether to log the issue with GBIF, or rather send an email directly to the dataset contacts (option b).<br>
* **Bonus tidbit**: For issues that relate to occurrence records, the dataset and publisher UUIDs are stored within the issue as well. To find all issues for a given publisher or dataset, add the publisher or dataset UUID as a free text search value, like this (omit the quotation marks): "[is:issue is:open 50c9509d-22c7-4a22-a47d-8c48425ef4a7](https://github.com/gbif/portal-feedback/issues?q=is%3Aissue+is%3Aopen+50c9509d-22c7-4a22-a47d-8c48425ef4a7)". This will find all open issues reported that relate to this publisher or dataset.<br>

---

### Question 5: Scientific name validation
[Session Video](https://player.vimeo.com/video/571521342#t=m35s15)

**Did you know that you can check a list of scientific names against the GBIF Taxonomic Backbone?** --- Demonstrate how to do this!

>**Suggested answer:**<br>
><br>
> Use the ["Species Matching" tool](https://www.gbif.org/tools/species-lookup). You find this from the main menu of GBIF.org, in the "Tools" menu section, under "GBIF Labs". <br>
><br>
> <small> If you want to try the examples that are available on the tool's web page, be aware that dragging the example directly into the drop-circle of the user interface may not work – rather, store the file locally first.</small> 

<div></div>

>**Poll:** What else can you do through GBIF.org?<br>
> <br>
> a) search for species records by a gene sequence (++)<br>
> b) find groups of occurrence records, across datasets, that seem to be somehow related (++) <br>
> c) download a detailed report for a dataset check/validation (DwC-A) (-) <br>
> d) check whether there are any known issues with the systems behind GBIF.org, right now, that could explain e.g. slow data updates (++) 

Notes:<br>

* To use the the Species Matching tool for checking a list of taxon names, create a file following the guidelines at the top of the [tool page](https://www.gbif.org/tools/species-lookup); consult or adapt the example files if you are not sure what the guidelines mean.<br>
* For a **sequence blast** (poll option a): try the [Sequence ID tool](https://www.gbif.org/tools/sequence-id), or [this example](https://www.gbif.org/search?q=TAATCTCTCAACTTTGGCCTTTGGTTTTTTTCTTAACCCAAGGCCTGTAGTTTGGATGGTGGAGGTGTGCTGGCTTGCTCTCCTGTCGGTCAGCTCCTCTGAAAAGAATCAGCAAAGGGAAACGCACCTGTTCCTGCATCTTGTGCTTCTGGCATTGATAACTGTCCATGCCTGTTATGGCTCAGATGCCTTTTGGATCGGTTGAGCGTCTTTGCTTACAGTTGTCCTTTGGACATTTTTGAC). This feature is only available for selected taxon groups.<br>
* To find **clusters** of occurrence records (poll option b), use the advanced occurrence search facet "[Is in cluster](https://www.gbif.org/occurrence/search?advanced=1&is_in_cluster=true)". This is an early feature at this point, still under ongoing development - you can read more [here](https://www.gbif.org/news/4U1dz8LygQvqIywiRIRpAU/new-data-clustering-feature-aims-to-improve-data-quality-and-reveal-cross-dataset-connections). A cluster groups occurrence records that appear to be somehow related. Examples are: siblings of herbarium specimens deposited in other collections; derived units like eDNA data sampled from a specimen; related multimedia collections; duplicates of records published by different institutions; etc. For occurrence records that were tagged as belonging to a cluster, the record page has a second tab labeled "Cluster", like in [this example](https://www.gbif.org/occurrence/865079977/cluster).<br>
* The GBIF.org "**[system health](https://www.gbif.org/health)**" indicator page (poll option d) is accessible from the main menu bar of all pages via the little "pulse" symbol. You find this next to the language setting, free text search and feedback symbols. If the site is not working as expected, or your newly published dataset does not become available as fast as you would expect it to, it is worth checking here whether there are any known issues.<br>
* Sorry - a detailed, _downloadable_ report with data validation results (poll option c) is not available - yet. You can validate your data using the [data validator](https://www.gbif.org/tools/data-validator) tool, but the report is so far only available online. That said, though: the data validator tool is currently under revision, and downloadable reports are one of the features being considered for a future version.<br>

---
### Question 6: The API

[Session Video](https://player.vimeo.com/video/571521342#t=m44s0)

**Did you know that GBIF.org builds on an API, and that you can use that API as well?** --- With your web browser as a client, use the GBIF registry API to find all technical installations of type BIOCASE_INSTALLATION. How many are there?

>**Suggested answer:**<br>
> <br>
> [https://api.gbif.org/v1/installation/?type=BIOCASE_INSTALLATION](https://api.gbif.org/v1/installation/?type=BIOCASE_INSTALLATION).
> At the time of the event, the "count" value near the top of the response reported that there were 81 data publishing software installation endpoints of this type registered to GBIF<br>
> <br>
> <small> A proposed alternative solution, [https://registry.gbif.org/installation/search?q=BioCASE](https://registry.gbif.org/installation/search?q=BioCASE), is the answer to a different question: it returns free text search results, i.e. entries that contain the text somewhere within the record like [here](https://registry.gbif.org/installation/cc9f1c90-72a5-4384-a689-cfb82a71f773), _not_ installations of that type.</small>

<div></div>

>**Poll:** What does "API" stand for?<br>
> <br>
> a) Advanced Programming Interface (~)<br>
> b) Academic Performance Index (~)<br>
> c) Adobe Acrobat Plug-In (~)<br>
> d) Application Programming Interface (++)<br>

Notes:<br>

* The documentation of the GBIF API is here: https://www.gbif.org/developer/summary. You can find the link to this entry page under "API" in the green footer bar on all pages within GBIF.org.<br>
* For more documentation relevant to the answer for this specific quiz question, check the "[Registry](https://www.gbif.org/developer/registry)" section of the documentation, sub-section "Installations".<br>
* All functionality of the user interface is based on the API - but: the API also allows for some options that the user interface cannot support. You can, for example, compose search filters that _exclude_ certain values by using the "not" predicate for an occurrence download. If you want to learn more about options around occurrence data, maybe start by browsing the [Occurrence section](https://www.gbif.org/developer/occurrence) of the API documentation for inspiration.<br>
* And the acronym from the poll? In this context: "Application Programming Interface" would be correct – sometimes also referred to as "Advanced" Programming Interface, though this seems to be a misnomer. Both "Adobe Acrobat Plug-In" (option c) and "Academic Performance Index" (option b) do turn up in abbreviation resolvers under "API" as well, but are not meant here.<br>

---

### Question 7: Ingestion history

[Session Video](https://player.vimeo.com/video/571521342#t=m53s20)

**Did you know that you can check up on the indexing process of a dataset yourself?** --- Since deploying our current generation of data "ingestion" infrastructure, iNaturalist has been crawled over 250 times. How do you find the logs that the infrastructure produced for crawl number 265, if you were asked to diagnose issues in that?

>**Suggested answer:**<br>
> <br>
> _Step-by-step_: <br>
> 1. find the dataset in the [dataset search](https://registry.gbif.org/dataset/search?q=iNaturalist) of the [GBIF registry](http://registry.gbif.org)<br>
> 2. go to the [dataset's registry page](https://registry.gbif.org/dataset/50c9509d-22c7-4a22-a47d-8c48425ef4a7), and 
> 3. check the "Ingestion history" from the sub-menu<br>
> 4. find "Attempt" number 265, and click on "Log"<br>
> This will bring you to the Elastic Search ingestion logs for this particular ingestion process run on that dataset
> <br>
> <small> If this particular example does no longer show the Elastic Search log entries, try a more recent one to get an idea of the options provided by the logs in general<br> 
> <br> Alternative option: a registered dataset contact for the dataset in question, logged in to their GBIF.org user account using the same email address, has access to the logs from the dataset page in a segment "Because you are a trusted contact". The logs accessed from here will be pre-filtered to the most recent ingestion run. Alternatively, the "history" option will enter the same registry page as through the pathway sketched out in the suggested answer</small>

<div></div>

>**Poll:** Which of these components can be helpful for diagnosing issues with a specific dataset?<br>
> <br>
> a) a dataset download (++)<br>
> b) the IPT (+)<br>
> c) the Data Quality page at gbif.org (-)<br>
> d) the GBIF registry (++)<br>
> e) the Dataset metrics tab at gbif.org (+)<br>

Notes:<br>

* Working with ingestion logs is an option that publishing software administrators and Nodes technical staff may be interested in exploring: it can give you more autonomy in diagnosing ingestion issues, for example, if local data update does not appear to be picked up by GBIF. If you are rather a data content curator or a data user, this option is probably not relevant for you.<br>
* The [Data Quality](https://www.gbif.org/data-quality-requirements) page (option c) is static, authored content and knows nothing about specific datasets, so this will not help to diagnose issues as such (though the page does contain some pointers to other components).<br>
* **Dataset downloads** (option a): yes - a download summary page also gives access to the "issues & flags" filter and the yellow "pill" labels that report content remarks and issues. This option is mostly intended for mixed content from many datasets (user downloads), rather than for downloading an individual dataset. You will also find the issues reported with each record within the downloaded data. Given the width of the download file, this is easy to overlook, but well worth being on the lookout for, especially if you still want to apply some additional post-download filtering of data to exclude records with particular issues. <br>
* The **[IPT](https://www.gbif.org/ipt)** (option b) does provide some feedback on formal structure (table relationships) and data content – a good first checkpoint for a new or substantially reworked dataset configuration! Keep in mind though: the IPT is a stand-alone tool, independent from the GBIF ingestion process, and would not "know" why a specific ingestion run failed in a given situation. <br>
* The **registry** (option d), as demonstrated in the response to the main question, gives access to the timeline and the logs of all past crawls and ingestions. A good place to be aware of if you want to know what is going on with a given dataset! <br>
* **Dataset metrics tab** (option e): every dataset page, like the one in our [iNaturalist](https://www.gbif.org/dataset/50c9509d-22c7-4a22-a47d-8c48425ef4a7) example,  has a dataset [metrics tab](https://www.gbif.org/dataset/50c9509d-22c7-4a22-a47d-8c48425ef4a7/metrics). Near the bottom of that page, the issues and flags that were diagnosed in the ingestion process are listed, together with corresponding occurrence record counts. If you would like to learn more about the meaning of issues and flags, we recommend the [GBIF Issues & Flags](https://data-blog.gbif.org/post/issues-and-flags/) post in the GBIF data blog.<br>
	Keep in mind, though, that this is only possible for records that could be ingested at all: records that did not meet certain minimum standards e.g. on mandatory fields, or that got lost to technical configuration challenges, cannot be reported here 
* Now, I am curious - did you miss the GBIF **[data validator](https://www.gbif.org/tools/data-validator)** in this list? Yes! That is certainly another option worth keeping in mind if you want to check a Darwin Core Archive (DwC-A)! We already mentioned this in Question 5, though, so we did not want to bore you...<br>

---

<small> The following questions and polls did not make it into the live session and recording - but we don't want to keep them from you, so here they are: </small>

---

### Question 8: GRSciColl

**Did you know that the "GBIF Registry of Scientific Collections" (GRSciColl) is connected to occurrence records in GBIF.org?** --- Find the GRSciColl entry for the Royal Belgian Institute of Natural Sciences, and check which collection code has most records in their data

>**Suggested answer:**<br>
> <br>
> "arachnofauna" with 291,402 records (status: end of June, 2021)<br>
> <br>
> How to get there, _step-by-step_:<br>
>
> 1. from the main menu at GBIF.org, go to Tools -> Scientific Collections. You now entered the Registry of Scientific Collections, [GRSciColl](https://www.gbif.org/grscicoll)<br>
> 2. go to the tab "Institutions"<br>
> 3. search for the institution name, and <br>
> 4. open that page by clicking on the name in the results list: [Royal Belgian Institute of Natural  Sciences](https://www.gbif.org/grscicoll/institution/c2bfdeef-9c03-435e-8465-c483dadd6995) <br>
> 5. open the [metrics tab](https://www.gbif.org/grscicoll/institution/c2bfdeef-9c03-435e-8465-c483dadd6995/metrics), and <br>
> 6. scroll down to "occurrences per collection code" to pick the entry with the most occurrence records <br>
> **bonus**: clicking on the name or count here will lead you to the occurrence records filtered for this entry <br>

<div></div>

>**Poll:** Imagine you are checking this institution entry in GRSciColl, and you find that you know of more collections that are not listed here. What can you do?<br>
><br>
> a) I can register "metadata only" datasets for this institution (-)<br>
> b) I can ask GBIFS to give me editing rights, so I can add the collections to GRSciColl myself (+)<br>
> c) I can suggest additions through the user interface (++)<br>
> d) I can log a GitHub issue with suggestions (~)

Notes:<br>

* "[Metadata-only datasets](https://www.gbif.org/dataset/search?type=METADATA)" (poll option a) describe datasets (as opposed to physical collections) in GBIF.org, typically without adding any occurrence records - hence the name. Reasons vary why a dataset may be registered with just a description,  at least initially. Regardless, as datasets and physical collections do not necessarily match each other in scope, such dataset registrations will never be automatically translated into collection entries in GRSciColl. <br>
In addition, independent from the above: unless you already have been authorized by the institution in question, you would not be able to register a dataset to GBIF.org under their name.<br>
<br>
The other three poll options all apply, though to varying degree:<br>
* **Editing rights** (option b): we do not grant these to everybody, but if you are an institutional contact, or for example a node manager, that is indeed an option. Requesting editing rights makes best sense if you have many and frequent changes to make, or if you would like to take general curational responsibility within a certain domain of GRSciColl (e.g. for an institution or country). Before you can get going, we will need to give you an introduction. Check with us via [scientific-collections@gbif.org](mailto:scientific-collections@gbif.org) if this has your interest.<br>
* **user interface** (option c): yes! That is an option for any user now, and we would recommend this as the preferred way to log change requests or additions for more casual users. Your suggestion will not be immediately visible, as it will go through a moderation process to ensure proper procedures and discourage spam - but we will keep you updated. Check out the green "[Suggest a Change](https://www.gbif.org/grscicoll/institution/c2bfdeef-9c03-435e-8465-c483dadd6995)" button on Institution, Collection and Contact pages within [GRSciColl](https://www.gbif.org/grscicoll)! To suggest a new collection under an institution, start from any existing collection, "Suggest a Change", and then use the "More" button on the registry "Collection details" page to "Create new collection". <br>
* GitHub issue (option d): well, yes.... technically, you could. The same applies to sending an email. We would rather you didn't though. There are easier and more integrated ways to send your feedback through the user interface option, above. <br>
And if you are considering to work offline with a larger section of the catalogue, please start by contacting us at [scientific-collections@gbif.org](mailto:scientific-collections@gbif.org)<br>


---
### Question 9: Citations

**Did you know that GBIF assigns unique DOIs to downloads of occurrence data, making citing the data easy, and enabling reproducibility and credit towards data publishers?** --- But - what is the best way to cite GBIF occurrence data obtained by other means, where no DOI has been assigned?

>**Suggested answer:**<br>
><br>
> Generate a ["derived dataset" DOI](https://www.gbif.org/derived-dataset/register)<br>
><br>
> requirements:<br>
>
> * a user account at GBIF.org<br>
> * a list (csv file) of the source datasets (GBIF dataset keys) and respective record counts from your otherwise-produced data download<br>
> * public deposition of the dataset you that wish to cite (e.g. in [Zenodo](https://zenodo.org/))<br>
> *  a GBIF registration (DOI) for your derived dataset - you can generate this<br> [here](https://www.gbif.org/derived-dataset/register)<br>
><br>
>
> To learn more, check the "[Derived datasets](https://data-blog.gbif.org/post/derived-datasets/)" post in the GBIF data blog<br>

<div></div>

> **Poll:** Which types of GBIF data downloads _do_ get a DOI generated automatically?<br>
><br>
> a) a species list download for an occurrence search (++)<br>
> b) occurrence records retrieved through the occurrence search API (-)<br>
> c) an occurence download through AWS, Azure or other cloud services (-)<br>
> d) an occurrence download using the rgbif library "occ_download" function (+)<br>

Notes:<br>

* The "[derived dataset DOI](https://data-blog.gbif.org/post/derived-datasets/)" pathway was designed to help exactly in cases like poll options b) and c) where the the integrated generation of download DOIs cannot "catch", unlike with **downloads through the user interface at GBIF.org** (option a).<br>
* GBIF search API (option b):  be aware that this is a bit of a trick question - download DOIs are not available for records retrieved through the [occurrence _search_ API](https://www.gbif.org/developer/occurrence#search). For the [occurrence _download_ API](https://www.gbif.org/developer/occurrence#download) on the other hand, they are. Check the blog post titled ["Search, download, analyze and cite (repeat if necessary)"](https://data-blog.gbif.org/post/search-download-analyze-cite/) for more on data citations.<br> 
* If you use the ```occurrence_download()``` function of the [rgbif](https://docs.ropensci.org/rgbif/articles/rgbif.html) library, the underlying GBIF download API will trigger the generation of a download DOI. The rgbif library also contains a function, ```gbif_citation()```, that will help you properly cite the data downloaded from GBIF through r (option d; also see [here](https://www.gbif.org/tool/81747/rgbif) and [here](https://docs.ropensci.org/rgbif/reference/gbif_citation.html) for instructions on use of the library and citation function). Just as for the GBIF API itself, be aware that this will only work properly for occurrence _downloads_, not for an aggregate of occurrence _search_ results. <br>

---

### Question 10: Rollup metrics

**Did you know that GBIF keeps track of growth of occurrence data over time?** --- Which time periods (years of occurrence) have the the highest numbers of occurrence records in the African region?

>**Suggested answer:**<br>
><br>
> Check the "[Global data trends](https://www.gbif.org/analytics/global)" page, and filter for "Africa". The panel "Records by year of occurrence" suggests that, for the African region, most records available through GBIF.org so far are on occurrences collected or observed in more recent years (from around 2007 onwards), but there is also stronger evidence from about 1986-92.<br>
><br>
>How to get there, _step-by-step_:<br>
>
> 1. from the main menu at GBIF.org, go to Get Data >> Trends<br>
> 2. the filter is set in the header section of the page - in this case, select "[Africa](https://www.gbif.org/analytics/region/AFRICA)"<br>
> 3. scroll down the page to find the panel "Records by year of occurrence" (orange color, under "Time and seasonality"). To view the [graphic](https://analytics-files.gbif.org/gbifRegion/AFRICA/about/figure/occ_yearCollected.svg) in more detail, click on the image<br>
> 4. this graphic shows counts of available occurrence records within GBIF.org by year of collection/observation since 1950. Four different snapshots of the data availability status within the GBIF index are combined here. The most recent one is found at the bottom; the ones above show the status of data mobilization (not: species occurrence) in a time sequence<br>
> 5. to answer the question, you want to refer to the bottom graph, representing the most recent snapshot - presently, this shows the status of data mobilization at July 1, 2021<br>
> 

<div> </div>

>**Poll:** What other metrics can you find on GBIF.org?<br>
><br>
>a) metrics on downloads made by users from a country (++)<br>
b) the number of registered GBIF users per country (-)<br>
c) an activity report for a country, territory or area (++)<br>
d) changes in popularity of data publishing tools and protocols over time (-)<br>

Notes:<br>

* **Downloads made by users from a country** (option a): basic counts on data download events and data volumes, per year and month and/or registered user country, are part of the regular analytics generation process. These reports are also available as [csv files](https://analytics-files.gbif.org/download/csv/).  This may be of interest to you if, e.g. as a node manager, you would like to create your own metrics or graphics based on the raw data<br>
	- The analytics tables only contain aggregated counts on downloads; information on individual downloads or users is not available.<br>
	- Since this is a data area that is not of public interest, it is rather hidden and not advertised on GBIF.org.<br>
* We do not publish counts, location, or any other information on registered users (option b)<br>
* **Activity report** (option c): yearly generated activity reports are linked from all "country or area" pages, like in [this example](https://www.gbif.org/country/CR/summary). <br>
	- incidentally: if you truncate the URL of such an [activity report](https://analytics-files.gbif.org/country/CR/GBIF_CountryReport_CR.pdf), you can also find your way to the underlying analytics tables (see option a)<br>
* While this may have some historic interest, we do not maintain metrics on publishing tools and protocols that were used for data publication over time (option d). Old data snapshots representing past stages do exist; they are not readily available for access though<br>

---

### Congratulations! 

You reached the end of the extended Quiz! How did it go - did you learn anything new? In our live event at the 2021 Global Nodes Meeting, GBIF Nodes representatives did very well indeed, and won the race! <br>
<br>
Whether you found something that you did not know before, or could confirm that you are fully up-to-date with GBIF systems: I hope that you had some fun, and that we could pique your curiosity to keep exploring GBIF.org and beyond!<br>
<br>
And, before closing:<br>
Special thanks for their contributions to the live event go to:<br>

* Marie Grosjean, who defended the GBIFS helpdesk valiantly in the "battle" in spite of here game-rules inflicted handicap,<br>
* Andrew Rodrigues, guiding the session with aplomb in his role as Official Battle Adjudicator and game show host,<br>

and, last not least:<b>

* the opposing team of combatants from our wonderful, engaged, and skillful community of GBIF Nodes! Thank you all for playing!
