---
title: Using shapefiles on GBIF data with R
authors: Jan Kristoffer Legind
date: '2018-11-22'
slug: your-post
categories:
  - GBIF
tags:
  - R
  - maps
  - arctic
  - shapefile
lastmod: '2018-11-21T15:24:11+02:00'
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

# Using shapefiles on GBIF data with R

It happens sometimes that users need GBIF data that fall within specific boundaries. The GBIF Portal provides a location filter where it is possible to draw a rectangle or a polygon on the map and get the occurrence records within this shape. However these tools have a limited precision and occasionally the job calls for more complex shapes than the GBIF Portal currently supports.

## The shapefile
In this case I want to use a **Circumpolar Arctic Map** which is already here providing us with some challenges.  
The map projection for this is 'laea' (*Lambert Azimuthal Equal Area* for those of you out in mercator land) and it is well suited to polar perspective.
So the task is to identify the Plant records that fall within this shape file of land areas in the Arctic region.


![arctic](/post/2018-11-22-R_shapefiles_GBIF/arcticPlot.png)

~~ARCTIC MAP PROJECTION~~

## The GBIF data
Since we are not going to work on GBIF data directly in the portal, we have to download a subset to work on in R. Now the more data we grab the bigger the risk that we will blow through memory. These are the steps taken to reduce the initial data download:

 1. As the target is the *arctic region*it would be reasonable to query for all plant records above 55 degrees latitude. This returns a file of about ~35 million records. This is too large.
 2. Remove records within Sweden, Finland, Denmark and Great Britain.
 3. Filter records out that have the geospatial issues flag and now we get down to ~6 million records.




