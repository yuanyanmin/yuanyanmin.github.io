---
toc: true
title: js-每日一题
date: 2018-09-16 13:18:11
tags:
 - js每日一题
categories:
---

推荐学习网站 [FreeCodeCamp](https://www.freecodecamp.cn)
 
### 题目一 

[Symmetric Difference](https://www.freecodecamp.cn/challenges/symmetric-difference)

**题目**

创建一个函数，接受两个或多个数组，返回所给数组的 对等差分(symmetric difference) (△ or ⊕)数组.

给出两个集合 (如集合 A = {1, 2, 3} 和集合 B = {2, 3, 4}), 而数学术语 "对等差分" 的集合就是指由所有只在两个集合其中之一的元素组成的集合(A △ B = C = {1, 4}). 对于传入的额外集合 (如 D = {2, 3}), 你应该安装前面原则求前两个集合的结果与新集合的对等差分集合 (C △ D = {1, 4} △ {2, 3} = {1, 2, 3, 4}).

<!---more-->

**题意**

把参数中的前两项做运算，得到一个所有项都没有同时出现在两个数组中的新数组，用此数组继续对后面的数组进行同样操作。

**思路&代码**

因为参数个数未确定，所以利用 arguments。

+ 把argument 对象的集合放进一个新数组

```
    // for 循环
    var arr = [];
    for(var i = 0; i < arguments.length; i++){
      arr.push(arguments[i]);
    }

    // Array.from() 方法
    var arr = Array.from(arguments);
```

+ Array.reduce() 方法。

```
    /* 传入的函数带有4个参数：
     * 前一个值 prev 、当前值 cur 、
     * 项的索引 index 、数组对象 array 
     */

    var temp = arr.reduce(function(prev,cur,index,array){
        var a = prev.filter(function(item){
          return cur.indexOf(item) < 0;
        });
        var b = cur.filter(function(item){
          return prev.indexOf(item) < 0;
        });
        return a.concat(b);
    })

```

+ 去重

```
    return temp.filter(function(item,index,array){
        return array.indexOf(item) == index;
    });
```
