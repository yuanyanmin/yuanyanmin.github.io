---
toc: true
title: JS对象 & 继承
date: 2018-09-19 15:44:12
tags:
 - js
categories:
---

日常学习 JS 中

<!--more-->
## 对象

一个对象就是一系列属性的集合，一个属性包含一个名和一个值。一个属性的值可以是函数，这种情况下属性也被称为方法。

### 创建对象方法

+ 声明一个对象

```
var person = {
      name: ['Bob', 'Smith'],
      age: 32,
      gender: 'male',
      interests: ['music', 'skiing'],
      bio: function() {
        alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
      },
      greeting: function() {
        alert('Hi! I\'m ' + this.name[0] + '.');
      }
}
```

+ 构造函数

```
// 一个构建函数通常是大写字母开头

function Person(name) {
  this.name = name;
  this.greeting = function() {
    alert('Hi! I\'m ' + this.name + '.');
  };
}
```

+ Object() 构造方法

```
var person1 = new Object();

person1.name = 'Chris';
person1['age'] = 38;
person1.greeting = function() {
  alert('Hi! I\'m ' + this.name + '.');
}

// 将对象文本传递给Object() 构造函数作为参数

var person1 = new Object({
  name : 'Chris',
  age : 38,
  greeting : function() {
    alert('Hi! I\'m ' + this.name + '.');
  }
});
```

+ create() 方法

```
// JavaScript有个内嵌的方法create(), 它允许您基于现有对象创建新的对象实例。
// 实际做的是从指定原型对象创建一个新的对象

var person2 = Object.create(person1);
```

### 原型链

JavaScript 常被描述为一种基于原型的语言 (prototype-based language)——每个对象拥有一个原型对象，对象以其原型为模板、从原型继承方法和属性。原型对象也可能拥有原型，并从中继承方法和属性，一层一层、以此类推。这种关系常被称为原型链 (prototype chain)，它解释了为何一个对象会拥有定义在其他对象中的属性和方法。

准确地说，这些属性和方法定义在Object的构造器函数(constructor functions)之上的prototype属性上，而非对象实例本身。

```
function doSomething() {}
console.log(doSomething.prototype);

```

返回结果：

![](/photo/js对象/result_1.png)

#### prototype 属性

继承成员被定义的地方

doSomeInstancing 的 __proto__ 就是 doSomething.prototype.当访问 doSomeInstancing 的属性时，浏览器首先查找 doSomeInstancing 是否有这个属性，若没有，则从 doSomeInstancing 的 __proto__ 中查找（doSomething.prototype）若有，则会被使用。否则从 doSomeInstancing 的 __proto__ 的 __proto__ 查找。以此类推，直到 Object.prototype === undefined.


#### constructor 属性

每个实例对象都从原型中继承了一个 constructor 属性，该属性指向了用于构造此实例对象的构造函数

```
function Person(name) {
  this.name = name;
  this.greeting = function() {
    alert('Hi! I\'m ' + this.name + '.');
  };
}

var person1 = new Person("andy")

console.log(person1.constructor);
```

返回结果：

![](/photo/js对象/result_2.png)

#### 修改原型

一种极其常见的对象定义模式是，在构造器（函数体）中定义属性、在 prototype 属性上定义方法。如此，构造器只包含属性定义，而方法则分装在不同的代码块，代码更具可读性。

```
// 构造器及其属性定义

function Test(a,b,c,d) {
  // 属性定义
};

// 定义第一个方法

Test.prototype.x = function () { ... }

// 定义第二个方法

Test.prototype.y = function () { ... }

```

## 继承

JavaScript 只有一种结构：对象。每个实例对象（object ）都有一个私有属性（称之为 __proto__）指向它的原型对象（prototype）。该原型对象也有一个自己的原型对象 ，层层向上直到一个对象的原型对象为 null。根据定义，null 没有原型，并作为这个原型链中的最后一个环节。

#### 原型式的继承

```
function Person(first, last, age, gender, interests) {
  this.name = {
    first,
    last
  };
  this.age = age;
  this.gender = gender;
  this.interests = interests;
};

// 所有的方法都定义在构造器的原型上

Person.prototype.greeting = function() {
  alert('Hi! I\'m ' + this.name.first + '.');
};

// 定义 teacher() 构造器函数

function Teacher(first, last, age, gender, interests, subject) {
  Person.call(this, first, last, age, gender, interests);

  this.subject = subject;
}

// 设置 teacher() 的原型和构造器引用

Teacher.prototype = Object.create(Person.prototype);
Teacher.prototype.constructor = Teacher;

// 向 teacher() 添加一个新的 greeting() 函数

Teacher.prototype.greeting = function(){
    // 代码
}
```

每一个函数对象(Function)都有一个 prototype 属性,并且只有函数对象有 prototype 属性

Person() 函数是 Person.prototype 的构造函数，
即：Person === Person.prototype.constructor.

任何想要继承的方法都应该定义在构造函数的 prototype 对象中。
并且使用父类的 prototype 来创造子类的 prototype。

## 总结

1. 那些定义在构造器函数中的、用于给予对象实例的。这些都很容易发现 - 在您自己的代码中，它们是构造函数中使用this.x = x类型的行；在内置的浏览器代码中，它们是可用于对象实例的成员（通常通过使用new关键字调用构造函数来创建，例如var myInstance = new myConstructor()）。
2. 那些直接在构造函数上定义、仅在构造函数上可用的。这些通常仅在内置的浏览器对象中可用，并通过被直接链接到构造函数而不是实例来识别。 例如Object.keys()。
3. 那些在构造函数原型上定义、由所有实例和对象类继承的。这些包括在构造函数的原型属性上定义的任何成员，如myConstructor.prototype.x()。
