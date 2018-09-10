---
title: JS发表评论小例子
date: 2018-05-26 20:25:07
tags:
 - learning-JS
---

发表评论功能的实现

# 概述

1.先搭好框架，HTML、CSS
2.输入文字，点击 “发表” ，显示在下方
3.对输入字符进行判断。
4.实现删除功能。

<!--more-->

# 效果图


![](/photo/comment/js-comment.png)

# 实现过程

## HTML搭框架

```
    <div class="comment">
        <div class="box">
            <span>发表评论:</span>
            <textarea name="" id="my_textarea" cols="30" rows="10"></textarea>
            <button id = 'send'>发表</button>
        </div>
        <ul id = 'ul'>
        </ul>
    </div>
```

ul 用来 存放 评论列表，每次点击发表，即追加一个 li 标签。

## CSS美化

```
    <style>
        *{
            margin:0;
            padding:0;
        }
        .comment{
            width: 800px;
            border:1px solid #ccc;
            margin:100px auto;
            padding:20px;
        }
        #my_textarea{
            width: 80%;
            height: 120px;
        }
        .box{
            margin-bottom: 20px;
        }
        #ul{
            list-style-type: none;
            margin:0 80px;
        }
        #ul li{
            border-bottom : 1px dashed #ccc;
            line-height: 40px;
            color:red;
        }
        #ul li a{
            float: right;
        }
```

emmmm CSS随意发挥吧，贴的代码仅供参考。

## JS实现功能

### 首先获取所需要的标签

```
    var ul = document.getElementById('ul');            #获取存放评论的标签
    var send = document.getElementById('send');        #获取 发表 按钮
    var text = document.getElementById('my_textarea'); #获取文本框
```


### 给按钮添加事件

```
    send.onclick = function () {
            var tag = document.createElement('li');    #创建li标签

            tag.innerHTML = text.value + '<a href="javascript:;">删除</a>';   #给li标签加入内容，因为此处需要加入a标签，所以用 innerHTMl。

            ul.insertBefore(tag,ul.children[0]);   #追加li标签到第一个标签之前。这样保证新添加的评论在上面显示。

            text.value = '';      #添加完将文本框清空。
        }
```


### 实现删除事件

```
    //del
    var del = document.getElementsByTagName('a');   #获取所追加的a标签
    //console.log(del);

    for(var i = 0;i < del.length;i++){
        del[i].onclick = function(){             #闭包了解一下子。
            return function(){
                this.parentNode.remove();        #移除父元素 li。
            }
        }(i);
    }
```


### 对输入进行判断

```
   if(text.value.length == 0){               #如果输入内容为空，则弹出。
        alert("请输入评论的内容~");
        return;                              #注意一定要return。如果没有则还会执行下面的代码。
    }
```


## 整合JS代码

```
    <script>
        var ul = document.getElementById('ul');
        var send = document.getElementById('send');
        var text = document.getElementById('my_textarea');

        send.onclick = function () {
            //console.log(text.value);
            if(text.value.length == 0){
                 alert("请输入评论的内容~");
                return;
            }

            var tag = document.createElement('li');
            tag.innerHTML = text.value + '<a href="javascript:;">删除</a>';
            ul.insertBefore(tag,ul.children[0]);

            text.value = '';

            //del
            var del = document.getElementsByTagName('a');
            //console.log(del);

            for(var i = 0;i < del.length;i++){
                del[i].onclick = function(){
                    return function(){
                        this.parentNode.remove();
                    }
                }(i);
            }

        }
    </script>
```


# 总结

源代码已上传到GitHub，[点此查看](https://github.com/yuanyanmin/Code/blob/master/%E5%8F%91%E8%A1%A8%E8%AF%84%E8%AE%BA.html)。如果对你有帮助，star一下子。

本文是学习中记录的笔记，难免有错误和疏漏，欢迎大家指出。
