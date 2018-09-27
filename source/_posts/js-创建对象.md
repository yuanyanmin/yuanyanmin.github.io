---
title: js 创建对象
date: 2018-09-25 17:46:28
tags:
 - js
categories:
toc: true
---

**天气**： :sunny:
**心情**： :confused:

<!--more-->

### object 构造函数

```
var person = new Object();
person.name = "Nicholas";
person.age = 29;
person.job = "software engineer";

person.sayName = function() {
    alert(this.name);
}
```

### 对象字面量创建

```
var person = {
    name: "Nicholas",
    age: 29,
    job: "software engineer",
    sayName: function() {
        alert(this.name);
    }
}
```

**缺点：** 以上两种方式都可以创建单个对象，但有个明显的缺点，使用同一个接口创建很多对象，会产生大量的重复代码。为解决这个问题，开始使用工厂模式

### 工厂模式

```
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    };
    return o;
}
var person = createPerson("Nicholas", 29, "software engineer");
```

**缺点：** 工厂模式解决了创建多个相似对象的问题，但是不知道对象的类型。

### 构造函数模式

```
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
        alert(this.name);
    }
}
var person = new Person("Nicholas", 29, "software engineer");
var person2 = new Person("Tom", 22, "software engineer");
```

Person() 函数取代了 createPerson() 函数。Person() 没有显示的创建对象，直接将属性和方法赋给了 this 对象，并且没有 return 语句。

ps: 构造函数名应该以 大写字母 开头。

```
person.constructor == Person   // true
person instanceof Object       // true
person instanceof Person       // true
```

**缺点：** 每个方法都要在每个的实例上重新创建一遍。

```
person.sayName == person2.sayName // false
```

### 原型模式

每个函数都有一个 prototype 属性。这个属性是一个指针，指向一个对象。这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。按照字面意思来理解，prototype 就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处就是可以让所有对象实例共享它所包含的属性和方法。换句话说，不必在构造函数中定义对象实例的信息。而是将这些信息直接添加到原型对象中。

```
function Person() {

}
Person.prototype.name = "Nicholas"
Person.prototype.age = 29;
Person.prototype.job = "software engineer";
Person.prototype.sayName = function() {
    alert(this.name);
}
var person1 = new Person();
person1.sayName();                    // Micholas
var person2 = new Person();
person2.sayName();                   // Micholas

person1.sayName == person2.sayName;  // true
```

只要创建一个新函数，就会根据一组特定的规则为函数创建一个 prototype 属性，这个属性指向函数的原型对象。

使用 hasOwnPrototype() 方法可以检测一个属性是存在于实例中，还是原型中。

使用 in 操作符，通过对象能够访问给定属性时返回 true，无论实例或原型。

自定义函数来检测原型中是否存在给定的属性.

```
function hasPrototypeProperty(object, name){
    return !object.hasOwnProperty(name) && (name in object);
}
```

获取对象上所有可枚举的实例属性，使用 Object.keys() 方法。这个方法接收一个对象作为参数，返回一个包含所有可枚举属性额字符串属性。
获取所有实例属性，可以使用 Object.getOwnPropertyNames() 方法。

```
var key1 = object.keys(Person.prototype);
alert(key1)   // "name, age, job, sayName"

var key2 = object.getOwnPropertyNames(Person.prototype);
alert(key2)  // "constructor, name, age, job, sayName"
```

视觉上封装原型功能

```
function Person() {}
Person.prototype = {
    constructor: Person,
    name: "nicholas",
    age: 29,
    job: "software engineer",
    sayName: function(){
        alert(this.name);
    }
};
```

```
function Person(){
}
Person.prototype = {
    constructor: Person,
    name : "Nicholas",
    age : 29,
    job : "Software Engineer",
    friends : ["Shelby", "Court"],
    sayName : function () {
    alert(this.name);
    }
};
var person1 = new Person();
var person2 = new Person();
person1.friends.push("Van");
alert(person1.friends);         //"Shelby,Court,Van"
alert(person2.friends);         //"Shelby,Court,Van"
alert(person1.friends === person2.friends); //true
```

**缺点：** 在所有的实例中共享一个数组。 事实上实例一般都要有属于自己的全部属性的。

### 组合构造函数和原型模式

构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。

```
function Person(name, age, job) {
    this.name = name;
    this.age =  age;
    this.job = job;
}

Person.prototype = {
    constructor: Person,
    sayName: function() {
        alert(this.name);
    }
}

var person1 = new Person("Nicholas", 29, "software engineer");
var person2 = new Person("Greg", "27", 'Doctor");

Person1.friends.push("Van");
person1.friends            // "Shelby, Court, Van"
person2.friends            // "Shelby, Court"
person1.friends == person2.friends      // false
person1.sayName == person2.sayName      // true
```

实例属性在构造函数中定义，而所有实例共享的属性 constructor 和方法 sayName() 则是在原型中定义。

### 动态原型模式

把所有信息封装在构造函数中，通过在构造函数中初始化原型（仅在必要的条件下），换句话说，可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

```
function Person(name, age, job) {
    this.name = name;
    this.age =  age;
    this.job = job;

    // 方法
    if(typeof this.sayName != 'function') {
        Person.prototype.sayName = function() {
            alert(this.name);
        }
    }
}

var friend = new Person("nicholas", 29, "software engineer");
friend.sayName();
```

只是在 sayName() 方法不存在的情况下，才会将它添加到原型中。这段代码只会在初次调用构造函数是才会执行。

还可以使用 instanceof 操作符确定它的类型

### 寄生构造函数模式

创建一个函数。用于封装创建对象的代码，然后再返回新创建的对象。

```
function Person(name, age, job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}
var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();        //"Nicholas"
```

这个模式跟工厂模式其实是一模一样的。构造函数在不返回值的情况下，默认会返回新对象实例。通过在构造函数末尾添加一个 return 语句，可以重写构造函数时返回的值。

返回的对象与构造函数或者与构造函数的原型属性之间没有关系；也就是说，构造函数返回的对象与在构造函数外部创建的对象没有什么不同。为此，不能依赖instanceof 操作符来确定对象类型

### 稳妥构造函数模式

所谓稳妥对象，指的是没有公共属性，而且其方法也不引用this 的对象。稳妥对象最适合在一些安全的环境中（这些环境中会禁止使用this 和new），或者在防止数据被其他应用程序（如Mashup程序）改动时使用。稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用this；二是不使用new 操作符调用构造函数。

```
function Person(name, age, job){
    //创建要返回的对象
    var o = new Object();

    //可以在这里定义私有变量和函数
    //添加方法
    o.sayName = function(){
    alert(name);
    };

    //返回对象
    return o;
}

var friend = Person("Nicholas", 29, "Software Engineer");
friend.sayName();          //"Nicholas"
```

注意，在以这种模式创建的对象中，除了使用sayName()方法之外，没有其他办法访问name 的值。

