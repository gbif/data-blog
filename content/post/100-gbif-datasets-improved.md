---
title: 100 GBIF datasets, improved
author: Robert Mesibov
date: '2021-07-13'
slug: 100-gbif-datasets-improved
categories:
  - GBIF
  - Guest Post
tags:
  - '# data quality'
lastmod: '2021-07-13T14:50:25+02:00'
draft: yes
keywords: []
description: ''
authors: ''
comment: no
toc: no
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


Biodiversity data papers are a special kind of scientific publication. As explained by [Chavan and Penev (2011)] [1]:

>A data paper is a journal publication whose primary purpose is to describe data, rather than to report a research investigation. As such, it contains facts about data, not hypotheses and arguments in support of those hypotheses based on data, as found in a conventional research article. Its purposes are threefold: to provide a citable journal publication that brings scholarly credit to data publishers; to describe the data in a structured human-readable form; and to bring the existence of the data to the attention of the scholarly community.

In recent years many publishers of GBIF data have also published data papers describing their GBIF datasets. If the data paper manuscript was submitted to a Pensoft journal, the dataset was carefully audited ([Penev et al. 2019] [2]). Manuscripts were not passed to reviewers until dataset errors in structure, format or compliance with Darwin Core recommendations were corrected.

<!--more-->

As of 2021-07-13, 100 GBIF datasets had passed through the Pensoft audit process and in many cases had been substantially improved. In this blog post I give an overview of the data errors found in the GBIF datasets.

In this overview I've used data problem categories that overlap to some extent, and although I avoided double-counting an error, the numbers should not be regarded as exact sample statistics. The numbers are also minima, as I haven't counted very minor errors that occurred only once or twice in a dataset.

Please also note that the 100 datasets audited were *original* datasets, not the datasets after processing by GBIF. These original datasets were downloaded as Darwin Core "source archives" from IPTs or from the GBIF website. There is, of course, some overlap between the data quality checks carried out independently by Pensoft and by GBIF. From the data publisher's point of view, an important difference is that GBIF does not require publishers to fix the problems it identifies.

