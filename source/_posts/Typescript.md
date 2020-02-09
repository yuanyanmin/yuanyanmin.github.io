---
title: Typescript
date: 2020-02-07 12:21:01
tags: ts
categories:
toc:
---

Typescirpt 是 JavaScript 的超级，遵循最新的 ES6、ES5 规范。

本文是在观看视频文档时，做的一些笔记。

[视频地址](https://www.bilibili.com/video/av38379328?p=1)

[官方文档](https://www.tslang.cn/docs/handbook/basic-types.html)


<!--more-->

### 安装 & 编译

```
npm install -g typescript

tsc helloworld.ts
```

### 在vscode中配置自动编译 .ts 文件

1. 创建 tsconfig.json 文件，tsc --init 生成配置文件
2. 点击 菜单 终端-运行任务 点击 tsc:watch tsconfig.json


### 数据类型

typescript 中为了使编写的代码更规范，更有利于维护，增加了类型校验。

即写 ts 代码必须指定类型

> 布尔类型(boolean)
> 数字类型(number)
> 数组类型(array)
> 元祖类型(tuple)
> 枚举类型(enum)
> 任意类型(any)
> null 和 undefined
> void 类型
> never 类型
> Object 类型

```
// 布尔类型
var flag:boolean = true;
flag = 123 // error
flag = false // true

// 数字类型
var num:number = 1;

// 定义数组的方式
var arr = [1, 2, 3]  // es5
var arr:number = [1, 2, 3]  
var arr:Array<number> = [1, 2, 3]
var arr:any[] = ['123', 123, true]

// 元祖类型  属于数组的一种
let arr:[number, string] = [123, 'this is ts']

// 枚举类型
enum 枚举名 {
    标识符[=整型常数],
    标识符[=整型常数],
    ...
    标识符[=整型常数]
}

enum Flag {success=1, error=2};
let s:Flag = Flag.success;
console.log(s)  // 1

// 如果标识符没有赋值，则值为 下标
enum Flag {success, error};
let s:Flag = Flag.success;
console.log(s)  // 0

// 任意类型
var num:any = 123;
num = 'str';

// null 和 undefined  其他类型的子类型
var number:number;  // 报错，输出 undefined
var num:undefined;  // 正常，输出 undefined
// 定义没有赋值就是 undefined
var num:number | undefined;

//void 类型： 一般用于定义方法的时候方法没有返回值
function run():void {
    console.log('run');
}
run()

// nerver 类型 表示的是那些永不存在的值得类型。
var a:never;
a = (() => {
    throw new Error('错误');
})

// object 类型
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

```

### 函数

```
function add(x:number, y:number):number {
    return x + y;
}
let myAdd = function(x:number, y:number):number { 
    return x + y;
}

// 可选参数
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName
    }
}

// 默认参数
function buildName(firstName: string, lastName = "Alice") {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName
    }
}

// 剩余参数
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName+ ' ' + restOfName.join(" ");
}
buildName('john', 'lucas', 'jack')

// 重载
// 上边是声明
function add (arg1: string, arg2: string): string
function add (arg1: number, arg2: number): number
// 因为我们在下边有具体函数的实现，所以这里并不需要添加 declare 关键字

// 下边是实现
function add (arg1: string | number, arg2: string | number) {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 arg1 + arg2
  if (typeof arg1 === 'string' && typeof arg2 === 'string') {
    return arg1 + arg2
  } else if (typeof arg1 === 'number' && typeof arg2 === 'number') {
    return arg1 + arg2
  }
}
```

### 类

```
class Person {
    name: string;
    constructor(n:string) {   // 构造函数，实例化类的时候出发的方法
        this.name = n;
    }
    run():void {
        alert(this.name)
    }
    getName():string {
        return this.name;
    }
    setName(name:string) {
        this.name = name;
    }
}
var p = new Person('张三');
p.run()
p.getName();
p.setName('李四');
p.getName();

// 继承  父类的方法和子类的方法一致
class Web extends Person {
    constructor(name: string) {
        super(name)
    }
    work() {
        alert(this.name)
    }
}
var w = new Web('李四')
w.run()
w.work()

// 修饰符
public、protected、private

// 静态方法
class Person {
    public name:string;
    public age:number = 20;

    // 静态属性
    static sex = '男'

    constructor(name: string) {
        this.name = name;
    }
    work() {    // 实例方法
        alert(this.name)
    }
    static print() {   // 静态方法，不能直接调用类里的属性
        alert('静态方法')
    }
}
Person.print();  // 静态方法
Person.sex;  // 男

// 多态：父类定义一个方法去实现，让继承它的子类去实现，每一个子类有不同的表现
// 多态也是继承的一种表现
class Animal {
    name: string;
    constructor(name:string) {
        this.name = name;
    }
    eat() {
        console.log('吃的方法)
    }
}
class Dog extends Animal {
    constructor (name:string) {
        super(name)
    }
    eat() { // 具体吃什么有继承它的子类去实现
        return this.name + '骨头'
    }
}
class Cat extends Animal {
    constructor (name:string) {
        super(name)
    }
    eat() {
        return this.name + '老鼠'
    }
}

// 抽象方法
// 抽象类中的方法不包含具体实现并且必须在派生类中实现
// abstract 抽象方法只能放在抽象类里面

abstract class Animal {  // 抽象类
    public name: string;
    constructor(name:string) {
        this.name = name;
    }
    abstract eat(): any;  // 抽象方法
}

class Dog extends Animal {
    // 抽象类的子类必须实现抽象类里面的抽象方法
    constructor(name:any) {
        super(name)
    }
    eat() {
        console.log(this.name + '吃粮食')
    }
}
```

### 接口
作用：在面对对象的编程中，接口是一种规范的定义，它定义行为和动作的规范，在程序设计里面，接口起到一种限制和规范的作用，接口定义了某一类所需要遵循的规范，接口不关心这些类的内部状态和数据，也不关心这些类的方法的实现细节，它只规定这批类里必须提供某方法，提供这些方法的类就可以满足实际需要。

```
//  定义传入参数

function printLable(label:string):void {
    console.log('printLabel');
}

printLable('hello')

// 对 json 的约束
function printLabel(labelInfo:(label:string)):void {
    console.log('printLabel');
}
printLabel({label: 'hello'})

// 接口 属性接口 传入对象的约束
interface fullName {
    firstName: string;  // 以分号结束
    secondName: string;
}

function printName(name: fullName) {
    console.log(name.firstName + ' ' + name.secondName)
}

// 可选属性
interface fullName {
    firstName: string;
    secondName: stirng;
    age?: number;
}

// 返回值约束
interface encrpty {
    (type:string, value:string): string;
}

var md5:encrypt = function(key:string, value:string): string {
    // 模拟操作
    return key + value;
}
md5('name', 'zhangsan')

// 可索引接口 数组、对象的约束
interface UserArr {
    [index:number]: string
}
var arr:UserArr=['1234', '5678']
arr[0] // '12345'

interface UserObj {
    [index:string]: string
}
var arr:UserObj = {name: 'zhangsan'}

// 类类型接口： 对象的约束和抽象类的约束
interface Animal {
    name: string;
    eat(str: string):void;
}

class Dog implements Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    eat() {
        console.log(this.name + '骨头')
    }
}

// 接口的扩展 接口可以继承接口

interface Animal {
    eat():void;
}
interface Person extends Animal {
    work():void;
}
class Web implement Person {
    public name:string;
    constructor(name:string) {
        this.name = name;
    }
    eat() {
        console.log(this.name + '零售')
    }
    work() {
        console.log(this.name + '代码')
    }
}
```

### 泛型

泛型，在软件工程中，不仅要创建一致的定义良好的API, 同时也要考虑到可重用性。组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型。

```
// 只能返回 string 类型的数据
function getData(value:string):string {
    return value;
}

// 同时返回 string 类型 和 number 类型
// 泛型可以支持不特定的数据类型，要求，传入的参数和返回的参数一致
// T 表示泛型，具体什么类型是由调用这个方法的时候决定的
function getData<T>(value:T):T {
    return value;
}
getData<number>(123)
getData<string>('123')

// 类的泛型

class MinClass<T> {
    public list:T[] = [];
    add (value:T):void {
        this.list.push(value)
    }
}

// 泛型接口

interface ConfigFn {
    <T>(value:T):T;
}

var getData:ConfigFn = function<T>(value:T):T {
    return value;
}

```

### 装饰器

```
// 类装饰器

// 类装饰器 (普通装饰器，没有参数)
function logClass(params:any) {
    console.log(params)

    // params 是当前类
    params.prototype.apiUrl = '动态扩展的属性'
}

@logClass
class HttpClient {
    contructor() {

    }
    getData() {

    }
}
var http:any = new HttpClient();
http.apiUrl // 动态扩展的属性

// 类装饰器 装饰器工厂（可传参）

function logClass(params:string) {
    return function(target:any) {
        console.log(target);   // 当前类
        console.log(params);   // hello
    }
}

@logClass('hello')
class HttpClient{
    constructor() {}
    getData() {}
}

// 属性装饰器

function logClass(params:string) {
    return function(target:any) {
        console.log(target);   // 当前类
        console.log(params);   // hello
    }
}

function logProperty(params: any) {
    return function(target:any, attr:any) {
        console.log(target);  // 类的原型对象
        console.log(attr);  // url （属性名称
    }
}

@logClass('hello')
class HttpClient{
    @logProperty('http://www.baidu.com)
    public url:any | undefined
    constructor() {}
    getData() {}
}

// 方法装饰器
// 传入3个参数，
// 1.对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象
// 2.成员的名字
// 3.成员的属性描述符
function logMethod(params:any) {
    return function(target:any, methodName:any, desc:any) {
        console.log(target)  // 原型对象
        console.log(methodName)  // getData
        console.log(desc)  // 描述信息 (desc.value 即是 方法)

        target.apiUrl = 'xxxx';
        target.run = function() {
            console.log('run')
        }
      
        // 修改修饰器的方法 (例如，修改为字符串)
        // 1. 保存当前的方法
        var oMethod = desc.value;
        desc.value = function(...args:any[]) {
            args = args.map((value) => {
                return String(value);
            })
            console.log(args);
            oMethod.apply(this.args);
        }
    }
}

class HttpClient{
    public url:any | undefined
    constructor() {}

    @logMethod('http://www.baidu.com)
    getData() {}
}

var http:any = new HttpClient();
http.apiUrl // xxxx
http.run()  // run

// 方法参数装饰器
functon logParams(params:any) {
    return function(target:any, methodName: any, paramIndex:any) {
        cosnole.log(params);  // uuid
        console.log(target);  // 原型对象
        console.log(methodName);  // 方法名称
        console.log(paramIndex);  // 索引位置
    }
}
class HttpClient{
    public url:any | undefined
    constructor() {}
    getData(@logParams('uuid') uuid: any) {
        console.log('getData方法')
    }
}

var http = new HttpClent();
http.getData(123)
```
