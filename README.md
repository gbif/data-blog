
![](https://raw.githubusercontent.com/jhnwllr/gbifAnalyticsBlog/master/static/GBIF-analytics-blog.png)

The GBIF analytics blog is a [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) blog generated using [hugo](https://gohugo.io/) and currently hosted using [Netlify](https://www.netlify.com/).  The blog uses a slightly modified theme called [even](https://github.com/olOwOlo/hugo-theme-even). We hope markdown will allow us to simply move posts to a new service or theme when we get tired of this one in a few years. 

### Writing a post in markdown

Posts should be written in markdown. If you are unfamiliar, you can use some of the links below. 

* [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [online editor](https://stackedit.io/app#)

Markdown files are just simple text files with an `.md` extension. 

You can download a simple `.md` blog post template [here](). 

### Post Template

Posts should have a specific header text. 

```
---
title: Test Post
author: john
date: '2018-09-21'
slug: test-post
categories:
  - GBIF
tags:
  - test tag
  - test
lastmod: '2018-09-21T15:24:11+02:00'
keywords: []
description: ''
comment: no
toc: no
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

```

Important fields to **fill in** are below: 

```
title: Test Post
author: john
date: '2018-09-21'
slug: test-post
categories:
  - GBIF
tags:
  - test tag
  - test
lastmod: '2018-09-21T15:24:11+02:00'

(The rest of the header text)

```
And the rest of the fields can be left the same. I am not really sure if they are all needed, but just to not break the theme, I would **leave them in**. 

You can download a simple `.md` blog post template [here](). 

### How hugo works

[Hugo](https://gohugo.io/) is a simple static site generator that will turn `.md` files into nicely styled html files. 

### Getting your .md onto the blog

A post can be done by simply copying an `.md` into the gitHub post directory. 

Make sure to name your post something like `2018-08-14-finding-gridded-datasets.md` with `-` for spaces and the `YYYY-MM-DD` date at the front. This determines what order the post will appear on the blog. 

#### Blog post names examples 

* 2018-05-23-I-travlled-back-in-time.md
* 2019-01-30-GBIF-Pipelines-How-To-Use.md
* 2030-02-12-How-To-Use-VR-Dataviewer-api.md


```
.
├───content
    └───post <- put .md here
```

I am still working out the best way to do this in practice. 

Some options: 

1. Send me the `.md` file **jwaller@gbif.org**
2. Fork the repository and add the `.md` file, then do a pull request. 
3. If you have access to the data products gitHub, just update the repository yourself. 

### After a post is in the post directory

**After the gitHub repository is updated**, [Netlify](https://www.netlify.com/) will build the blog automatically and the theme will be applied and the post will appear. 

### Previewing a post

You can preview the post using any markdown [online editor](https://stackedit.io/app#). 

If you want to see the post with the blog theme applied, **but don't want to post to appear yet on the home page**, you can set `hiddenFromHomePage` to **yes** in the header text. 

```
hiddenFromHomePage: yes
```
You will have to set this to **no** for the blog to appear on the home page. The default is **no**.

### Locally serving the site

You can download and serve the blog locally using [hugo server](https://gohugo.io/commands/hugo_server/). 

### Writing a post in Rmarkdown

It is also possible to write a post in [Rmarkdown](https://rmarkdown.rstudio.com/lesson-1.html), since this is a data/analytics blog many people who might want to contribute using **Rmarkdown**. 

Writing a post in Rmarkdown is more complicated than writing a vanilla `.md` post. 

With Rmarkdown files end with `.Rmd`. In order to write a blog post with embedded R code, you will need to set up an environment to work with 




