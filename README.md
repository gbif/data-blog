
![](https://raw.githubusercontent.com/jhnwllr/gbifAnalyticsBlog/master/static/GBIF-analytics-blog.png)

The GBIF analytic blog is a [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) blog generated using [hugo](https://gohugo.io/) and currently hosted using [Netlify](https://www.netlify.com/).  The blog uses a slightly modified theme called [even](https://github.com/olOwOlo/hugo-theme-even).  

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
And the rest of the fields can be left the same. I am not really sure if they are all needed, but just to not break the theme, I would **leave them in**. You can probably **ignore** the `lastmod field:`. 

You can download a simple `.md` blog post template `./content/post/2025-09-24-template.md` [link here](). 

### How hugo works

[Hugo](https://gohugo.io/) is a simple static site generator that will turn `.md` files into nicely styled html files. 

### Naming a post

Make sure to name your post something like `2018-08-14-finding-gridded-datasets.md` with `-` for spaces and the `YYYY-MM-DD` date at the front. The date at the front determines what order the post will appear on the blog. 

* 2018-05-23-I-traveled-back-in-time.md
* 2019-01-30-GBIF-Pipelines-How-To-Use.md
* 2030-02-12-How-To-Use-VR-Dataviewer-api.md

### Getting your post on the blog 

A post can be done by simply copying an `.md` into the gitHub `./content/post/` directory. 

```
.
├───content
    └───post <- put YYYY-MM-DD-yourPost.md here
```

I am still working out the best way to do this in practice. 

Some options: 

1. Send me the `YYYY-MM-DD-yourPost.md` file **jwaller@gbif.org**
2. Fork the repository and add the `YYYY-MM-DD-yourPost.md`, then do a pull request. 
3. If you have access to the GitHub, just update the repository yourself. 

After `YYYY-MM-DD-yourPost.md` is in the `./content/post/` directory, [Netlify](https://www.netlify.com/) will build the blog automatically, the theme will be applied, and **your post will be online**. 

### Insert an image or other static content 

You can always link images from some online hosting service, but it is probably better to associate images directly with a blog post. 

To host static content (images) associated with a particular blog post, you need to **create a folder** in the post directory. For the template post `2025-09-24-template.md`, I created a folder named `./content/post/2025-09-24-template_files/`. 

```
.
├───content
    ├───post
        ├───template
        └───2025-09-24-template_files <- put images here
            └───example.jpg
```

Then to link an [image in markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#images), use a path like this to the image. 

```
![example](/post/2025-09-24-template_files/example.jpg)
```
Note the lack of **/content/** in the path. This is because the image is being linked from the `./public/post/2025-09-24-template_files/`. The `./public/` folder becomes the `.` root directory when the site is built by hugo.  

### Previewing a post

You can preview the post using any markdown [online editor](https://stackedit.io/app#). 

If you want to see the post with the blog theme applied, **but don't want to post to appear yet on the home page**, you can set `hiddenFromHomePage` to **yes** in the header text. 

```
hiddenFromHomePage: yes
```
You will have to set this to **no** for the blog to appear on the home page. The default is **no**.

### Locally serving the site using hugo

You can download and serve the blog locally using [hugo server](https://gohugo.io/commands/hugo_server/). 

### Writing a post in Rmarkdown

It is also possible to write a post in [Rmarkdown](https://rmarkdown.rstudio.com/lesson-1.html), since this is a data blog some might want to contribute using **Rmarkdown**. 

Writing a post in `.Rmd` is more complicated than writing a vanilla `.md` post. 

In order to write a blog post with embedded R code (including [html widgets](https://www.htmlwidgets.org/)), you will need to set up an environment to work in (I assume you have **Rstudio** installed). 

1. Install [blogdown](https://bookdown.org/yihui/blogdown/installation.html) `install.packages("blogdown")`
2. Install hugo in R `blogdown::install_hugo()`
3. Download the blog repository from GitHub
4. Open the `gbifAnalyticsBlog.Rproj` file using Rstudio
5. Click the `Addins` menu (you might need to restart Rstudio for the menu to appear)
6. Click **New Post** and fill in the form
7. Serve site locally using **Serve Site** in `Addins` menu
7. Write post
8. Create a pull request to the main blog GitHub
9. Post should should rebuild automatically

Unless you edit a previously built `.Rmd`, you should not need to install any additional R packages. 

### Making changes to the blog appearance 

Unfortunately, hugo does not document changing a theme appearance very well. [This](https://bwaycer.github.io/hugo_tutorial.hugo/themes/customizing/) I have found for how to edit a theme. 

>The following are key concepts for Hugo site customization. Hugo permits you to supplement or override any theme template or static file, with files in your working directory. When you use a theme cloned from its git repository, you do not edit the theme’s files directly. 

If you want to change the blog appearance, you need to create folders and files in the **root directory** that match folders and files in the public directory. Hugo will copy and replace files in the root directory into the public directory. 

### css

Changes to theme css can be done using. 

```
.
├───static
    └───dist 
        └───even.min.css
```

**NOTE!** editing the `./public/dist/even.min.css` will have no effect on the blog appearance because it is replaced by `./static/dist/even.min.css` when hugo builds the blog. 

### flavicon and logo image

To replace the flavicon change the files in the root `./static/`. 

```
.
├───static
    ├───favicon-16x16.png
    ├───favicon-32x32.png
    └───GBIF-analytics-blog.png
```

### Other blog settings 

Other settings in the blog can be set in the `./config.toml`. 

### Deleting a post that has already been added to the blog 

Delete the `.md` file from from **2 locations**.

1. Delete the `.md` file from `./content/post/` .

```
.
├───content
    └───post <- delete yourPost.md and image folder (if you added one)
```

2. Delete the `.md` file from `./public/post/`

```
.
├───public
    └───post <- delete yourPost.md and image folder (if you added one)
```

### Netlify build settings 

The build settings of the blog on netlify. 

```
Repository: https://github.com/jhnwllr/gbifAnalyticsBlog
Build command: hugo
Publish directory: public
Production branch: master
Branch deploys: Deploy only the production branch and its deploy previews
Public deploy logs: Logs are public

Build environment variables
HUGO_VERSION: 0.42
```

