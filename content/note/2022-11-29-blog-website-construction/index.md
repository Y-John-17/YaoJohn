---
title: Blog-Website-Construction
author: Yao John
date: '2022-11-29'
slug: blog-website-construction
categories:
  - Website building skills
  - Hugo
  - R
tags:
  - Website
  - Blog
  - Technology
---

# Personal Blog Building

> **brief introduction**

- [hugo](https://github.com/gohugoio/hugo/releases) : The blog generation framework used in this article

-   [github](https://github.com/) : Blog dynamic remote storage warehouse (realize web page dynamic refresh)
    
-   [netlify](app.netlify.com) : Web hosting (Publication of web pages)
    
-   [Git](https://git-scm.com): Complete the remote warehouse download and push tool
    

## Environment Construction

> [R compiled language](https://www.r-project.org/)

-   Choose a close mirror image
    
-   Select an installation package for your operating system , and performing the installation
    

> [R studio](https://posit.co/products/open-source/rstudio/)

-   Download and installation
    

> [Hugo](https://github.com/gohugoio/hugo/releases)

-   You can choose to install it in your own operating system, or you can choose to install it in Rstudio (usually not recommended)
    

> [Github](https://github.com/)

-   Sign up a Github account
    
-   Create a new repository ( remember to ==Add a README file==)
    

> [netlify](netlify)

-   You can sign in with the github acount
    

## Blog Building

> There are three ways to do this

### The first way

> Open Rstudio

-   install.package('blogdown')
    

> Following Steps

-   `file` -->`New Project`-->`New Directory`-->selecting `Website using blogdown`among the options. next, type "Your blog directory name" at `Directory name`and "Your Hugo theme you liked (==Usual format: Author/subject name==)" at `Hugo theme`,at last, choose `Open in new session`-->`Create Project`
-   now, we already compeleted environment construction and file project for the blog creation

```R
 # preview the blog website   
`blogdown::server() `     

# If you want to change the theme of you blog webits,try doing so   
`blogdown::new_site( theme = "Author/subject name")` 

# New blogs
`Addins`-->`New Post`

# Finish building your website   
`blogdown::build_site()`  

# next step is to publish
```

### The second way

-   Firstly, copy your github repository's [code](URL) (This is used for cloning and push) `file` -->`New Project`-->`Version Control`-->`Git`,type your [repository URL](URL) and project directory name

```R
library(blogdown)

blogdown::new_site( theme = "Author/subject name")

blogdown::server()

blogdown::build_site()  

# next step is to publish
```



----------

### The third way

-   This is the simplest of the three methods
    
```R
# Create a new blog space
hugo new site "Your blog's name"  

# Download a hugo theme
git clone https://github.com/digitalcraftsman/hugo-agency-theme   themes/"theme's name"  

 # preview and build
hugo server -t hugo-agency-theme --buildDrafts

# Add a blog  
hugo new posts/blog.md

# construct framework
hugo --theme=hugo-agency-theme --baseUrl="https://lastknightcoder.github.io/" --buildDrafts

# check
cd public  
ls  
# next step is to publish
```

- Now, you can browse public, this folder contains all of your blog website files, [host this folder on [netlify](netlify) and you will be able to see your web blog generated](static). But this requires you to manually refresh the web content, let's try hosting web content on GitHub

## Host your website

- In this step , You need to use git to do this

`Open Git Bash`

-   `Set User.name`：git config --global user.name"`github's username`"
    
-   `Set User.eamil`：git config --global user.email "`register's email`"
    
```R
# Clone Repositroy
git clone "Your repositroy's URL"   
ls

cd "Your repositroy's name"   
cp /public/ .  /"Your repositroy's name"/public
### don't replace README.md   

git init   
git add .   
git commit -m "my first commit"   
git remote add origin "Your repositroy's URL"   
git push -u origin master #/(mian)
```
### open [netlify](netlify)

- `Add new site`-->`Import an existing project`-->choose GitHub-->selecting your repositroy

## Epilogue

The construction of a blog seems very simple, but we can see a lot of things from it. Its function is simply to give you a rigid website to record learning and memories, and it also reflects your growth. You should fill your blog including your life with efforts and sweat

```
 <p align="left">[netlify]:app.netlify.com</p>  
 <p align="left">[URL]:`open repository`-->`code`-->'HTTPS'</p>  
 <p align="left">[static]:`open netlify website`--> `Add new site`-->`Deploy manually` just wait a few minutes</p>
```

