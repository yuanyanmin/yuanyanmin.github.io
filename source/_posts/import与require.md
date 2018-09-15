---
title: import 与 require
date: 2018-09-15 16:18:19
tags:
 - node
 - 随笔
categories:
---

node 的模块化编程思想，import 与 require 都是被模块化所使用。

- require 是 AMD 规范引入方式  运行时加载
- import  是 es6 的语法标准    编译时加载 

这这这，这种当然是看[大佬们的博客](!http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)了.

<!--more-->

## require

在导出的文件中定义 module.export, 导出的对象的类型可以是任何类型（字符串，变量，对象，方法），在引入的文件中直接调用 require() 方法引入对象即可。

```
// a.js 
module.export = {
    a: function() {
        console.log("hello");
    }
}

// b.js
var obj = require('../a.js');
obj.a();   // "hello";
```



## import

导出的对象与模块的值一一对应，即导出的对象与整个模块进行结构赋值。

```
// a.js
// 加入 default 表示在 import 时可以使用任意变量名并且不需要{}
export default {      
    a: function() {
        console.log("hello");
    }
}

// 导出函数
export function() {

}

// 解构赋值语法(as关键字在这里表示将newA作为a的数据接口暴露给外部，外部不能直接访问a)

export {newa as a, b, c}

//b.js

//import常用语法（需要export中带有default关键字）可以任意指定import的名称
 import a from '...'  

 // 基本方式，导入的对象需要与export对象进行解构赋值
 import {...} from '...'  

// 使用as关键字，这里表示将a代表hello引入（当变量名称有冲突时可以使用这种方式解决冲突）
 import a as hello from '...'  

// as关键字的其他使用方法
 import {a as hello,b,c} 


```

[阮一峰es6](!http://es6.ruanyifeng.com/#docs/module-loader)

[转自](!https://www.imooc.com/article/22371?block_id=tuijian_wz)