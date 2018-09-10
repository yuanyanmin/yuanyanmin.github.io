---
toc: true
title: Hexo 实现文章目录
date: 2018-05-25 18:51:14
tags: [Hexo,Git,node.js]
---

Implement the article directory.

# 前言

本文简单结束如何给每个文章添加目录。

<!-- more -->


# 正文
---

## 添加CSS样式

在 themes\yilia\source 下的 main.0cf68a.css 文件，添加如下代码

```
/* 新添加的 */
#container .show-toc-btn,#container .toc-article{display:block}
.toc-article{z-index:100;background:#fff;border:1px solid #ccc;max-width:250px;min-width:150px;max-height:500px;overflow-y:auto;-webkit-box-shadow:5px 5px 2px #ccc;box-shadow:5px 5px 2px #ccc;font-size:12px;padding:10px;position:fixed;right:35px;top:129px}.toc-article .toc-close{font-weight:700;font-size:20px;cursor:pointer;float:right;color:#ccc}.toc-article .toc-close:hover{color:#000}.toc-article .toc{font-size:12px;padding:0;line-height:20px}.toc-article .toc .toc-number{color:#333}.toc-article .toc .toc-text:hover{text-decoration:underline;color:#2a6496}.toc-article li{list-style-type:none}.toc-article .toc-level-1{margin:4px 0}.toc-article .toc-child{}@-moz-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-webkit-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@-o-keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}@keyframes cd-bounce-1{0%{opacity:0;-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}60%{opacity:1;-o-transform:scale(1.01);-webkit-transform:scale(1.01);-moz-transform:scale(1.01);-ms-transform:scale(1.01);transform:scale(1.01)}100%{-o-transform:scale(1);-webkit-transform:scale(1);-moz-transform:scale(1);-ms-transform:scale(1);transform:scale(1)}}.show-toc-btn{display:none;z-index:10;width:30px;min-height:14px;overflow:hidden;padding:4px 6px 8px 5px;border:1px solid #ddd;border-right:none;position:fixed;right:40px;text-align:center;background-color:#f9f9f9}.show-toc-btn .btn-bg{margin-top:2px;display:block;width:16px;height:14px;background:url(http://7xtawy.com1.z0.glb.clouddn.com/show.png) no-repeat;-webkit-background-size:100%;-moz-background-size:100%;background-size:100%}.show-toc-btn .btn-text{color:#999;font-size:12px}.show-toc-btn:hover{cursor:pointer}.show-toc-btn:hover .btn-bg{background-position:0 -16px}.show-toc-btn:hover .btn-text{font-size:12px;color:#ea8010}

.toc-article li ol, .toc-article li ul {
    margin-left: 30px;
}
.toc-article ol, .toc-article ul {
    margin: 10px 0;
}
```

## 修改article.ejs 文件

修改themes\yilia\layout\_partial 文件夹下的 article.ejs 文件

```
        <!-- 目录内容 -->
        <% if (!index && post.toc){ %>
            <p class="show-toc-btn" id="show-toc-btn" onclick="showToc();" style="display:none">
            <span class="btn-bg"></span>
            <span class="btn-text">文章导航</span>
            </p>
            <div id="toc-article" class="toc-article">
                <span id="toc-close" class="toc-close" title="隐藏导航" onclick="showBtn();">×</span>
                <strong class="toc-title">文章目录</strong>
                <%- toc(post.content) %>
           </div>
           <script type="text/javascript">
            function showToc(){
                var toc_article = document.getElementById("toc-article");
                var show_toc_btn = document.getElementById("show-toc-btn");
                toc_article.setAttribute("style","display:block");
                show_toc_btn.setAttribute("style","display:none");
                };
            function showBtn(){
                var toc_article = document.getElementById("toc-article");
                var show_toc_btn = document.getElementById("show-toc-btn");
                toc_article.setAttribute("style","display:none");
                show_toc_btn.setAttribute("style","display:block");
                };
           </script>
        <% } %>
        <!-- 目录内容结束 -->
```

## 显示

如果想在文章中显示目录，则在每篇文章开头中加入

```
top: true
```

# 最终效果

![](/photo/second/result.png)


