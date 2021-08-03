---
title: GBIF API beginners guide
author: John Waller
date: '2021-06-16'
slug: gbif-api-beginners-guide
categories:
  - GBIF
tags:
  - API
lastmod: '2021-06-16T15:04:13+02:00'
draft: yes
keywords: []
description: ''
authors: ''
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

This a **GBIF API** beginners guide. 

The GBIF API technical documentation can be found [here](https://www.gbif.org/developer/summary). This documentation is nice, but you might be a bit confusing if you have never used an API before. This guide is will walk you through how to begin using the GBIF API from a non-technical perspective.  

**The goal of the GBIF API is to give access to GBIF databases is a safe way**, so that a user does not accidentally delete something or run a request that will crash GBIF servers or add some data they were not supposed to. The GBIF API also allows the web interface [GBIF.org](https://www.gbif.org/) to function. Almost all GBIF.org web pages are created using the API. 

<!--more-->

You do not need any special software or skills to access the GBIF API. **You simply have to visit a URL with your web browser**. Depending on what web browser you are using, you should see a **wall of text**. 

[https://api.gbif.org/v1/species/5231190/name](https://api.gbif.org/v1/species/5231190)

```JSON
// 20210617092223
// https://api.gbif.org/v1/species/5231190/name

{
  "key": 8290258,
  "scientificName": "Passer domesticus (Linnaeus, 1758)",
  "type": "SCIENTIFIC",
  "genusOrAbove": "Passer",
  "specificEpithet": "domesticus",
  "bracketAuthorship": "Linnaeus",
  "bracketYear": "1758",
  "parsed": true,
  "parsedPartially": false,
  "canonicalName": "Passer domesticus",
  "canonicalNameWithMarker": "Passer domesticus",
  "canonicalNameComplete": "Passer domesticus (Linnaeus, 1758)",
  "rankMarker": "sp."
}

```

<small>You can use a [chrome extension](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) to make the JSON look more readable. </small>

It might not be immediately clear what the parameters in the response mean, but usually with a bit of puzzling, you can figure it out. Almost all GBIF API(s) will give you back **JSON** (seen above), which is just structured human readable text. JSON can become very complex and nested depending on the underlying data. You might be thinking that a **table would be more convenient than nested structured text**. I will show you some strategies for converting JSON later on, but this is a common headache of working with JSON or any nested data type.  

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
https:// <span style="color:#73B273"><b>base url</b></span> / <span style="color:#CD915E"><b>api</b></span> / <span style="color:#231F20"><b>parameter</b></span> ? <span style="color:#231F20"><b>parameter</b></span> = <span style="color:#d62d20"><b>query</b></span>
</p>


<ul>
  <li><span style="color:#73B273"><b>base url</b></span> : this will always be <b>https://api.gbif.org/v1/</b></li>
    <li><span style="color:#CD915E"><b>api</b></span> : this is the GBIF API group/namespace you want to query. GBIF has a few of these: species, download, occurrence, dataset ...</li>
  <li><span style="color:#231F20"><b>parameter</b></span> : the parameters for your API call. A <b>?</b> is sometimes used.  </li>
  <li><span style="color:#d62d20"><b>query</b></span> : the query you fill in. Sometimes will be free text and sometimes will be a predefined argument.  </li>
</ul>  

The [GBIF species API](https://www.gbif.org/developer/species) is one of the more useful APIs to know. Often the best way to learn how an API works is by looking at examples. The [name matcher](https://www.gbif.org/tools/species-lookup) (seen below) is one of the more useful GBIF API endpoints. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>match</b></span>?<span style="color:#231F20"><b>name</b></span>=<span style="color:#d62d20"><b>Passer domesticus</b></span><br>
<small><a href="https://api.gbif.org/v1/species/match?name=Passer domesticus">https://api.gbif.org/v1/species/match?name=Passer domesticus</a></small> <br><small> If you look at the JSON result, the part that is most interesting is <b>"usageKey": 5231190</b>, which is the GBIF taxonkey for house sparrows. You can visit <a href=https://www.gbif.org/occurrence/search?taxon_key=5231190>https://www.gbif.org/occurrence/search?taxon_key=5231190</a> to see this usage key / taxonkey in action.</small><br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>match</b></span>?<span style="color:#231F20"><b>name</b></span>=<span style="color:#d62d20"><b>Tracheophyta</b></span><br>
<small><a href="https://api.gbif.org/v1/species/match?name=Tracheophyta">https://api.gbif.org/v1/species/match?name=Tracheophyta</a></small> <br><small> <b>"usageKey": 7707728</b> is the GBIF taxonkey for vascular plants.</small><br>
</p></div>

`?` is the so-called **query parameter** ([more info](https://stackoverflow.com/questions/4024271/rest-api-best-practices-where-to-put-parameters)). It is usually hard to predict when you will need `?` before seeing an example. You might have noticed that your web browser filled in `%20` for the **spaces** in the last examples (you usually don't need to add this yourself). 

You can also get **lists of species** from higher groups using the species API. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<br>
<small><a href="https://api.gbif.org/v1/species/">https://api.gbif.org/v1/species/</a></small> <br><small> Will give back a long list of species information. This is just 20 random records so it is not very useful.</small><br>
</p></div>

You can make use of the **limit** parameter to get back more results, **but only up to 1000 at a time**. This is because these results are usually held in some in memory cache on GBIF servers, and GBIF cannot hold an too many of them at time. This design decision helps the GBIF web portal run more quickly.  

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>212</b></span>?<span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>100</b></span><br>
<small><a href="https://api.gbif.org/v1/species/212/?limit=100">https://api.gbif.org/v1/species/212/?limit=100</a></small> <br><small> Will give back 100 bird records.</small><br>
</p></div>

You can also use the <span style="color:#231F20"><b>offset</b></span> parameter to skip through the first page of results to get to the next 1000 records. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>212</b></span>?<span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>1000</b></span>&<span style="color:#231F20"><b>offset</b>=</span><span style="color:#d62d20"><b>1000</b></span><br>
<small><a href="https://api.gbif.org/v1/species/212/?limit=1000&offset=1000">https://api.gbif.org/v1/species/212/?limit=1000&offset=1000</a></small> <br><small>Get the next 1000 records of birds. Offset will move the window of results up by 1000. You can <b>page through</b> API results if you want more records, but you can only go <b>100,000 records deep</b>.</small><br>
</p></div>

Notice the use of **&** to combine parameters. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>212</b></span>?<span style="color:#231F20"><b>limit</b></span>=<span style="color:#d62d20"><b>1000</b></span>&<span style="color:#231F20"><b>offset</b>=</span><span style="color:#d62d20"><b>2000</b></span><br>
<small><a href="https://api.gbif.org/v1/species/212/?limit=1000&offset=2000">https://api.gbif.org/v1/species/212/?limit=1000&offset=2000</a></small> <br><small>Page through 1000 more records.</small><br>
</p></div>


There are also other useful functions of the species API. Here are few additional examples. 

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>212</b></span>/<span style="color:#231F20"><b>synonyms</b></span><br>
<small><a href="https://api.gbif.org/v1/species/212/synonyms">https://api.gbif.org/v1/species/212/synonyms</a></small> <br><small>All taxonomic synonyms of birds (there are none).</small><br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>7707728</b></span>/<span style="color:#231F20"><b>synonyms</b></span><br>
<small><a href="https://api.gbif.org/v1/species/7707728/synonyms">https://api.gbif.org/v1/species/7707728/synonyms</a></small> <br><small>All taxonomic synonyms of Tracheophyta.</small><br>
</p></div>

<div class="w3-panel w3-card"><p>
https://<b><span style="color:#73B273">api.gbif.org/v1</span></b>/<span style="color:#CD915E"><b>species</b></span>/<span style="color:#231F20"><b>7707728</b></span>/<span style="color:#231F20"><b>vernacularNames</b></span><br>
<small><a href="https://api.gbif.org/v1/species/7707728/vernacularNames">https://api.gbif.org/v1/species/7707728/vernacularNames</a></small> <br><small>Gives you any common names for Tracheophyta, commonly known as "vascular plants". Remeber you could find out the taxonkey for Tracheophyta by using the name matcher from before <a href="https://api.gbif.org/v1/species/match?name=Tracheophyta">https://api.gbif.org/v1/species/match?name=Tracheophyta</a>. </small><br>
</p></div>

Hopefully with this introduction you will be able to figure out how the other GBIF API(s) work.  


## Using Curl

So far all of the examples I have been showing you are just links accessed in the browser. Usually the main reason you would want to use an API is **because you want software (rather than a human) to interact with gbif.org somehow**. 

Along with your web browser you can also use a command line program called [curl](https://curl.se/download.html). 

<small>visit this page using curl https://api.gbif.org/v1/species/7707728/vernacularNames </small>


```shell
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET https://api.gbif.org/v1/species/7707728/vernacularNames
```

This should print the same result you saw in your web browser. This by itself is not very useful and you usually have to make this part of a larger program for it to be interesting. For example, getting all the vernacular names of humming birds would be an [interesting project](https://gist.github.com/jhnwllr/0c10feb4be1c64cfec81f92a1047fddd). 

Most popular scripting languages will have a library for doing something similar from within the language of your choice. I often use use the GBIF API **from within R** with the packages `httr` or `jsonlite`. 

```R
library(httr)

GET("https://api.gbif.org/v1/species/match?name=Trochilidae") %>%
content()
```

```R
library(jsonlite)

fromJSON("https://api.gbif.org/v1/species/match?name=Trochilidae")
```



### Working with JSON results in R

JSON usually isn't a useful format for most end users, **who would usually rather have a table**. Turning JSON into a table can usually be done with a little bit of effort depending on the degree of JSON nesting. 

Although JSON is good for storing structured complex data, often **we just want some table of results**. 

Lets get all of the vernacular names of Tracheophyta back as a table. 

```r
library(jsonlite)

res = fromJSON("https://api.gbif.org/v1/species/7707728/vernacularNames",flatten = TRUE)

res$results # get a table of results
```

Unfortunately, **turning JSON into a table is not always this easy**. I have found [purrr]() to be helpful in working with highly nested JSON. You can see a complex example [here](https://gist.github.com/jhnwllr/0c10feb4be1c64cfec81f92a1047fddd). 

### Downloads

So far we have only covered **getting** data from the GBIF api. With the query (`?`) parameter we have sent back a few words, but often we need to send something more significant back to the GBIF servers, like an entire file. In API-speak this is called a **PUT** request. 

The [downloads API](https://www.gbif.org/developer/occurrence#predicates) is the primary place where sending (posting) a file to GBIF is useful.  

This file is a request to download all of the [occurrences form Chile] 

<small>Put this in a file called query.json **query.json**.</small>

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

**userName:PASSWORD** should be replaced with your **GBIF username and password** (for example: fakeuser12:12dfakePassword#osj323).

<small>Run this on the command line in the directory where you have your query.json file</small>

```shell
curl --include --user userName:PASSWORD --header "Content-Type: application/json" --data @query.json https://api.gbif.org/v1/occurrence/download/request
```

You could do the same thing also in R. Again here `user` and `pwd` are the your GBIF username and password. 

```R
library(httr)

library(httr)
POST(url = http://api.gbif.org/v1/occurrence/download/request, 
config = authenticate(user, pwd), 
add_headers("Content-Type: application/json"),
body = upload_file("query.json"), # path to your local file
encode = 'json') %>% 
content(as = "text")
```

And again you can always turn to `rgbif` for doing something like this quite compactly. 

<small>Here user,pwd, and email are your GBIF credentials</small>

```r
library(rgbif)

occ_download(
pred("country", "CL"),
format = "SIMPLE_CSV",
user=user,pwd=pwd,email=email
)

```

You might be thinking that this query is [easier to do in the web portal](https://www.gbif.org/occurrence/search?country=CL). This is true. The motivation to do it this way would be if you need to do it repeatedly. If each month you wanted all of the occurrences from Chile, the GBIF API would allow you to do this **consistently and automatically with software**. 

### Further reading 


* The species API blog post 
*
*
*



