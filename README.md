
![](https://raw.githubusercontent.com/gbif/data-blog/master/static/logo.png)

The GBIF [data blog](https://data-blog.gbif.org/) is a [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) blog generated using [hugo](https://gohugo.io/) and currently hosted using [Netlify](https://www.netlify.com/).  The blog uses a slightly modified theme called [even](https://github.com/olOwOlo/hugo-theme-even).  

### TLDR recommended blog-writing workflow 

1. Write a post in markdown named like `YYYY-MM-DD-your-post.md` [blog post template here](https://raw.githubusercontent.com/gbif/data-blog/master/templates/2025-09-24-template.md)
2. Preview with a simple online [markdown editor](https://stackedit.io/app#)
3. Save plots or images into an image folder named like `./content/post/YYYY-MM-DD-your-post_files`

```
# embed with this template
![example](/post/YYYY-MM-DD-your-post_files/plot1.jpg)
```

4. Copy/paste [header text](https://github.com/gbif/data-blog/blob/master/templates/header.txt) into your `.md` file 
```
---
title: Your post
authors: Your Name
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

```
5. Edit important fields in header text: 
 
 - `title: Your post` <- your title 
 - `authors: Your Name` <- your author name
 - `date: '2018-09-21'` <- the order the post will appear on blog. Does not necessarily have to be the same as the date in  `YYYY-MM-DD-your-post.md`. 
 - `slug: your-post` <- name that will appear in the url https://data-blog.gbif.org/post/your-post/ 
 - `tags:` <- category tags for post
 - `hiddenFromHomePage: yes` <- keeps people from seeing your post as you work on it

6. Clone the repository locally `git clone https://github.com/gbif/data-blog.git`
7. Copy `YYYY-MM-DD-your-post.md` and `YYYY-MM-DD-your-post_files` into `./content/post/`
```
.
├───content
    ├───post
        ├───YYYY-MM-DD-your-post.md
        └───YYYY-MM-DD-your-post_files <- put images here
            └───plot1.jpg
```
8. Push your changes to **gbif/data-blog** (or create pull request or edit with github ui). 
9. Get [someone](https://github.com/jhnwllr) with admin access to **gbif/data-blog** to invite you as a collaborator (**Settings** > **Collaborators & teams**).
10. Check your post at https://data-blog.gbif.org/post/your-post/ (Netlify will build your post automatically)
11. **Find mistakes** 
12. Fix mistakes and push changes to **gbif/data-blog** until satisfied. 
13. Switch to `hiddenFromHomePage: no`
14. Ask [someone](https://github.com/jhnwllr) with admin access to [https://discourse.gbif.org/](https://discourse.gbif.org/) to turn on comments for you.  

### Writing a post in markdown

Posts should be written in markdown. If you are unfamiliar, you can use some of the links below. 

* [cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [online editor](https://stackedit.io/app#)

Markdown files are just simple text files with an `.md` extension. 

You can download a simple `.md` [blog post template here](https://raw.githubusercontent.com/gbif/data-blog/master/templates/2025-09-24-template.md) 

### Post Template

Posts should have a specific header text. [blog post template here](https://raw.githubusercontent.com/gbif/data-blog/master/templates/2025-09-24-template.md) 
 

```
---
title: Your post
authors: Your Name
date: '2018-09-21'
slug: your-post
categories:
  - GBIF
tags:
  - test tag
  - test
lastmod: '2018-09-21T15:24:11+02:00'
keywords: []
description: ''
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

# Some markdown header text  
I am some text. 

```

Important fields to **fill in** are below: 

```
title: Your post
author: Your Name
date: '2018-09-21'
slug: your-post
categories:
  - GBIF
tags:
  - test tag
  - test

(The rest of the header text)

```
And the rest of the fields can be left the same. I am not really sure if they are all needed, but just to not break the theme, I would **leave them in**. You can probably **ignore** the `lastmod field:`. 

You can download a simple `.md` blog post template `./content/post/2025-09-24-template.md` [link here](https://github.com/gbif/data-blog/blob/master/templates/2025-09-24-template.md). 

### How hugo works

[Hugo](https://gohugo.io/) is a simple static site generator that will turn `.md` files into nicely styled html files. 

### Getting your post on the blog 

A post can be done by simply copying an `.md` into the **gbif/data-blog** `./content/post/` directory. 

```
.
├───content
    └───post <- put YYYY-MM-DD-your-post.md here
```

I am still working out the best way to do this in practice, but see the **TLDR recommended blog-writing workflow**. 

Some options: 

* Send me the `YYYY-MM-DD-your-post.md` file **jwaller@gbif.org**
* Fork the repository and add the `YYYY-MM-DD-your-post.md`, then do a pull request. 
* If you have access to **github.com/gbif/data-blog**, just update the repository yourself. 

After `YYYY-MM-DD-your-post.md` is in the `./content/post/` directory, [Netlify](https://www.netlify.com/) will build the blog automatically, the theme will be applied, and **your post will be online** at  https://data-blog.gbif.org/post/your-post/. Where **/your-post/** is the **slug** you gave your post in the header text.  

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

### Previewing a post

You can preview the post using any markdown [online editor](https://stackedit.io/app#). 

If you want to see the post with the blog theme applied, **but don't want to post to appear yet on the home page**, you can set `hiddenFromHomePage` to **yes** in the header text. 

```
hiddenFromHomePage: yes
```
You will have to set this to **no** for the blog to appear on the home page. The default is **no**. You will still be able to visit your blog post but outsiders would have to guess your title to see it (or navigate with the arrows).  

### Locally serving the site using hugo

You can download and serve the blog locally using [hugo server](https://gohugo.io/commands/hugo_server/). **This is probably not necessary.** I would just ask to become a collaborator before trying to do this as netlify will build the post automatically. 

### Writing a post in Rmarkdown

It is also possible to write a post in [Rmarkdown](https://rmarkdown.rstudio.com/lesson-1.html), and since this is a data blog some might want to contribute using **Rmarkdown**. 

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
9. Blog should should rebuild automatically

Unless you edit a previously built `.Rmd`, you should not need to install any additional R packages (except the ones you use in your post). 

### Inserting images into an Rmarkdown post

If you want to insert an image into an Rmarkdown post, but this image **is not** generated by a plot and **not from** an external source, you should put files into a folder like  `./static/post/2025-09-24-template_files/` rather than `./content/post/2025-09-24-template_files/`. This seems to be blogdown specific. Probably if you named your folder something other than `YYYY-MM-DD-title_files`, you could put it in the **content** folder, but it is probably better to keep all the R associated static files together. 

### Hightlight js

The blog comes with syntax highlighting from the `highlight.js` library. 

Currently it supports the following languages:

* Apache  
* Bash  
* C#  
* C++  
* CSS  
* CoffeeScript  
* Diff  
* HTML
* XML
* HTTP
* Ini
* TOML
* JSON
* Java
* JavaScript
* Makefile
* Markdown
* Nginx
* Objective-C
* PHP
* Perl
* Properties
* Python
* Ruby
* R
* SQL  
* Shell 
* Session

You can add support for a language **not in the list above** by downloading a custom package from https://highlightjs.org/download/. Then replace the existing `highlight.pack.js` file with the downloaded one in `./static/lib/highlight/`. 

Additionally, to make the language **name** appear at the top of the code block, instead of "code" add something like the following to `/static/dist/even.min.css`. 

```
.post .post-content figure.highlight.language-r>table:after {
    content: "R"
}

```

See and example post using highlight.js [here](https://github.com/gbif/data-blog/blob/master/templates/highlight-js.md). 


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
HUGO_VERSION: 0.42
```

