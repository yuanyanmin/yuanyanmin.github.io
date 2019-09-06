---
title: hexo在多台电脑上提交和更新
date: 2019-09-06 09:57:15
tags: -随笔
categories:
toc:
---

本文主要记录了如何再多台电脑上进行博客的更新

<!--more-->

## hexo 搭建博客的原理

由于 github pages 存放的都是静态文件，博客存放的不只是文章内容，还有文字列表、分类、标签、翻页等动态内容。hexo 负责的就是将文章都放到本地，每次写完文章后调用相关命令批量完成相关页面的生成，然后将改动的页面提交到GitHub中。

**部署后生成的目录**
![](/photo/hexo多台电脑更新/hexo-deploy.jpg)


**本地目录**
![](/photo/hexo多台电脑更新/hexo-local.png)

## 多台电脑更新

1. 在 git 上新建分支 hexo
2. 将 本地文件 上传到 git 的 hexo 分支中
3. deploy 到 master 分支中
4. 其他电脑下载 hexo 分支代码
5. npm install... 等操作
6. 更新博客时，记得 git pull，同步最新代码 


