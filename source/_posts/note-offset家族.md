---
title: note-offset家族
date: 2018-06-01 16:29:42
tags: offset
categories:
---

offset 家族介绍。

offset可以方便的获取元素尺寸。

<!--more-->

offset：偏移，补偿  偏移的距离。

### offsetWidth ，offsetHeight  检测盒子的宽高

offsetHeight,offsetWidth 包括padding,border。不包括margin.

即：
```
    offsetHeight = height+padding+border;(不加margin)  
    offsetWidth = width+padding+border;(不加margin)  
```


### offsetTop , offsetLeft   返回距离上级盒子(最近的带有定位)上边/左边的位置

如果父级都没有定位则以body 为准
这里的父级指的是所上一级 不仅仅指的是 父亲 还可以是 爷爷 曾爷爷 曾曾爷爷。。。。

offsetTop,offsetLeft 从父级的padding开始算 border不算.
即 子盒子到定位的父盒子边框到边框的距离

**offsetTop与style.top区别：**

一、offsetTop 可以返回没有定位盒子的距离左侧的位置，
    如果父系盒子都没有定位，则以body为准.
    style.top 不可以，只有定位的盒子才有top,left等;

二、offsetTop  返回的是数字，而style.top返回的是字符串,XXpx。

三、offsetTop 只读，而 style.top即可以读还可以写。

四、若没有给HTML元素指定过top样式，则style.top 返回的是空字符串。

五、style.left 只能得到行内样式，offsetLeft 随便。


### offsetParent  返回该对象的父级 (带有定位)不一定是亲爸爸

offsetParent就是距离该子元素最近的进行过定位的父元素（position：absolute  relative fixed）
如果其父元素中不存在定位则offsetParent为：body元素

**offsetParent 与 parentNode 的区别：**

offsetParent 只读，离当前元素最近的一个有定位属性的父节点(有兼容问题)。
parentNode 只读，指当前元素的父级节点。

## 补充：

### clientX/clientY

clientX/clientY 返回事件触发时鼠标相对于 **元素视口** 的X/Y坐标

*这里的元素视口实际上代指就是浏览器，clientX是鼠标距离浏览器左边框的距离，clientY是鼠标距离浏览器上边框的距离。*

### screenX/screenY

screenX/screenY  返回事件触发时鼠标相对于 **屏幕** 的X/Y坐标

*screenX是鼠标距离显示器屏幕左边框的距离，screenY是鼠标距离显示器屏幕上边框的距离。*

### pageX/pageY

返回事件触发时鼠标相对于 **文档** 的X/Y坐标

*所谓文档可以简单的理解为浏览器中的页面内容，pageX是鼠标距离文档左侧的距离，pageY是鼠标距离文档上侧的距离，如果我们将鼠标悬停在浏览器中间，通过滚轮滚动浏览器，那么尽管没有移动鼠标的位置而pageY一直在变化，因为相对文档顶部的距离一直在变化。*


## 结语

本文是学习中记录的笔记，难免有错误和疏漏，欢迎大家指出。




