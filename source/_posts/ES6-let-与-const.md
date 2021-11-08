---
title: ES6 - let 与 const
date: 2021-11-08 16:38:03
tags: ES6
categories: ES6
toc:
---

开始重新学习ES6,并记录相关知识。首先学习基础的 let 与 const

<!--more-->

js 作用域包括 全局作用域和函数作用域。没有块级作用域的概念。let 实际上为 js 新增了块级作用域

>let 与 var 都是用于声明变量的, 但 let 声明的变量只在其代码块中有效

```
{
  let a = 10
  var b = 1
}

a  // ReferenceError...
b  // 1
```
**常见场景**

```
var a = []
for(let i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i)
  }
}
a[6]()  // 6


var a = []
for(var i = 0; i < 10; i++) {
  a[i] = function() {
    console.log(i)
  }
}
a[6]()  // 10
```

*解释：*

var 声明变量在全局都有效，所以全局只有一个变量 i。每一次循环， i 的值都会发生变化，console.log(i) 里面的 i 就是全局的 i。也就是所有数组a的成员里面的 a 都指向的是同一个 i。导致运行时输出的也是循环结束后的 i，即 10。

let 声明的i在每一个块级作用中生效，所以是 6

**let 不存在变量提升，var 存在变量提升**

```
console.log(foo) // 输出 undefined
var foo = 2

console.log(bar) // Error
let bar = 2
```

**暂时性死区**

```
var tmp = 123

if (true) {
  tmp = 'abc' // Error
  let tmp
}
```

*解释：*

暂时性死区，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

**块级作用域**

1、可解决变量覆盖

```
var tmp = new Data()

function f() {
  console.log(tmp)
  if (false) {
    var tmp = 'hello'
  }
}
fn() // undefined 因为变量提升 导致内部变量覆盖了外层的变量
```

2、用来计算的循环变量泄漏为全局变量

```
var s = 'hello'

for(var i = 0; i < s.length; i++) {
  console.log(s[i])
}

console.log(i) // 5
```

**块级作用域与函数声明**

ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域中声明

```
if (true) {
  function f() {}
}

try {
  function f() {}
} catch(e) {
  // ...
}
```

以上两种写法，根据ES5规定都是非法的，但为了兼容旧代码，以上代码实际都能运行。

ES6 中
 - 运行在块级作用域中声明函数
 - 函数声明类似于 var ，即会提升到全局作用域活函数作用域的头部
 - 同时，函数声明还会提升到所在的块级作用域的头部

```
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());

```

```
// ES5 环境

// 输出 I am inside。 因为 if 内声明的函数会被提升到函数的头部
// 实际运行的代码如下

function f() { console.log('I am outside!'); }

(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```

```
// ES6

// Error 
// 实际运行代码如下

function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```

**const**

const 声明一个只读的常量，一旦申明，值就不能改变

```
const PI = 3.1415
PI // 3.1415

PI = 3 // Error
```

const 声明的变量不得改变，这就意味着，const一旦声明变量，就必须立即初始化。

const 与 let 命令相同，只在声明的块级作用域中有效，变量不提升，存在暂时性死区

> const 本质：并不是值不能改动，而是变量指向的那个内存地址所保存的数据不得改动。对于普通类型的数据，值就保存在变量指向的那个内存地址中，因此等同于变量。对于复合类型的数据，变量指向内存地址，保存的只是一个指向实际数据的指针，const 只能保证这个指针是固定的，至于指向它的数据结构是不是可变的，就完全不能控制了

```
const foo = {}

foo.prop = 123
foo.prop // 123

foo = {} // Error
```
