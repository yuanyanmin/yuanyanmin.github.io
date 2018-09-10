---
title: clock
date: 2018-05-29 13:52:01
tags: 
 - learning-JS
categories:
---

利用定时器实现[时钟](https://yuanyanmin.github.io/Code/%E6%97%B6%E9%92%9F%E6%A1%88%E4%BE%8B.html)的功能

# 概述

1.先画好时钟，时针、分针、秒针等。
2.运用定时器，获取当前时间，并计算时分秒。
3.让针转动。

<!--more-->

# 效果图

[点此查看](https://yuanyanmin.github.io/Code/%E6%97%B6%E9%92%9F%E6%A1%88%E4%BE%8B.html)

# 正文

## 画时钟，需要时针、分针、秒针的图片。[点此自取](https://github.com/yuanyanmin/Code/tree/gh-pages/img/clock)

```
    <div id="box">
        <div id="hour"></div>
        <div id="min"></div>
        <div id="second"></div>
    </div>
```

css

```
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        #box {
            width: 600px;
            height: 600px;
            background:url(img/clock/clock.jpg);
            margin:10px auto;
            position: relative;
        }
        #hour,#min,#second {
             position: absolute;                      //子绝父相
             top: 0;
             left: 50%;
             width: 30px;
             height: 600px;
             margin-left: -15px;
        }
        #hour {
            background:url(img/clock/hour.png);
        }
        #min {
            background:url(img/clock/minute.png);
        }
        #second {
            background:url(img/clock/second.png);
        }
    </style>
```


## 开启定时器，获取当前时间，并计算时分秒

```
    <script>
        window.onload = function () {
            // 1. 获取需要的标签
            var hour = document.getElementById("hour");
            var min = document.getElementById("min");
            var second = document.getElementById("second");

            // 2.开启定时器
            setInterval(function () {
                 // 2.1 获取当前的时间戳
                var date = new Date();

                 // 2.2 求出总毫秒数
                var s = date.getSeconds();
                var m = date.getMinutes();
                var h = date.getHours() % 12;     //因为表盘只有12个数字，所以 % 12

                // console.log(s);
                // console.log(m);
                // console.log(h);

            }, 10);
        }
    </script>
```


## 让针转动

```
     // 2.3 旋转
     hour.style.transform = 'rotate('+ h * 30 +'deg)';   //360deg / 12 = 30deg
     min.style.transform = 'rotate('+ m * 6 +'deg)';     //360deg / 60 = 6deg
     second.style.transform = 'rotate('+ s * 6 +'deg)';  //360deg / 60 = 6deg
```

## 优化

此时，我们发现[效果图](/photo/clock/clock.png)
应为 下午 2：39。但是时针正对着数字2，按照习惯来说应该在2和3之间。

```
     // 2.2 求出总毫秒数
     var millS = date.getMilliseconds();
     var s = date.getSeconds() + millS / 1000;
     var m = date.getMinutes() + s / 60;
     var h = date.getHours() % 12 + m / 60;
```


# 总结

源代码已上传到GitHub，[点此查看](https://github.com/yuanyanmin/Code/blob/gh-pages/%E6%97%B6%E9%92%9F%E6%A1%88%E4%BE%8B.html)。如果对你有帮助，star一下子。

本文是学习中记录的笔记，难免有错误和疏漏，欢迎大家指出。