# TypeScript入门

> TypeScript主要提供了类型系统和对ES6的支持,对于一个需要长期维护的项目，使用TypeScript可以减少维护成本。使用VSCode编辑器，默认支持TypeScript，其次需要下载npm install -g typescript，因为我们需要tsc命令来将ts文件编译为js。

<!-- more -->
> 参考教程[https://github.com/xcatliu/typescript-tutorial/blob/master/README.md](https://github.com/xcatliu/typescript-tutorial/blob/master/README.md)
## 基本数据类型
布尔值、数值、字符串、null、undefined Symbol 
**Symbol是ES6中的基本数据类型，返回的值为唯一**
## 类型标注
我们声明变量可以如下声明
```javascript
let a: boolean = false
let b: string = '11'
let c: number = 11
let d: null = null
let e: undefined = undefined
```
当然还有 **void** ,如果一个变量声明为void，那么你只能将它赋值给null和undefined
```javascript
let a: void = null
let a: void = undefined
```
另外undefined 和 null 是所有类型的 **子类型** ，所以你可以将它们赋值给所有类型,下面都不会报错的。
```javascript
let a: number = null
let a: string = undefined
let a: boolean = null
```
使用 **any** 指定任意类型
```javascript
let a: any = '1'
// 因为是任意类型，可以随意赋值，一下不会报错
a = 11
a = false
```
如果声明变量时(不赋值)没有指定类型即为默认any
```javascript
let a
a = '1'
a= 1
```
## 类型推论
类型推断顾名思义typescript会根据你在声明变量时的赋值，来推断变量的类型
```JavaScript
let a = '1'
a = 1 //会报错，因为ts已经推断变量a为字符类型
```
## 联合类型
联合类型表示取值可以为多种类型中的一种。使用 | 
```javascript
let a: number | string
a = 1
a = '1'
```
以上表示a可以是number或者string
## 类型别名
类型别名就是给类型取一个新的名字，使用type定义
```JavaScript
type a = string
type b = number
let c: a | b
```
## 内置对象
ES里的内置对象
Boolean、Error、Date、RegExp 等，我们可以在ts中将变量定义为这些类型
```javascript
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```
DOM 和 BOM 的内置对象
Document、HTMLElement、Event、NodeList 等。
```javascript
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```
## 对象类型-接口
接口是对行为的抽象，一般用类去实现，我们也可以用来定义对象；
对于对象来说，我们用接口来定义基本格式，那么对象必须满足这个格式，比如：
```javascript
// typescript推荐所有接口以I开头
interface Ia {
    name: string;
    age: number;
}
// 定义对象，使用Ia格式
let a: Ia = {
    name: 'a',
    age: 1
}
// 如果类型不符合会报错,比如
let b: Ia = {
    name: 11,
    age: '1'
}
```
### 可选属性
有时我们希望不要完全匹配一个格式，那么可以用可选属性：
```JavaScript
interface Ia {
    name: string;
    age?: number;
}
```
### 任意属性
使用 [propName: string] 定义了任意属性取 string 类型的值。
```JavaScript
interface Ia {
    name: string;
    age?: number;
    [propName: string]: any;
}
```
需要注意的是，**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的子类型**,也就是说name和age的类型必须这个任意属于类型的子类型，这么定义的any满足情况。
### 只读属性
我们只在刚刚创建的时候赋值，再修改就不可以了。
```JavaScript
interface Ib {
    readonly x: number;
}
let b: Ib = { x: 1};
b.x = 5; // error!
```
## 数组类型
### 类型+[]
最简单的方法是使用「类型 + 方括号」来表示数组：
```javascript
// 声明一个number数组，如果有其他类型则会报错
let a: number[] = [1,2,3,4]
```
### 数组泛型
泛型之后会讲
```javascript
let a: Array<number> = [1,2,3,4]
```
### 用接口表示数组
我们之前说过，接口就是抽象+格式,既然可以定义对象，数组同样也可以
```javascript
interface INumberArray {
    [a: number]: number;
}
let a: INumberArray = [1，1，1，1]
```
### 使用any
用any表示数组可以出现任意类型
```javascript
let a: any[] = [1,'2',{a: '11'}]
```
### 类数组
我想就是伪数组，比如arguments
```javascript
function abc () {
    // args 是伪数组
    let args: IArguments = arguments
    console.log(args)
}
```
IArguments是typescript内部已经定义好的，除此还有NodeList, HTMLCollection 等
### 元组
元组较为数组更加严格，你可以规定元组第一个必须为string，第二个必须为number，如果不是则报错,比如
```JavaScript
let a: [string, number] = ['1', 1]
```
顺序是必须匹配的，当然你也可以只赋值一项
```javascript
let a: [string, number]
a[0] = '12'
```
如果你越界赋值，则赋值类型就是已知的联合类型
```javascript
let a: [string, number] = ['1', 1]
a.push('11') // correct
a.push(1) // correct
a.push(true) // error true不是字符也不是数值
```
## 函数类型
函数的输入和输出都要约束到，函数定义有两种方式，函数声明和函数表达式
### 函数声明
```javascript
// 函数声明
function a(x: number, y: number): number {
    return x + y
}
```
### 函数表达式
```javascript
// 函数表达式
let a = function (x: number, y: number) {
    return x + y
}
```
上面的函数表达式的定义其实是不对的，虽然也能编译通过，因为它只是对右边进行了类型定义，左边只是通过赋值，typescript通过类型推论推断出来的，因此左边我们也要手动赋值
```javascript
let a: (x: number, y: number) => number = function (x: number, y: number) {
    return x + y
}
```
=>不是将头函数，在typescript中表示函数的返回值
### 接口形式定义函数格式
```javascript
interface Ia {
    (a: number, b: number): number;
}
let d: Ia;
d = function(a: number, b: number): number {
    return a + b
}
```
### 可选参数
同样使用?,可选参数必须在必须参数后面
```javascript
function a(x: number, y?: number): number {
    if(y) {
        return x + y
    } else {
        return x
    }
}
```
### 参数默认值
y默认为1
```javascript
function a(x: number, y: number = 1): number {
        return x + y
}
```
### 剩余参数
使用...rest，获取函数中剩余的参数, items是一个数组，我们定义为任何类型
```javascript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```
### 重载
重载就是函数名同名，我们根据参数类型不同来达到精确匹配。ts里面我们需要手动定义，最精确的写在前面。
```javascript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```
## 类
除了ES6中类一样外，TypeScript 还可以使用三种访问修饰符，分别是 public、private 和 protected。
* public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的。
* private 修饰的属性或方法是私有的，不能在声明它的类的外部访问。
* protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的。
比如下面，name是public，所以可以直接访问实例，如果不想被访问可以用private
```javascript
class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
console.log(a.name); // Tom
```
### 抽象类
抽象类不允许被实例化，我们必须用子类继承它，并且如果抽象类里面定义了抽象方法，必须由子类实现
```javascript
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
    public abstract sayHi();
}

class Cat extends Animal {
    public sayHi() {
        console.log(`Meow, My name is ${this.name}`);
    }
}

let cat = new Cat('Tom');
```

## 声明文件
如果我们想使用第三方库比如jquery怎么办，ts并不认识$或者Jqyery,所以我们需要声明，这里我们使用declare关键字来定义，我们一般会把声明文件单独放到一个文件中
```javascript
// jQuery.d.ts
declare var jQuery: (string) => any;
```
然后在使用到的文件的开头，用「三斜线指令」表示引用了声明文件：
```javascript
/// <reference path="./jQuery.d.ts" />
jQuery('#foo');
```
现在已经有很多的库不需要我们自己写声明文件了，我们下载时会自带声明文件，只要加上@types前缀就好，比如jquery
```JavaScript
npm install @types/jquery --save-dev
```
## 声明合并
如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型,当然类型必须一致
以接口为例
```javascript
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```
相当于
```javascript
interface Alarm {
    price: number;
    weight: number;
}
```
## 枚举
枚举使用enum关键字来定义，比如
```javascript
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```
枚举的值可以是常数也可以是计算的值，没有初始化会被默认赋值为0，下面的枚举成员的值会一次+1，如果赋值了的话，会根据赋值的值一次+1
### 常数枚举
常数枚举是使用 const enum 定义的枚举类型：
```javascript
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

### 外部枚举
外部枚举是使用 declare enum 定义的枚举类型：
```javascript
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```
## 泛型
泛型就是类型的变量，如果我们想输入什么类型，就要输出什么类型，就可以使用泛型，可能你会想到用any,但是这样其实失去了内部检查。
```javascript
function identity(arg: any): any {
    return arg;
}
```
这时候我们可以用泛型，<T>
```javascript
function identity<T>(arg: T): T {
    return arg;
}
```
我们输入的类型，那么T就是什么类型，输出就是这个类型。
### 约束泛型
```javascript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```
上例中，我们使用了 extends 约束了泛型 T 必须符合接口 Lengthwise 的形状，也就是必须包含 length 属性。