The cost of the data auditing service provided by Pensoft is included in the article processing charges for the data paper. Pensoft also offers data auditing and cleaning as a stand-alone service ([Mesibov 2020] [3]). For information about the tools Pensoft uses to audit GBIF and other data, see [*A Data Cleaner's Cookbook*] [4] and its companion blog [*BASHing data*] [5].

## Structural problems

Three datasets contained records with **field shifts**, where blocks of data items were shifted "right" or "left" into fields in which they did not belong.

Four datasets had incompletely joined "event.txt" and "occurrence.txt" files. In other words, there were *eventID* entries in "occurrence.txt" that were not found in "event.txt". In a database, this would be called a **referential integrity failure**. Another dataset contained an "extendedmeasurementorfact.txt" table with no joining field (foreign key) at all.

## File content problems

Five occurrence-record datasets contained records that were exact **duplicates** apart from the unique code in *occurrenceID*. Some partial-duplicate records were also queried during audits and removed by authors after inspection.

Two occurrence-record datasets contained **non-unique identifiers**, i.e. duplicate codes in *occurrenceID*.

Sixty-two of the 100 datasets lacked one or more of 17 fields where the data context required that field to be present. These **missing fields** included *eventDate* (seven datasets) and *occurrenceID* (one dataset), which GBIF [requires] [6] in occurrence record datasets. The most frequently omitted field was *coordinateUncertaintyInMeters* (21 datasets).

## Between-field content problems

In 23 of the datasets, there were **valid entries in the wrong field**, with 25 fields affected. A simple example would be the entry "Holotype of Aus bus Smith, 1900" in the *type* field rather than in *typeStatus*.

Nearly half the datasets (49) contained **disagreements between fields within a single record**. Most of the disagreements were in taxon name fields, for example the *genus* or *specificEpithet* entries disagreeing with the entry in *scientificName*. Other common disagreements were between *eventDate* and *year*, *month* or *day*. This category of problems also includes minimum depth or elevation greater than maximum depth or elevation, and date anomalies such as *dateIdentified* before *eventDate*.

Spatial data formed a sub-category of between-field content problems. In many datasets, it was apparent that the data compilers did not clearly understand the relationship between coordinate values, coordinate uncertainty and coordinate precision.

## Within-field content problems

This category holds most of the problems identified in Pensoft audits.

Seventy-one of the 100 datasets had **invalid or incorrect entries** in one or more of 57 fields. The nature of the error varied with the field. In *scientificName*, for example, invalid entries ranged from "Sponge massive violet" to "Hesperiidae sp.", while 33°58'18.69" was a data error in *decimalLongitude*.

Forty-six datasets had **missing-but-expected entries** in one or more of 41 fields (**completeness failure**, in data science). Often the fields had valid entries in some records, but blanks in others.

A common problem was **pseudo-duplication**, where the same entry appeared in a field in more than one format (**consistency failure**). Pseudo-duplications were found in 33 fields across 36 datasets.

Two datasets had an **incrementing fill-down error**, in which an entry such as "WGS84" in *geodeticDatum* appeared as "WGS84", "WGS85", "WGS86", "WGS87" etc.

## Data item formatting problems

Sixty-five datasets had **non-recommended, incorrect or inconsistent formats** for data items in one or more of 31 fields. The *eventDate* field, for example, should contain dates in YYYY-MM-DD format, but data publishers used a wide range of other formats including Microsoft's serial date numbers, and sometimes mixed the date formats within a single dataset.

Thirty datasets had **NITS** as the sole entry in one or more of 29 fields. NITS (**N**othing **I**nteresting **T**o **S**ay) are entries like "-", "??", "nodata" and "N/A" that fill in for missing data and can be replaced by blanks.

Three datasets had **truncated data items** in four fields. These typically arise when data items exceed a character-count limit in a database field.

Six datasets had **unnecessary quoting** around data items in nine fields. Excess quoting often appears in a dataset built from a CSV file in which quotes are used to surround data items containing commas or spaces.

## Character problems in data items

Thirty-four datasets had **encoding conversion failures** in a range of fields, but most often in *scientificName*. Characters not in the basic ASCII set of letters and punctuation had been replaced by "?", &#xFFFD;, invisible [control characters] [7] or [mojibake] [8] (e.g. "Lefèbvre" became "LefÃ¨bvre") by the time the data had been UTF-8 encoded for GBIF use. Another two datasets had [combining characters] [9] in place of characters with diacritical marks.

A common character issue was the replacement of plain spaces with **no-break spaces** (NBSPs). An NBSP tells a Web browser or word-processing program that the words on either side of the NBSP are not to be spread across lines, and must remain together on the same line. NBSPs are invisible and a human reader cannot see the difference between *Aus bus* with a plain space and *Aus bus* with an NBSP. A text-parsing program will not only see the difference but will treat the two names as different. NBSPs were found in 17 fields across 39 datasets and were particularly abundant in *scientificName*.

## What to do about dataset quality?

GBIF datasets account for about 40% of the audits for Pensoft data papers since 2017. The non-GBIF datasets had the same range of problems as the GBIF ones, but between-field and within-field problems were much less common in datasets which did *not* use the Darwin Core framework.

It would be great if GBIF dataset compilers

+ knew the [basics of Darwin Core] [10]
+ knew [GBIF's data quality requirements and recommendations] [6]
+ knew the [basics of georeferencing] [11]
+ knew the [basics of character encoding] [12]
+ knew about spreadsheet hazards (if using spreadsheets to compile data) such as field shifts, fill-down errors, empty end-of-record fields and unwanted re-formatting of dates and numbers
+ knew how to efficiently check for between-field and within-field data problems

The Pensoft experience, however, has been that many compilers do not have the necessary knowledge or skills to produce tidy, complete, consistent and Darwin Core-compliant datasets. For those Pensoft authors, a post-compilation data-checking service has been both welcome and worthwhile.


[1]:https://doi.org/10.1186/1471-2105-12-S15-S2
[2]:https://doi.org/10.3897/biss.3.35019
[3]:https://blog.pensoft.net/2020/10/21/data-checking-for-biodiversity-collections-and-other-biodiversity-data-compilers-from-pensoft/
[4]:https://www.datafix.com.au/cookbook/
[5]:https://www.datafix.com.au/BASHing/
[6]:https://www.gbif.org/data-quality-requirements-occurrences
[7]:https://en.wikipedia.org/wiki/Control_character
[8]:https://en.wikipedia.org/wiki/Mojibake
[9]:https://en.wikipedia.org/wiki/Combining_character
[10]:https://dwc.tdwg.org/terms/
[11]:https://docs.gbif.org/georeferencing-best-practices/1.0/en/
[12]:https://kunststube.net/encoding/


