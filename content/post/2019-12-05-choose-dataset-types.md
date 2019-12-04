---
title: How to choose a dataset class on GBIF?
author: Marie Grosjean
date: '1990-12-05'
slug: choose-dataset-type
categories:
  - GBIF
  - Publishing
tags:
  - publish
  - GBIF
  - checklist
  - occurrence
  - sampling-event
  - dataset
  - dataset type
  - dataset classes
  - metadata
lastmod: '2019-12-05'
draft: no
keywords: ['dataset', 'metadata', 'checklist', 'occurrence', 'sampling-event']
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

If you are a (first time) publisher on GBIF and you are trying to decide which type of dataset would best fit your data, this blogpost is for you.

All the records shared on GBIF are organized into datasets. Each dataset is associated with some metadata describing its content (the classic "what, where, when, how"). The dataset's content depends strongly on the dataset's class. GBIF currently support four types of dataset:

* Resource datasets (e.g. metadata-only datasets)
* Checklists
* Occurrence datasets
* Sampling-event datasets

These classes are described in detail on our [Dataset classes page](https://www.gbif.org/dataset-classes) along with links to examples and documentation. As mentioned on this page:

> The four classes of datasets supported by GBIF start simply and become progressively richer, more structured and more complex.

In other words, the distinction between the different classes is not always very obvious. This is why I tried to put together a quick guide to help you choose the class that fits best your data.

# Five questions to help you decide

The first step to help you choose a dataset type is to ask yourself the right questions. The five questions below might not cover every single case but I hope it can help most of you:

![5 questions to choos a dataset type](/post/2019-12-05-choose-dataset-types/which_dataset_type_to_choose_questions.jpg)


# Data quality requirements associated with each class

The data quality requirements describe what you should provide for each dataset class. It doesn't mean that the data won't be indexed if some values are missing, but these requirements summerize what can be considered meaningful information for each class.

These requirements are detailed on our [Data quality requirements](https://www.gbif.org/data-quality-requirements) pages. I strongly suggest that you consult these pages before sharing data on GBIF.

That being said, the schema below summarizes the information required for each classes:

![Dataset classes and requirements](/post/2019-12-05-choose-dataset-types/dataset_classes_and_requirements.jpg)


# Sampling-event datasets

This type of datasets contain the most information and is therefore the most valuable for the community. We have an [introduction page](https://www.gbif.org/sampling-event-data) on the topic. If you are considering publishing sampling-events, this is a good place to start but I will make an even smaller shorter introduction here.

Sampling-event datasets can seem a bit more complicated that other datasets with because they need two files:

* One discribing events
* One describing the occurrences associated with each event

Each occurrence is link to its event thanks to the [eventID](https://dwc.tdwg.org/terms/#dwc:eventID) columns.
The example below illustrates how sampling-event datasets are structured.

![Sampling-event dataset example](/post/2019-12-05-choose-dataset-types/schema_sampling_event.jpg)

# Conclusion

I hope this short guide is useful. Don't hesitate to [browse the datasets available on GBIF](https://www.gbif.org/dataset/search?q=) and check what other data providers chose.

If your data cannot fit into any of the classes supported by GBIF, please leave a comment or send an email to helpdesk@gbif.org.
