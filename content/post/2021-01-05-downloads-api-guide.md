---
title: Downloads API Guide
author: John Waller
date: '2021-01-05'
slug: downloads-api-guide
categories:
  - GBIF
tags:
  - downloads
lastmod: '2021-01-05T13:07:01+01:00'
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

Most users will want to make downloads using the [search interface](https://www.gbif.org/occurrence/search?occurrence_status=present&q=). 

For more complex downloads, it is helpful to use the [GBIF downloads API](https://www.gbif.org/developer/occurrence#download).

One good reason for using the downloads API would be to download a [long species list](https://data-blog.gbif.org/post/downloading-long-species-lists-on-gbif/). 

With this guide, I will be showing you how to take advantage of the **downloads API**, with some features that are not currently avaiable in the search interface. 

<!--more-->

This guide will use **rgbif** (`occ_download`) and **curl** for the examples. 

Here is a **simple example** using curl: 

Create a file named **query.json** and save it somewhere on your computer. This example downloads all the the records with basis of record equal to "PRESERVED_SPECIMEN" and in the countries Kuwait, Iraq, and Iran ("KW","IQ","IR"). You will need to change the **userName** to your GBIF username. And you will need to change the **userEmail@example.org** to your email. 

```json
{
  "creator": "userName",
  "notificationAddresses": [
    "userEmail@example.org"
  ],
  "sendNotification": true,
  "format": "SIMPLE_CSV",
  "predicate": {
    "type": "and",
    "predicates": [
      {
        "type": "equals",
        "key": "BASIS_OF_RECORD",
        "value": "PRESERVED_SPECIMEN"
      },
      {
        "type": "in",
        "key": "COUNTRY",
        "values": [
          "KW",
          "IQ",
          "IR"
        ]
      }
    ]
  }
}

```

Next we will use [curl](https://curl.se/download.html) to submit the request. You will need to change **userName:PASSWORD** to your **GBIF username** and **GBIF password**. Run this in the same folder where you saved **query.json**.

```shell 
curl --include --user userName:PASSWORD --header "Content-Type: application/json" --data @query.json https://api.gbif.org/v1/occurrence/download/request
```

This simple example could easily be done using the [search interface](https://www.gbif.org/occurrence/search?basis_of_record=PRESERVED_SPECIMEN&country=KW&country=IR&country=IQ). 

It could also be run using **rgbif**. 

```R 
library(rgbif)

# fill in your GBIF user info
user=""
pwd=""
email=""

occ_download(
pred("basisOfRecord", "PRESERVED_SPECIMEN"),
pred_in("country", c("KW","IQ","IR"),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)
```

## Exclusion queries

For a long time it was impossible to do "not" queries on GBIF. Recent changes, however, have made it possible to use exclusion filters in downloads. 

For example, if you wanted all of the GBIF bird records **except for** eBird (now with >500M records). 

In **curl**:

```json
{
  "creator":"userName",
  "notificationAddresses": ["userName@example.org"],
  "predicate":
  {
    "type":"not",
    "predicate":
    {
      "type":"equals",
      "key":"DATASET_KEY",
      "value":"4fa7b334-ce0d-4e88-aaae-2e0c138d049e"
    }
  }
}
```

In **rgbif**:

```R 
library(rgbif)

# fill in your GBIF user info
user=""
pwd=""
email=""

occ_download(
pred("basisOfRecord", "PRESERVED_SPECIMEN"),
pred_not("datasetKey","4fa7b334-ce0d-4e88-aaae-2e0c138d049e"),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)
```


non dwc terms in downloads



