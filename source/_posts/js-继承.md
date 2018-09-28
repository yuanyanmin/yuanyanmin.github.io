---
title: js-继承
date: 2018-09-28 10:16:59
tags:
   - js
categories:
toc: true
---

**天气**： :partly_sunny:
**心情**： :frowning:

许多 OO 语言都支持两种继承方式：接口继承和实现继承。ECMAScript 只支持实现继承，实现继承主要是依靠原型链来实现的

<!--more-->

### 原型链

利用原型让一个引用类型继承另一个类型的属性和方法。每个构造函数都有一个原型对象，原型对象都包含一个指向函数的指针，而实例都包含一个指向原型对象的内部指针。

```
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function () {
    return this.property;
}

function SubType() {
    this.subproperty = false;
}

//继承了 SuperType
SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function() {
    return this.subproperty;
}

var instance= new SubType();
instance.getSuperValue();    // true;
```

#### 原型链图

![](/photo/js原型链/1.png)

实际上，不是SubType 的原型的constructor 属性被重写了，而是SubType 的原型指向了另一个对象——SuperType 的原型，而这个原型对象的constructor 属性指向的是SuperType。

#### 确定原型和实例的关系

+ instanceof 操作符

```
instanace instanceof Object      // true
instanace instanceof SuperType   // true
instanace instanceof SubType     // true
```

+ isPrototypeOf() 方法

```
Object.prototype.isPrototype(instance);       // true
SuperType.prototype.isPrototype(instance);    // true
SubType.prototype.isPrototype(instance);      // true

```

+ 谨慎定义方法

给原型添加方法的代码一定要放在替换原型的语句之后

```
//继承了SuperType
SubType.prototype = new SuperType();

//添加新方法
SubType.prototype.getSubValue = function (){
    return this.subproperty;
};

//重写超类型中的方法
SubType.prototype.getSuperValue = function (){
    return false;
};
var instance = new SubType();
alert(instance.getSuperValue());     //false
```

再通过原型链实现继承时，不能使用对象字面量创建原型方法，因为这样会重写原型链

### 借用构造函数（经典继承）

在子类型构造函数的内部调用超类型构造函数。通过 call() 和 apply() 方法可以在新创建的对象上执行构造函数

```
function SuperType(){
    this.colors = ["red", "blue", "green"];
}

function SubType(){
    //继承了SuperType
    SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors);            //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors);            //"red,blue,green"
```

#### 传递参数

可以在子类型构造函数中向超类型构造函数传递参数。

```
function SuperType(name){
    this.name = name;
}
function SubType(){
    //继承了SuperType，同时还传递了参数
    SuperType.call(this, "Nicholas");

    //实例属性
    this.age = 29;
}
var instance = new SubType();
alert(instance.name);                 //"Nicholas";
alert(instance.age);                  //29
```

#### 存在的问题

方法都在构造函数中定义，因此函数复用就无从谈起了。

### 组合继承

有时候也称伪经典继承，指的是将原型链和借用构造函数的技术组合到一块。使用原型链实现对原型属性和方法的继承，而通过借用构造函数实现对实例属性的继承。

```
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
    alert(this.name);
}

function SubType(name, age) {
   //继承属性
   SuperType.call(this, name);
   this.age = age;
}

//继承方法
SubType.prototype = new SuperType();

SubType.prototype.constructor = SubType;

SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);

instance1.colors.push("black");
alert(instance1.colors);             //"red,blue,green,black"
instance1.sayName();                 //"Nicholas";
instance1.sayAge();                  //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors);            //"red,blue,green"
instance2.sayName();                //"Greg";
instance2.sayAge();                 //27
```

**最常用的继承模式**

### 原型式继承

```
function object(o){
    function F(){}
    F.prototype = o;
    return new F();
}

var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
}

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");
alert(person.friends);               //"Shelby,Court,Van,Rob,Barbie"
```

### 寄生式继承

创建一个仅用于封装继承过程的函数。

```
function createAnother(original) {
    var clone = object(original);   // 通过调用函数创建一个对象
    clone.sayHi = function() {      // 以某种方式增强这个对象
        alert('hi');
    };
    return clone;                  // 返回这个对象
}

var person = {
    name: "Nicholas",
    friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = createAnother(person);
anotherPerson.sayHi();                        //"hi"
```

### 寄生组合式继承

通过借用够着函数来继承属性，通过原型链的混成形式来继承方法。

```
function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype); //创建对象
    prototype.constructor = subType; //增强对象
    subType.prototype = prototype; //指定对象
}

function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
    alert(this.name);
};
function SubType(name, age){
    SuperType.call(this, name);
    this.age = age;
}
inheritPrototype(SubType, SuperType);

SubType.prototype.sayAge = function(){
    alert(this.age);
};
```

![](/photo/js原型链/2.png)

**被称为最理想的继承范式**
