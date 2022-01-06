
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
6. Check your post at https://data-blog.gbif.org/post/your-post/ (Netlify will build your post automatically)
7. **Find mistakes** 
8. Fix mistakes and push changes to **gbif/data-blog** until satisfied. 
9. Switch to `hiddenFromHomePage: no` and `draft: no`
10. Ask [someone](https://github.com/jhnwllr) with admin access to [https://discourse.gbif.org/](https://discourse.gbif.org/) to turn on comments for you.  


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


Making changes to the blog appearance 
------

Unfortunately, hugo does not document how to change a theme appearance very well. [This](https://bwaycer.github.io/hugo_tutorial.hugo/themes/customizing/) is the best resource I have found for how to edit a theme. 

>The following are key concepts for Hugo site customization. Hugo permits you to supplement or override any theme template or static file, with files in your working directory. When you use a theme cloned from its git repository, you do not edit the theme’s files directly. 

If you want to change the blog appearance, you need to create folders and files in the **root directory** that match folders and files in the **public directory**. Hugo will copy and replace files in the root directory into the public directory. 

### General blog settings 

Other settings in the blog can be set in the `./config.toml`. 

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

### How I added the logo to the blog

To add the logo by editing `./layouts/partials/header.html`. 

```
<div class="logo-wrapper">
  <a href="{{ "/" | relLangURL }}" class="logo">
	<img src= "{{ .Site.Params.logoSrc }}" alt="GBIF-analytics-blog" style ="width:20%;">
  </a>
</div>

```
I set the following variable in `config.toml`

```
logoSrc = "/logo.png"
```

### How I added author names 

I added author names by editting 

* `./layouts/post/single.html` 
* `./layouts/post/summary.html`

I added the following lines. 
```
<div class="post-author">{{ .Params.author }}</div>
```

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

### How I added discoure.org comments 

I connected the gbif community forum (https://discourse.gbif.org/) to the blog using the instructions found [here](https://meta.discourse.org/t/embedding-discourse-comments-via-javascript/31963). 

Following the instructions I added code below to `./layouts/partials/footer.html`: 

```
{{if eq .Section "post"}} 
<div id='discourse-comments'></div>
<script type="text/javascript">
  DiscourseEmbed = { discourseUrl: 'https://discourse.gbif.org/',
                     discourseEmbedUrl: 'https://data-blog.gbif.org{{ .URL }}' };

  (function() {
    var d = document.createElement('script'); d.type = 'text/javascript'; d.async = true;
    d.src = DiscourseEmbed.discourseUrl + 'javascripts/embed.js';
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(d);
  })();
</script>  
{{end}}
```
**NOTE!**
* I had to add `{{ .URL }}` to the **discourseEmbedUrl** to make this work. 
* I also added `{{if eq .Section "post"}}` so that comments would only appear at the bottom of post pages. 

Steps to take using discourse.gbif.org UI: 

1. I went to **Admin > Customize > Embedding**
2. Created New embeddable host
3. Set **Path Whitelist** to `/post/.*` 
4. Created new category **data-blog-preview** (staff-only)
5. Created new category **data-blog** (everyone)

How to make comments viewable to the general public: 
1. Comments at the bottom of the page will initially show "Embedding Failed"
2. To make comments viewable. In **discourse.gbif.org**, set category **data-blog-preview** to **data-blog**
3. You should now see something like "loading..."
4. Then if you re-fresh the page the comments should appear. 

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

