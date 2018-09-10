---
title: note-scroll家族
date: 2018-06-05 15:25:45
tags: scroll
categories:
---

scroll 家族介绍。

网页正文全文宽： document.body.scrollWidth;

网页正文全文高： document.body.scrollHeight;

网页被卷去的高： document.body.scrollTop;

网页被卷去的左： document.body.scrollLeft;

<!--more-->

## 处理scroll家族浏览器适配问题

### ie9+ 和 最新浏览器

```
    window.pageXOffset; (scrollLeft)
    window.pageYOffset; (scrollTop)
```

### Firefox浏览器 和 其他浏览器

```
    document.documentElement.scrollTop;
```

### Chrome浏览器 和 没有声明 DTD <DOCTYPE>

```
    document.body.scrollTop;
```

### 兼容写法

```
    var scrollTop = window.pageYOffset || 
                    document.documentElement.scrollTop ||
                    document.body.scrollTop ||
                    0;
    var scrollLeft = window.pageXOffset ||
                     document.documentElement.scrollLeft ||
                     document.body.scrollLeft ||
                     0; 
```

## scrollTo(x,y)

把内容滚动到指定的坐标

格式：scrollTo(xpos,ypos)

xpos 必需；要在窗口文档显示区左上角显示的文档的 x 坐标；

ypos 必需；要在窗口文档显示区左上角显示的文档的 y 坐标 。

网页大部分都没有水平滚动条，所以，这个x 不太常用。

## 结语

本文是学习中记录的笔记，难免有错误和疏漏，欢迎大家指出。