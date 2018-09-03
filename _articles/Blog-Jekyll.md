---
layout: article
title: Create a static blog with Jekyll
category: Web_Dev
tag: [Jekyll, GitHub, Bootstrap]
date: 2017-05-25
---

## Introduction

This article documents the development of a blog as a static website.  

The source code of the project will be hosted in GitHub to use GitHub Pages, a static site hosting service using 
Jekyll engine.Jekyll is a static site generator processing HTML, Markdown and Liquid markups that can be used to 
manage content stored as data objects in plain text files (here using JSON format) rather than relying on a 
database system.  
The front-end of the blog will be designed using Boostrap, a front-end web framework natively supported by Jekyll.
  
The site presented is the blog containing this article itself.


## Structure of the project repository

* **_include** directory contains files of partial page elements.
  * The head section, the navbar and the footer are injected into the default layout of the website.
  * Can be included into other files using liquid tag markup  
  ```
  { % include fileName.html % }
  ```
* **_layout** directory contains files of HTML templates.
  * The default layout of the website is designed in the file default.html and used by all pages.
  * **index.html** and all other main static pages are HTML partials using the default layout of the website
  * Layout templates are used by indicating the layout to use in the YAML Front Matter of a page.
  ```
  ---
  layout: fileName  
  ---
  ```
  * The location of a content injected into a layout file is indicated by the use of a Liquid tag markup.
  ```
  { { include } }
  ```
* **_posts** directory contains the files of each blog post content to inject into layout templates
  * Files must respect a naming convention
  ```
  YEAR-MONTH-DAY-title.MARKUP
  ```
* **_articles** directory is user-defined collection directory holding articles to inject into the article layout template
  * User-defined collections can be defined in "_config.yml" file to behave like "_posts" directory
   ```
    collections:
      articles:
        output: true
    ```
  * Unlike the posts in "_posts" directory, the elements of the collection don't need to be prefixed by their creation date
  * A listing of the articles in the **_articles** directory is done using a for loop given as Liquid tag markup
  ```
  { % for article in site.articles % }
    do actions using Liquid output markup { { article.attributeName } }
  { % endfor % }
  ```
  * Liquid Tag 'raw' and 'endraw' markups can be used when writing about a Liquid markups in an article
  * Code snippets can be highlighted using Liquid 'highlight' tag markup
  ```
  { % highlight ruby linenos % }
  write Ruby code
  { % endhighlight % }
  ```  
* **_data** directory contains the data files in which are stored data objects following different possible format
  * In this project, the JSON format is the only used
  * The data objects are accessible from any other files by using a Liquid output markup
  ```
  { { site.data.fileName.attributeName } }
  ```
* **assets** directory contains static files used in the projects
  * Static files are files that don't use YAML Front Matter
  * Front matter values can be set to static files in the **_config.yml** configuration file
  
