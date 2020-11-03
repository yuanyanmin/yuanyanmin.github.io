---
title: keyup.enter 和 blur 事件冲突
date: 2020-11-03 18:11:18
tags: 随笔
categories:
toc:
---

**需求：** 实现输入框，输入值后，回车或者点击空白处实现保存，

**问题：** 会遇到一个问题就是，当敲回车的时候，不但会触发回车事件同时也会触发失去焦点的事件

**解决：**  `@keyup.enter="$event.target.blur`

```
<i-input
  @on-blur="updateName"
  @on-keyup.enter="$event.target.blur"></i-input>
```
