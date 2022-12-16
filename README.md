
![](https://raw.githubusercontent.com/gbif/data-blog/master/static/logo.png)

The GBIF [data blog](https://data-blog.gbif.org/) is a [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) blog generated using [hugo](https://gohugo.io/) and currently hosted using [Netlify](https://www.netlify.com/).  The blog uses a slightly modified theme called [even](https://github.com/olOwOlo/hugo-theme-even).  

How to write a blog post 
------
### TLDR recommended blog-writing workflow 

1. Write a post in markdown named `YYYY-MM-DD-your-post.md` 
2. Save plots or images into folder named `./content/post/YYYY-MM-DD-your-post_files`

```
# embed images with this template
![example](/post/YYYY-MM-DD-your-post_files/plot1.jpg)
```

3. Copy/paste [header text](https://github.com/gbif/data-blog/blob/master/templates/header.txt) into your `.md` file 
```
---
title: Your Title
author: Your Name
date: '2022-01-04'
slug: your-post
categories:
  - GBIF
tags: []
lastmod: '2022-01-04T10:19:14+01:00'
draft: yes
keywords: []
description: ''
authors: ''
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
4. Edit important fields in header text: 
 
 - `title: Your post` <- your title 
 - `authors: Your Name` <- your author name
 - `date: '2018-09-21'` <- the order the post will appear on blog. Does not necessarily have to be the same as the date in  `YYYY-MM-DD-your-post.md`. 
 - `slug: your-post` <- name that will appear in the url https://data-blog.gbif.org/post/your-post/ 
 - `tags:` <- category tags for post
 - `hiddenFromHomePage: yes` <- keeps people from seeing your post as you work on it
 - `draft: yes` <- keeps the post off of navigation as you work on it 
5. Copy `YYYY-MM-DD-your-post.md` and `YYYY-MM-DD-your-post_files` into `./content/post/`
```
.
├───content
    ├───post
        ├───YYYY-MM-DD-your-post.md
        └───YYYY-MM-DD-your-post_files <- put images here
            └───plot1.jpg
```
6. **Find mistakes** 
7. Fix mistakes and push changes to **gbif/data-blog** until satisfied. 
8. Switch to `hiddenFromHomePage: no` and `draft: no`
9. Check your post at https://data-blog.gbif.org/post/your-post/ (Netlify will build your post automatically)
10. Ask [someone](https://github.com/jhnwllr) with admin access to [https://discourse.gbif.org/](https://discourse.gbif.org/) to turn on comments for you.  

------
### Previewing a post locally 

It is possible to preview a post locally using `hugo`. https://gohugo.io/installation/

```

```

More details on how to write a post
------
### Writing a post in markdown

Posts should be written in markdown. If you are unfamiliar, you can use some of the links below. 

* [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [online editor](https://stackedit.io/app#)

Markdown files are just simple text files with an `.md` extension. 

### Getting your post on the blog 

A post can be done by simply copying an `.md` into the **gbif/data-blog** `./content/post/` directory. 

```
.
├───content
    └───post <- put YYYY-MM-DD-your-post.md here
```

### Insert an image or other static content 

You can always link images from some online hosting service, but it is probably better to associate images directly with a blog post. 

To host static content (images) associated with a particular blog post, you need to **create a folder** in the post directory. For `YYYY-MM-DD-your-post.md` you can create a folder named `./content/post/YYYY-MM-DD-your-post_files/`. 

```
.
├───content
    ├───post
        ├───YYYY-MM-DD-your-post.md
        └───YYYY-MM-DD-your-post_files <- put images here
            └───plot1.jpg
```

Then to link an [image in markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#images), use a path like this to the image. 

```
![example](/post/YYYY-MM-DD-your-post_files/plot1.jpg)
```
Note the lack of **/content/** in the path. This is because the image is being linked from the `./public/post/YYYY-MM-DD-your-post_files/`. The `./public/` folder becomes the `.` root directory when the site is built by hugo.  

### Deleting a post that has already been added to the blog 

Delete the `.md` file from from **2 locations**.

1. Delete the `.md` file from `./content/post/` .

```
.
├───content
    └───post <- delete YYYY-MM-DD-your-post.md and image folder (if you added one)
```

2. Delete the `.md` file from `./public/post/`

```
.
├───public
    └───post <- delete YYYY-MM-DD-your-post.md and image folder (if you added one)
```


### Netlify build settings 

The build settings of the blog on netlify. 

```
Repository: https://github.com/gbif/data-blog
Build command: hugo
Publish directory: public
Production branch: master
Branch deploys: Deploy only the production branch and its deploy previews
Public deploy logs: Logs are public

Build environment variables
HUGO_VERSION: 0.91.2
```

