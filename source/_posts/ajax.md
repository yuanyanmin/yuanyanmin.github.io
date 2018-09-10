---
toc: true
title: ajax 学习笔记
date: 2018-08-27 10:45:05
tags: ajax
categories:
---

## 前言
Ajax 是 "Asynchronous Javascript And XML" 的缩写，中文译作"异步的JavaScript 和 XML"。使用Ajax可以通过Http协议与服务器交互数据，可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

<!--more-->

### 创建 XMLHttpRequest 对象

```
var xmlhttp;
if (window.XMLHttpRequest)
{
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
}
else
{
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
```

### 向服务器发送请求
我们使用呢 XMLHttpRequest 对象的 open() 和 send() 方法
```
open(method,url,async)
    method： 请求的类型; GET 或 POST
    url: 文件在服务器上的位置
    async: true(异步) 或 false（同步）

send(string)
    string： 仅用于POST请求
```

### 服务器响应
如果来自服务器的响应并非 XML,用 responseText 属性
如果来自服务器的响应是 XML,而且需要作为 XML 对象进行解析，则使用 responseXML s属性

```
responseText     // 获得字符串形式的响应数据
responseXML      // 获得 XML 形式的相应数据
```

### onreadystatechange 事件
当请求被发送到服务器时，我们需要执行一些基于响应的任务。
每当 readyState 改变时，就会触发 onreadystatechange 事件。
readyState 属性存有 XMLHttpRequest 的状态信息。

```
onreadystatechange
    存储函数，每当 readyState 属性改变时，就会调用该函数。
readyState
    XMLHttpRequest 的状态。从 0-4 发生变化。
    0： 请求未初始化
    1： 服务器连接已建立
    2： 请求已接收
    3： 请求处理中
    4： 请求已完成，且响应已就绪
status
    200： OK
    404:  未找到页面
```

在 onreadystatechange 事件中，我们规定当服务器响应已经做好被处理的准备时所执行的任务。
当 readyState 等于 4 且状态为 200 时，表示响应已就绪

```
xmlhttp.onreadystatechange = function() {
    if(xmlhttp.readyState === 4 && xmlhttp.status === 200) {
        document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
    }
}
```

### 总结

```
var xmlhttp;
if(window.XMLHttpRequest) {
    xmlhttp = new XMLHttpRequest();
}else {
    xmhttp = new ActiveXObject("Microsofo.XMLHTTP");
}

xmlhttp.open('GET',url,true);

xmlhttp.send();

xmlhttp.onreadystateChange = function() {
    if(xmlhttp.readyState === 4 && xmlhttp.status === 200) {
        document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
    }
}
```

## 对 Ajax 进行封装
```
/* 封装ajax函数
 * @param {string}opt.type http连接的方式，包括POST和GET两种方式
 * @param {string}opt.url 发送请求的url
 * @param {boolean}opt.async 是否为异步请求，true为异步的，false为同步的
 * @param {object}opt.data 发送的参数，格式为对象类型
 * @param {function}opt.success ajax发送并接收成功调用的回调函数
 */
function ajax(opt) {
    opt = opt || {};
    opt.method = opt.method.toUpperCase() || 'POST';
    opt.url = opt.url || '';
    opt.async = opt.async || true;
    opt.data = opt.data || null;
    opt.success = opt.success || function () {};
    var xmlHttp = null;
    if (XMLHttpRequest) {
        xmlHttp = new XMLHttpRequest();
    }
    else {
        xmlHttp = new ActiveXObject('Microsoft.XMLHTTP');
    }
    var params = [];
    for (var key in opt.data){
        params.push(key + '=' + opt.data[key]);
    }
    var postData = params.join('&');
    if (opt.method.toUpperCase() === 'POST') {
        xmlHttp.open(opt.method, opt.url, opt.async);
        xmlHttp.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded;charset=utf-8');
        xmlHttp.send(postData);
    }
    else if (opt.method.toUpperCase() === 'GET') {
        xmlHttp.open(opt.method, opt.url + '?' + postData, opt.async);
        xmlHttp.send(null);
    }
    xmlHttp.onreadystatechange = function () {
        if (xmlHttp.readyState === 4 && xmlHttp.status === 200) {
            opt.success(xmlHttp.responseText);
        }
    };
}s

// 使用实例

ajax({
    method: 'POST',
    url: 'test.php',
    data: {
        name1: 'value1',
        name2: 'value2'
    },
    success: function (response) {
       console.log(response);
    }
});
```

 PS: 附上[免费接口网址](https://blog.csdn.net/c__chao/article/details/78573737)
