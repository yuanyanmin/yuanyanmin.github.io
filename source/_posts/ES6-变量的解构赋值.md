---
title: ES6-变量的解构赋值
date: 2021-11-10 15:51:39
tags: ES6
categories: ES6
toc: true
---

开始复习神奇的解构赋值

<!--more-->

#### 定义
> ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为建构赋值

#### 数组的解构赋值

* 1 按照对应位置，对变量赋值 本质上称为 “模式匹配”，只要等号两端模式相同，左边的变量就会被赋予对应的值

```
let [a, b, c] = [1, 2, 3]
a // 1
b // 2
c // 3

let [foo, [[bar], baz] = [1, [[2], 3]]
foo // 1
bar // 2
baz // 3

let [, , third] = [1, 2, 3]
third // 3

let [head, ...tail] = [1, 2, 3, 4]
head // 1
tail // [2,3,4]

```
* 2 如果解构不成功则为 undefined

```
let [x, y, ...z] = ['a']
x // a
y // undefined
z // []

// 不成功
let [foo] = []
let [bar, foo] = [1]

foo // undefined
```

* 3 不完全解构 即等号左边的模式只匹配一部分等号右边的数组，仍可以成功
```
let [x, y] = [1, 2, 3, 4]
x // 1
y // 2
```

* 4 指定默认值(只有当一个数组成员炎等等于undefined，默认值才会生效)

```
let [x, y = 'b'] = ['a']
x // a
y // b

let [x] = [null]
x // null
```

* 5 扩展运算符参数必须放在最后一位

```
let [a, ...b, c] = [1, 2, 3, 4]

// error:Uncaught SyntaxError: Rest element must be last element
```

#### 对象的解构赋值

> 与数组有个不同，数组是按次序排列的，变量值由位置决定，而对象，变量必须与属性同名，才能取到正确的值

```
let {foo, bar} = {foo: 'aaa', bar: 'bbb'}
foo // aaa
bar // bbb
```

* 1 如果解构失败 返回 undefined

```
let {foo} = {bar: 'baz'}
foo // undefined
```

* 2 如果变量名 不一致

```
let {foo:baz} = {foo: 'aaa', bar: 'bbb'}
baz // aaa
foo // error: foo is not undefined
```
**对象的解构赋值内部机制：先找到同名属性，然后再赋值给对应的变量，真正被赋值的是后者，而不是前者**

* 3 解构也可以用于嵌套结果的对象

```
let obj = {
  p: [
    'hello',
    {y: 'world'}
  ]
}
let { p:[x, {y}] } = obj
x // hello
y // world
p // ["hello", {y:'world'}]
```

* 4 默认值(默认值生效的条件是，对象的属性值严格等于 undefined)

```
var {x = 3} = {}
x // 3
```

#### 字符串的解构赋值

```
const [a, b, c, d, e] = 'hello'
a // h
b // e
c // l
d // l
e // o
```

* 1 类似数组的对象都有一个 length ，因此还可以对这个属性解构赋值

```
let {length: len} = 'hello'

len // 5
```

#### 数值和布尔值的解构赋值

如果等号右边是数值和布尔值，则会先转为对象

```
let {toString: s} = 123
s === Number.prototype.toString // true

let {toString: s} = true
s === Boolean.prototype.toString // true
```

#### 函数参数的解构赋值

```
function add([x, y]) {
  return x + y
}
add([1, 2])  // 3
```

* 1 默认值

```
function move({x = 0, y = 0} = {} {
  return [x, y]
})
move({x: 3, y: 8}) // [3, 8]
move({x: 3}) // [3, 0]
move({}) // [0, 0]
move() // [0, 0]

// 为参数指定默认值，而不是为变量 x 和 y 指定
function move({x, y} = {x: 0, y: 0} {
  return [x, y]
})
move({x: 3, y: 8}) // [3, 8]
move({x: 3}) // [3, undefined]
move({}) // [undefined, undefined]
move() // [0, 0]
```

#### 用途

* 1 交换变量的值

```
let x = 1
let y = 2
[x, y] = [y, x]
```

* 2 从函数返回多个值

```
function example() {
  return [1, 2, 3]
}

let [a, b, c] = example()

function example() {
  return {
    foo: 1,
    bar: 2
  }
}
let {foo, bar} = example()
```

* 3 函数参数的定义

```
function f([x, y, z]) {...}
f([1, 2, 3])

function f({x, y, z}) {...}
f({x:1, y:2, z:3})
```

* 4 提取JSON

```
let jsonData = {
  id: 42,
  status: 'ok',
  data: [869]
}
let {id, status, data:number} = jsonData
id // 42
status // ok
number // [869]
```

* 5 函数参数的默认值

```
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```

* 6 遍历map结构

```
const map = new Map()
map.set('first', 'hello')
map.set('second', 'world')

for (let [key, value] of map) {
  console.log(key + " is " + value)
}

```

* 7 输入模块的指定方法

```
const {SourceNode} = require("source-map")
```




