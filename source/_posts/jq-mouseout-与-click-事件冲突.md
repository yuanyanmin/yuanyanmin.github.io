---
title: jq mouseout 与 click 事件冲突
date: 2018-12-18 16:13:15
tags: 
 - jq
categories:
toc:
---

#### 前言

问题描述：鼠标移上去，添加背景颜色，移开，背景颜色消失。点击时，添加背景颜色。但每次点击后，移走鼠标，背景颜色就消失了。:frowning:

<!--more-->

#### 代码

```
		$(".img img").on("click",function () {

			$(this).unbind('mouseout');

			$(this).siblings().removeClass('current');
			$(this).addClass('current');

		}).on("mouseover",function (e) {
			$(this).addClass('current');
		}).on("mouseout",function (e) {
			$(this).removeClass('current');
		})

```


**重点**

```
$(this).unbind('mouseout');
```


查文档可知

> **unbind() 方法**：移除备选元素的事件处理程序
>
>>**取消绑定元素的事件处理程序和函数**
>>>**语法**： $(selector).unbind(event,function)
>>>规定从指定元素上删除的一个或多个事件处理程序,若没有规定参数，则会删除指定元素的所有事件处理程序
>>>**event**: 可选，规定从指定元素上删除的一个或多个事件处理程序，由空格分隔多个事件值。
>>>**function**: 可选，规定从元素的指定事件取消绑定的函数名。
>>
>>**使用 Event 对象来取消绑定事件处理程序**
>>>**语法**：$(selector).unbind(eventObj)
>>>规定要删除的事件对象。用于对自身内部的事件取消绑定（比如当事件已被触发一定次数之后，删除事件处理程序）。如果未规定参数，则 unbind() 方法会删除指定元素的所有事件处理程序。
>>>**eventObj**:可选，规定要使用的事件对象，这个 eventObj 参数来自事件绑定函数。