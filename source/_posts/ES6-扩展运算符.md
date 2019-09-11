---
title: ES6 扩展运算符
date: 2019-09-11 17:44:46
tags: ES6
categories:
toc:
---

本文主要用于记录 ES6 的扩展运算符 ...

<!--more-->

### 对象的扩展运算符

> 对象中的扩展运算符（...）用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中

```
let test1 = {a: 1, b: 2};
let test2 = {c: 3, d: 4};

let test3 = {...test1, ...test2}    // {a: 1, b: 2, c: 3, d: 4}
```

上述代码等价于：

```
let test1 = {a: 1, b: 2};
let test2 = {c: 3, d: 4};

let test3 = Object.assign({}, test1, test2);     // {a: 1, b: 2, c: 3, d: 4}
```

**Object.assign** 方法用于对象的合并，将源对象source的所有可枚举属性，复制到目标对象target。第一个参数为目标对象，后面的参数都是源对象。若目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

**Object.assign 属于深拷贝还是浅拷贝呢？**

> 当是第一级属性时是深拷贝，以后级别属于浅拷贝

```
let a = {name: {firstName: 'yuan'}};
let b = Object.assign({}, a);

b.name.firstName = 'yun';
console.log(a,b);      // {name: {firstName: 'yun'}} {name: {firstName: 'yun'}}

let c = {name: {firstName: 'yuan'}};
let d = Object.assign({}, a);

d.name = 'yun';
console.log(c,d);      // {name: {firstName: 'yun'}} {name:  'yun'}
```

[对象的扩展运算符参考链接](!http://es6.ruanyifeng.com/#docs/object#%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6)

**那么 展开运算符... 算作什么拷贝呢**

> 扩展运算符对数组或对象进行拷贝时，只能扩展和拷贝第一层的值，对于第二层极其以后的值，扩展运算符不能将其进行扩展，也不能对其进行深拷贝，即拷贝后和拷贝前第二层中的对象或者数组仍然引用的是同一个地址，其中一方改变，另一方也跟着改变，故其为浅拷贝。


**深拷贝与浅拷贝的区别**

> 简单来说，假设 B 复制了 A，当 A 修改时，B也跟着变化，则为浅拷贝，若不变，则为深拷贝。

[深拷贝与浅拷贝参考链接](!https://www.cnblogs.com/echolun/p/7889848.html)

### 数组的扩展运算符

1.可以将一个数组转为用逗号分隔的参数序列

```
console.log(...[1,2,3])     // 1 2 3

// 常用于函数
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}
const numbers = [4, 5];
add(...numbers);    // 9
```

2.复制数组

```
const a1 = [1, 2];

// 写法1
const a2 = [...a1];

// 写法2
const [...a2] = a1;
```

3.合并数组

```
const arr1 = ['a', 'b'];
const arr2 = ['c'];

[...arr1, ...arr2];         // ['a', 'b', 'c']
```

4.与解构赋值相结合

```
// 与解构赋值相结合，生成新数组
const [first, ...rest] = [1,2,3,4,5]

first // 1,
rest  // [2,3,4,5]
```

[数组的扩展参考链接](!http://es6.ruanyifeng.com/#docs/array)