---
title: Array
date: 2018-09-28 14:14:17
tags:
 - js
categories:
toc: true
---

**天气**： :partly_sunny:
**心情**： :frowning:

ECMAScript 数组的每一项可以保存任何类型的数据，大小也可以动态调整。

<!--more-->

### 创建数组的基本方式

+ 使用 Array 构造函数

```
var colors = new Array();

var colors = new Array(20);

var colors = new Array("red", "blue", "green");
```

可以省略 new 操作符

+ 使用数组字面量表示法

```
var colors = ['red', 'blue', 'green'];
var names = [];
```

### Array 基本类型

#### 检测数组

+ instanceof 操作符

```
if(value instanceof Array) {

}
```

+ Array.isArray() 方法

```
if(Array.isArray(value)) {

}
```

#### 转换方法

所有对象都具有 toLocaleString()、toString() 、valueOf() 方法。

调用 toString() 返回有数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。而调用 valueOf() 返回的还是数组。

#### 栈方法

+ push() 接收任意数量的参数，逐个**添加到数组末尾**，并返回**修改后数组的长度**，**原数组改变**。

+ pop() 从数组**末尾中移除最后一项**，减少数组的 length 值，返回**移除的项**，**原数组改变**。

```
var colors = new Array(); // 创建一个数组

var count = colors.push("red", "green"); // 推入两项
alert(count);              // 2
alert(colors);             // ["red", "green"]

var item = colors.pop(); // 取得最后一项

alert(item);                // "green"
alert(colors.length);       // 1
alert(colors);              // ["red"]
```

#### 队列方法

+ shift() **移除数组中第一项**，并返回**该项**，同时将数组长度减 1。**原数组改变**。

+ unshift() 在数组**前端添加任意个项**，并返回**数组的长度**，**原数组改变**。

```
var colors = new Array();            // 创建一个数组
var count = colors.unshift("red", "green");  // 推入两项
alert(count);            // 2
alert(colors);           // ["red", "green"]

var item = colors.shift(); // 取得最后一项
alert(item);               // "red"
alert(colors.length);      // 1
alert(colors);             // ["green"]
```

#### 重排序方法

+ reverse() 反转数组项的顺序，返回**反转后的数组**，**原数组中改变**。

+ sort() 默认情况下按**升序**排列数组项，**原数组中改变**。

```
var values = [1, 2, 3, 4, 5];
values.reverse();
alert(values); // 5,4,3,2,1

var values = [0, 1, 5, 10, 15];
values.sort();
alert(values); // 0,1,10,15,5
```

sort() 方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面，比较函数接收两个参数，若第一个参数应该位于第二个之前则返回一个负数，若相等则返回0，若之后则返回一个正数。

```
function compare(value1, value2) {
    if (value1 < value2) {
        return -1;
    } else if (value1 > value2) {
        return 1;
    } else {
        return 0;
    }
}

// 方式二
function compare(value1, value2) {
    return value2 - value1;
}

var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values);             // 0, 1, 5, 10, 15;
```

#### 操作方法

+ concat() 方法可以基于当前数组中所有项创建一个新数组。创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后**返回新构建的数组**，**不会修改原数组**

```
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);

alert(colors); //red,green,blue
alert(colors2); //red,green,blue,yellow,black,brown
```

+ slice()    基于当前数组中的一个或多个**创建一个新数组**。slice() 方法接收一或者两个参数，即要**返回项的起始和结束位置**。在只有一个参数的情况下，slice()方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该法返回起始和结束位置之间的项—-但不包括结束位置的项。注意，slice()方法**不会影响原始数组**。

```
var colors = ["red", "green", "blue", "yellow", "purple"];

var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);

alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow
```

+ splice()  向数组的中部**插入项**

> 删除：可以删除任意数量的项，只需指定 2 参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项

> 插入： 可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,"red","green")会从当前数组的位置 2 开始插入字符串"red"和"green"。

> 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如， splice (2,1,"red","green")会删除当前数组位置 2 的项，然后再从位置2 开始插入字符串"red"和"green"。

始终返回**一个数组**，该数组包含**从原始数组中删除的项**，若没有删除任何项，则返回一个空数组。

```
var colors = ["red", "green", "blue"];

var removed = colors.splice(0,1); // 删除第一项

alert(colors);      // green,blue
alert(removed);     // red，返回的数组中只包含一项

removed = colors.splice(1, 0, "yellow", "orange");   // 从位置1 开始插入两项

alert(colors);             // green,yellow,orange,blue
alert(removed);            // 返回的是一个空数组

removed = colors.splice(1, 1, "red", "purple");     // 插入两项，删除一项

alert(colors);            // green,red,purple,orange,blue
alert(removed);           // yellow，返回的数组中只包含一项
```

#### 位置方法

indeOf() 从数组的开头向后查找，返回**要查找的项在数组中的位置**
lastIndexOf() 从数组的末尾向前查找，返回**要查找的项在数组中的位置**
可接受两个参数，**要查找的项**和**查找起点位置的索引**（可选）

```
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];

numbers.indexOf(4);             // 3
numbers.lastIndexOf(4);         // 5

numbers.indexOf(4, 4)           // 5
numbers.lastIndexOf(4, 4)       // 3
```

#### 迭代方法

接收三个参数： 数组项的值、该项在数组中的位置和数组对象本身。

+ every(): 对数组中的每一项运行给定函数，若该函数对每一项都返回 true,则返回 true.

+ filter(): 对数组中的每一项运行给定函数，会返回 true 的项组成的数组

+ forEach(): 对数组中的每一项运行给定函数，没有返回值

+ map(): 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组

+ some(): 对数组中的每一项运行给定函数，若该函数对任意一项返回 true,则返回 true

```
var numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];

// every
var everyResult = numbers.every(function(item, index, array){
    return (item > 2);
});
everyResult             // false

//some
var someResult = numbers.some(function(item, index, array){
    return (item > 2);
});
someResult             // true

// filter
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
filterResult;          // [3,4,5,4,3]

// map
var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});
mapResult) ;           //[2,4,6,8,10,8,6,4,2]

// forEach
numbers.forEach(function(item, index, array){
    //执行某些操作
});
```

#### 归并方法

迭代数组的所有项，然后构建一个最终返回得值。

+ reduce()  从数组的第一项开始，逐个遍历到最后

+ reduceRight()  从数组的最后一项开始，向前遍历到第一项

都接受两个参数：一个是**在每一项上调用的函数**和**作为归并基础的初始值**(可选)。传给 reduce() 和 reduceRight() 的函数接收 4 个参数： **前一个值、当前值、项的索引和数组对象**。。这函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

```
// 使用reduce()方法可以执行求数组中所有值之和的操作
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
alert(sum); //15
```
