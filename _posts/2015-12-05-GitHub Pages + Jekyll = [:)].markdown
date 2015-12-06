---
layout: post
title:  "GitHub Pages + Jekyll = [:)]"
date:   2015-12-05 20:45:05
categories: feature
tags: featured
image: /assets/article_images/jekyll.jpg
---
#GitHub Pages + Jekyll = [:)]

>####Learn how GitHub and Jekyll works and how they can help you make static websites without database and easier to manage.

What are GitHub Pages?
----------------  

GitHub Pages are public webpages hosted and published through our site. User, Organization, and Project Pages. There are two basic types of GitHub Pages: User/Organization Pages and Project Pages. They are nearly identical, but there are a few important differences between them.

- User & Organization Pages live in a special repository dedicated to GitHub Pages files. You will need to name this repository with the account name.

- Unlike User and Organization Pages, Project Pages are kept in the same repository as their project. Both personal accounts and organizations can create Project Pages.

####Free hosting with GitHub Pages. 
Sick of dealing with hosting companies? GitHub Pages are powered by Jekyll, so you can easily deploy your site using GitHub for free—custom domain name and all.
[Learn more about GitHub Pages](https://pages.github.com)

What is Jekyll?
----------------  
The Jekyll is a generator of static code. The idea is that you create pages and even a statically blog using HTML with a few tricks that will help you convert your website into static files, ready to be published.
It is based on various formats like [Markdown](https://en.wikipedia.org/wiki/Markdown) formatting to text and posts and a template pattern called Liquid with a bit of [YAML](yaml.org) to view and save the data of the variables.

Using Jekyll
----------------  
Every GitHub Page runs through Jekyll when you push content to a specially named branch within your repository. Jekyll is a simple, blog-aware, static site generator. It takes a template directory containing raw text files in various formats, runs it through a converter (like Markdown) and our Liquid renderer, and spits out a complete, ready-to-publish static website suitable for serving with your favorite web server.

Jekyll also happens to be the engine behind GitHub Pages, which means you can use Jekyll to host your project’s page, blog, or website from GitHub’s servers for free.
[Learn more about Jekyll](https://jekyllrb.com/docs/home/)

####Installing Jekyll
We highly recommend installing Jekyll on your computer to preview your site and help diagnose troubled builds before publishing your site on GitHub Pages.To ensure your computer most closely matches the GitHub Pages settings, you can use the GitHub Pages Gem, which downloads the dependencies you need. 
[Learn more about Jekyll install] (https://help.github.com/articles/using-jekyll-with-pages/)

Code Structure
----------------  
The code structure of the files is very simple to understand, but for some it can be a bit strange not to have familiarity with data structures such as YAML. But it is simple and you learn fast, I'm sure. Continue reading for you to see how easy it is.

####Without Database
For starters, you do not maintain a database. The content of your site to be stored on each page files. You do not need to lift a MySQL server for example. All site information will be stored in files that you create for each page.

####Liquid and YAML
The YAML format is known for ease of reading. It is designed to be easy to understand the people and also write. That is, it is a simple format might write by hand, but also to manipulate programmatically. This is where the Jekyll starts to get cool. This structure is also used in the [Middleman](https://middlemanapp.com) and [DocPad](docpad.org). So learning here, you will already know roughly how the other generators.

####As somethings work
Any file that contains a YAML block - the Jekyll staff calls front-matter - will be treated as a special file. The front-matter must be the first thing in the file and must be a valid YAML format. Every page of your website done in Jekyll must start with this structure:

```yaml
---
layout: page
title: About Page
permalink: /about/
---
```
####Directory structure
The whole thing is very simple: all the file that you have _ (underscore) in front of the name, Jekyll will ignore in the final package, when you convert your project. See a structure of one of our projects:

[Structure]: https://github.com/robsonfagundes/robsonfagundes.github.io/blob/master/assets/images/structure.png

