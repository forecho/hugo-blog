---
title: "怎样把本地项目上传到 GitHub 上去？"
date: 2013-12-08T19:11:00+08:00
categories: 
draft: false
toc: true
---

先进入项目根文件夹 
    
    
    $ cd demo
    $ git init
    $ git add .
    $ git commit -m "新建项目"

到这一步时，你要去你的 GitHub 上新建一个项目，命名为 demo （注意不要选择第三个 - 使用README 文件初始化仓库） 
    
    
    $ git remote add origin git@github.com:你GitHub的用户名/demo.git
    $ git push -u origin master