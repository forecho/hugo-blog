---
title: "Git 分支的应用"
date: 2013-12-08T22:14:00+08:00
categories: 
draft: false
toc: true
---

**创建分支**( modify-README 为分支名)： 
    
    
    $ git checkout -b modify-README

注：创建完之后默认切换到这个分区。 **列出所有的分支：**
    
    
    $ git branch

** 合并到主分区：**
    
    
    $ git checkout master
    $ git merge modify-README

注：先切换到主分区，再合并。 **删除分区：**
    
    
    $ git branch -d modify-README