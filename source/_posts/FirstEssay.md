toc: true
title: Hexo 初次学习
date: 2018-05-25 15:30:14
tags: [Hexo,Git,node.js]
---
Learing MarkDown and build a personal blog with hexo.

# 前言

很早之前就想着做一个自己的博客，在了解了Git后终于知道了怎么实现。故...

<!-- more -->

# 正文
---
## 安装node.js

我的电脑是window，所以下来的均以window为环境。
首先登陆[node.js官网](https://nodejs.org/en/),选择合适的版本，下载安装。

## 安装git客户端

登陆[git官网](https://git-scm.com/),选择合适的版本下载安装。Git安装教程可以看[廖雪峰的官方网址](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000),或者自己搜其他博客。

# 搭建博客

再安装完node.js 和 git 后,开始搭建博客。

首先建立一个Blog文件夹，用于存放相关文件。右键选择Git Bash Here进入git的命令行。

## 安装hexo

```
    $ npm install -g hexo-cli
```

## 初始化

```
    $ hexo init    #初始化hexo环境
```

hexo 会自动生成一个目录结构。如下图；

![](/photo/first/first_1.png)

依次执行下面的命令

```
    $ npm install    #安装npm依赖包
    $ hexo generate  #生成静态页面
    $ hexo server    #生成本地服务
```

打开浏览器输入 http://localhost:4000/ 即可访问自己的博客。

## 目录介绍

**_config.yml**

&nbsp;&nbsp;&nbsp;&nbsp;全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数。

**package.json**

&nbsp;&nbsp;&nbsp;&nbsp;hexo 框架的参数

**scaffolds**

&nbsp;&nbsp;&nbsp;&nbsp;当新建一篇文章的时候，hexo根据这个目录下的文件进行构建。

**script**

&nbsp;&nbsp;&nbsp;&nbsp;脚本文件，此目录下的Javascript文件会自动被执行。

**source**

&nbsp;&nbsp;&nbsp;&nbsp;这个很重要，新建的文章都是保存在这个目录下的，有两个目录：_drafts,_posts。需要新建的博文都放在_posts目录下。

&nbsp;&nbsp;&nbsp;&nbsp;_posts目录下是一个个markdown文件。你应该可以看到一个hello-world.md的文件，文章就在这个文件中编辑。(ps:请自学markdown 语法)

&nbsp;&nbsp;_posts目录下的md文件，会被编译成html文件，放到public（此文件现在应该没有，因为你还没有编译过）文件夹下

**themes**

&nbsp;&nbsp;&nbsp;&nbsp;网站的主题目录。每一个子目录就是一个主题，可以自己下载自己喜欢的主题。

## 修改主题

我使用的是 [yilia](https://github.com/litten/hexo-theme-yilia),直接从GitHub 上下载压缩包解压到themes 文件夹

或者使用git 命令 clone 到themes文件夹

```
    $ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

然后修改根目录 _config.yml中的 theme:landscape 改为： themes: yilia.

然后执行 hexo generate 重新生成。如果出现一些问题，可以先执行hexo clean清理一下public内容，然后重新生成。

执行hexo server  就可以在本地查看到新主题的博客了。

## 将本地博客发布到Github上

### 创建远程仓库

创建的仓库 yourname.github.io 。yourname 为自己github的名字。

### 与连接GitHub仓库

编辑根目录下的_config.yml,配置其中 deploy 的部分。

```
    deploy:
        type: git
        repo: https://github.com/yourname/yourname.github.io.git
        branch: master
```

然后gitBash,执行

```
    $ npm install hexo-deployer-git --save
```

最后提交自己的博客

```
    $ hexo deploy
```

在浏览器中输入 https://yourname.github.io/ 即可看到自己搭建的博客了。

### 常用的hexo 命令

```
    hexo new "postName"  #新建文章
    hexo new page "pageName" #新建页面
    hexo generate   #生成静态页面至public目录
    hexo server  #开启预览访问端口
    hexo deploy  #部署到Github
    hexo help    #查看帮助
    hexo clean   #清除缓存
    hexo version  #查看hexo 版本
```

组合命令

```
    hexo s -g   #生成并本地预览
    hexo d -g   #生成并上传
```

----

# 结语

本文是我搭建过程中的一些记录，难免有错误和疏漏，欢迎大家指出。



