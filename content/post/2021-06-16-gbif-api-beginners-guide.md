---
title: GBIF API beginners guide
author: John Waller
date: '2021-09-07'
slug: gbif-api-beginners-guide
categories:
  - GBIF
tags:
  - API
lastmod: '2021-06-16T15:04:13+02:00'
draft: no
keywords: []
description: ''
authors: ''
comment: no
toc: ''
autoCollapseToc: no
postMetaInFooter: no
hiddenFromHomePage: no
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

This a **GBIF API** beginners guide. 

The [GBIF API technical documentation](https://www.gbif.org/developer/summary) might be a bit confusing if you have never used an API before. The goal of this guide is to introduce the GBIF API to a semi-technical user who may have never used an API before.  

The purpose of the GBIF API is to give users access to GBIF databases in a **safe way**. The GBIF API also allows [GBIF.org](https://www.gbif.org/) and [rgbif](https://github.com/ropensci/rgbif) to function. 

<!--more-->

You do not need any special software or skills to access the GBIF API. You simply have to visit a URL.

<center><big><a href="https://api.gbif.org/v1/species/match?name=Passer%20domesticus">https://api.gbif.org/v1/species/match?name=Passer%20domesticus</a></big></center>

You should see a **wall of text** known as [JSON](https://www.w3schools.com/js/js_json_intro.asp). 

```JSON
{
  "usageKey": 5231190,
  "scientificName": "Passer domesticus (Linnaeus, 1758)",
  "canonicalName": "Passer domesticus",
  "rank": "SPECIES",
  "status": "ACCEPTED",
  "confidence": 98,
  "matchType": "EXACT",
  "kingdom": "Animalia",
  "phylum": "Chordata",
  "order": "Passeriformes",
  "family": "Passeridae",
  "genus": "Passer",
  "species": "Passer domesticus",
  "kingdomKey": 1,
  "phylumKey": 44,
  "classKey": 212,
  "orderKey": 729,
  "familyKey": 5264,
  "genusKey": 2492321,
  "speciesKey": 5231190,
  "synonym": false,
  "class": "Aves"
}
```

<small>You can use a [chrome extension](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) to make the JSON look more readable. </small>

This link just performed a **match query** of all species records with "Passer domesticus" (House sparrow) in the GBIF backbone. This is useful for getting GBIF taxonkeys ("usageKey" here). See [this post](https://discourse.gbif.org/t/understanding-gbif-taxonomic-keys-usagekey-taxonkey-specieskey/3045) explaining the difference between different GBIF taxonomic keys.     

JSON can become very complex and nested. You might be thinking that a **table would be more convenient**. I will show you some strategies for converting to a table later on. Highly nested data is a **common headache of working with JSON**.  

## API Examples

<style>
.w3-card{
box-shadow:0 1px 2px 0 rgba(0,0,0,0.16),0 2px 5px 0 rgba(0,0,0,0.12)
}
.w3-panel{
padding:0.01em 10px;
margin-top:2px;
margin-bottom:2px
}
</style>

The **basic pattern** of an API call: <p>
https:// <span style="color:#73B273"><b>base url</b></span> / <span style="color:#CD915E"><b>api</b></span> / <span style="color:#3498DB"><b>function</b></span> ? <span style="color:#231F20"><b>parameter</b></span> = <span style="color:#d62d20"><b>query</b></span>
</p>


<ul>
  <li><span style="color:#73B273"><b>base url</b></span> : this will always be <b>https://api.gbif.org/v1/</b></li>
    <li><span style="color:#CD915E"><b>api</b></span> : this is the GBIF API group/namespace you want to query. GBIF has a few of these: species, download, occurrence, dataset ...</li>
  <li><span style="color:#3498DB"><b>function</b></span> : the functionality you want to use.</li>
  <li><span style="color:#231F20"><b>parameter</b></span> : the parameters for your API call. A <b>?</b> is sometimes used.  </li>
  <li><span style="color:#d62d20"><b>query</b></span> : the query you fill in. Sometimes will be free text and sometimes will be a predefined argument.  </li>
</ul>  

The [GBIF species API](https://www.gbif.org/developer/species) is one of the more useful APIs to know. Often the best way to learn how an API works is by looking at examples. The [name matcher](https://www.gbif.org/tools/species-lookup) is one of the more useful GBIF API endpoints. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>match</b></span>?<span style="color:#231F20"><b>name</b></span>=<span style="color:#d62d20"><b>Passer domesticus</b></span><br>
<small><a href="https://api.gbif.org/v1/species/match?name=Passer domesticus">https://api.gbif.org/v1/species/match?name=Passer domesticus</a></small> <br> The most interesting part of the JSON result is the "usageKey": 5231190, which is the GBIF taxonkey for house sparrows. A taxonkey is a GBIF internal id for a species or taxnomic group. These are useful in other parts of the API.<br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>match</b></span>?<span style="color:#231F20"><b>name</b></span>=<span style="color:#d62d20"><b>Tracheophyta</b></span><br>
<small><a href="https://api.gbif.org/v1/species/match?name=Tracheophyta">https://api.gbif.org/v1/species/match?name=Tracheophyta</a></small> <br>"usageKey": 7707728 is the GBIF taxonkey for vascular plants.<br>
</p></div>

`?` is the so-called **query parameter** ([more info](https://stackoverflow.com/questions/4024271/rest-api-best-practices-where-to-put-parameters)). It is usually hard to predict when you will need `?` before seeing an example. You might have noticed that your web browser filled in `%20` for the **spaces** in the last examples (you usually don't need to add this yourself). 

You can also get **lists of species** from higher groups using the species API. 


<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>search</b></span>?<span style="color:#231F20"><b>rank</b></span>=<span style="color:#d62d20"><b>SPECIES</b></span>&<span style="color:#231F20"><b>highertaxon_key</b></span>=<span style="color:#d62d20"><b>7707728</b></span><br>
<small><a href="https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=7707728">https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=7707728
</a></small><br>Will give back a list of vascular plant species information. This will just be the first 20 records, so it is not very useful.<br>
</p></div>

You can make use of the **limit** parameter to get back more results, **but only up to 1000 at a time**. This is because these results are usually held in a memory cache on GBIF servers. This design decision helps GBIF.org run more quickly. These [20 results](https://www.gbif.org/occurrence/search?taxon_key=7707728) should load quickly. And these [100 results](https://www.gbif.org/occurrence/search?taxon_key=7707728&limit=100) should load more slowly.

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>search</b></span><span style="color:#231F20">?<b>rank</b></span>=<span style="color:#d62d20"><b>SPECIES</b></span>&<span style="color:#231F20"><b>highertaxon_key</b></span>=<span style="color:#d62d20"><b>212</b></span>&</span><span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>100</b></span><br>
<small><a href="https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=100">https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=100</a></small> <br> Will give back 100 bird species records.<br>
</p></div>

You can also use the <span style="color:#231F20"><b>offset</b></span> parameter to skip through the first page of results to get to the next 1000 records. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>search</b></span><span style="color:#231F20">?<b>rank</b></span>=<span style="color:#d62d20"><b>SPECIES</b></span>&<span style="color:#231F20"><b>highertaxon_key</b></span>=<span style="color:#d62d20"><b>212</b></span>&</span><span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>1000</b></span>&<span style="color:#231F20"><b>offset</b>=</span><span style="color:#d62d20"><b>1000</b></span><br>
<small><a href="https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=1000&offset=1000">https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=1000&offset=1000</a></small> 
<br>Get the next 1000 records of birds. Offset will move the window of results up by 1000. You can <b>page through</b> API results if you want more records, but you can only go 100,000 records deep.<br>
</p></div>

Notice the use of **&** to combine parameters. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#3498DB"><b>search</b></span><span style="color:#231F20">?<b>rank</b></span>=<span style="color:#d62d20"><b>SPECIES</b></span>&<span style="color:#231F20"><b>highertaxon_key</b></span>=<span style="color:#d62d20"><b>212</b></span>&</span><span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>1000</b></span>&<span style="color:#231F20"><b>offset</b>=</span><span style="color:#d62d20"><b>2000</b></span><br>
<small><a href="https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=1000&offset=2000">https://api.gbif.org/v1/species/search?rank=SPECIES&highertaxon_key=212&limit=1000&offset=2000</a></small> 
<br>Page through 1000 more records.<br>
</p></div>

There are also other useful functions of the species API. Here are few examples of how to get synonyms and common names. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#d62d20"><b>212</b></span>/<span style="color:#231F20"><b>synonyms</b></span><br>
<small><a href="https://api.gbif.org/v1/species/212/synonyms">https://api.gbif.org/v1/species/212/synonyms</a></small> <br>All taxonomic synonyms of birds (there are none).<br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#d62d20"><b>7707728</b></span>/<span style="color:#231F20"><b>synonyms</b></span><br>
<small><a href="https://api.gbif.org/v1/species/7707728/synonyms">https://api.gbif.org/v1/species/7707728/synonyms</a></small> <br>All taxonomic synonyms of Tracheophyta.<br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#d62d20"><b>7707728</b></span>/<span style="color:#231F20"><b>vernacularNames</b></span><br>
<small><a href="https://api.gbif.org/v1/species/7707728/vernacularNames">https://api.gbif.org/v1/species/7707728/vernacularNames</a></small> <br>Gives you any common names for Tracheophyta, commonly known as "vascular plants".<br>
</p></div>

Hopefully with this introduction you will be able to figure out how the other GBIF API(s) work. Check out these other examples. 

* https://api.gbif.org/v1/occurrence/1258202889
  - information about this occurrence id
* https://api.gbif.org/v1/occurrence/1258202889/verbatim
  - information about this occurrence with no GBIF interpretation
* https://api.gbif.org/v1/dataset/4fa7b334-ce0d-4e88-aaae-2e0c138d049e
  - information about this dataset
* https://api.gbif.org/v1/dataset/8409e32e-f762-11e1-a439-00145eb45e9a/machineTag
  - all the machine tags (special/tags labels) for a certain dataset

## Using Curl

So far all of the examples I have been showing you are just links accessed in the browser. Usually the main reason you would want to use an API is because you want **software**, to interact with GBIF somehow. 

Along with your web browser you can also use a command line program called [curl](https://curl.se/download.html). We can visit this page using curl https://api.gbif.org/v1/species/7707728/vernacularNames. If you have curl [installed](https://stackoverflow.com/questions/9507353/how-do-i-install-and-use-curl-on-windows), you should be able to paste this into your any command line shell. 

```shell
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET https://api.gbif.org/v1/species/7707728/vernacularNames
```

This should print the same result you saw in your web browser. This by itself is not useful. This would need to be back of something larger, to be useful. For example, getting all the vernacular names of humming birds would be an [interesting project](https://gist.github.com/jhnwllr/0c10feb4be1c64cfec81f92a1047fddd). 

Most scripting languages will have a library for doing **http requests**. I often use use the GBIF API from **within R** with the packages `httr` or `jsonlite`. 

```r
httr::GET("https://api.gbif.org/v1/species/match?name=Trochilidae") %>%
httr::content()

jsonlite::fromJSON("https://api.gbif.org/v1/species/match?name=Trochilidae")
```

### Working with JSON results in R

JSON usually isn't a useful format for most end users, **who would usually rather have a table**. Turning JSON into a table can usually be done with a little bit of effort depending on the degree of JSON nesting. 

Below gets all of the vernacular names of Tracheophyta back as a data.frame in R. 

```r
library(jsonlite)

res = fromJSON("https://api.gbif.org/v1/species/7707728/vernacularNames",flatten = TRUE)

res$results # get a table of results
```

Unfortunately, **turning JSON into a table is not always this easy**. I have found [purrr](https://purrr.tidyverse.org/) to be helpful in working with highly nested JSON.

### Downloads

So far we have only covered **getting** data from the GBIF api. With the query (`?`) parameter we have sent back a few words or numbers, but often we need to send something more significant back to the GBIF servers, **like an entire file**. 

The [downloads API](https://www.gbif.org/developer/occurrence#predicates) is the primary place where sending a file to GBIF is useful.  

This file is a request to GBIF to download all of the [occurrences from Chile](https://www.gbif.org/occurrence/search?country=CL). Put this in a file called **query.json**.

```json
{
"creator": "fakeuser12",
"notification_address": [
"youremail1984@gbif.org"
],
"sendNotification": true,
"format": "SIMPLE_CSV",
"predicate": {
"type": "and",
"predicates": [
{
"type": "in",
"key": "COUNTRY",
"values": ["CL"]
}
]}}

```

**userName:PASSWORD** should be replaced with your **GBIF username and password** (for example: fakeuser12:12dfakePassword#osj323). Run this on the command line in the directory where you have your query.json file.

```shell
curl --include --user userName:PASSWORD --header "Content-Type: application/json" --data @query.json https://api.gbif.org/v1/occurrence/download/request
```

You could do the same thing also in R. Again here `user` and `pwd` are the your GBIF username and password. 

```r
library(httr)

POST(url = http://api.gbif.org/v1/occurrence/download/request, 
config = authenticate(user, pwd), 
add_headers("Content-Type: application/json"),
body = upload_file("query.json"), # path to your local file
encode = 'json') %>% 
content(as = "text")
```

And again you can always turn to `rgbif` for doing something like this quite compactly. See [here](https://discourse.gbif.org/t/setting-up-your-rgbif-environment-with-username-and-password/3017) for a nice way to save your GBIF credentials.

```r
# Here user,pwd, and email are your GBIF credentials.
library(rgbif)

occ_download(
pred("country", "CL"),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)

```

You might be thinking that this query is [easier to do in the web portal](https://www.gbif.org/occurrence/search?country=CL). This is true. The motivation to do it this way would be if you need to do it repeatedly. Also certain features (such as [not queries](https://www.gbif.org/developer/occurrence#download)) are only available via the API. 

### Further reading 

* [GBIF API Documentation](https://www.gbif.org/developer/summary)
* [(Almost) everything you want to know about the GBIF Species API]( https://data-blog.gbif.org/post/gbif-species-api/)

