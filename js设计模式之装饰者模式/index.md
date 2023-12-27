# js设计模式之装饰者模式

>装饰者模式能够在不改变对象自身的基础上，在程序运行期间给对象动态地添加指责，总结来说就是两点，第一为对象添加新功能，第二不改变原有的结构和功能。

<!-- more -->
### 装饰函数
在javascript中，万物皆对象，函数则是一等对象，当我们想要为函数添加一些功能时，如果直接改写函数，就违背了开放-封闭原则，我们可以保留原有函数的引用，然后直接放到新函数内执行，比如
```javascript
let frank = function() {
    console.log("i am frank")
}
// 我们想为frank添加一个技能时，先用临时变量把原有函数存起来
let oldFrank = frank
// 放到新的frank函数执行
frank = function() {
    oldFrank()
    console.log("i am best")
}
```
此做法符合开放-封闭原则，在不改变原函数源代码的情况下为其添加新功能，但是每次添加一个新功能都必须维护一个中间变量（比如oldFrank）,长此以往，要维护的中间变量就会越来越多。
### 用AOP装饰函数
> AOP就是面向切面编程,把一些与核心业务逻辑无关的功能抽离出来
再通过“动态织入”方式掺入业务逻辑模块

在这里我们需要两个方法，一个是前置装饰，一个是后置装饰
```javascript
Function.prototype.before = function(beforeFunc){
    var that = this;
    return function(){
        beforeFunc.apply(this, arguments);
        return that.apply(this, arguments);
    }
}
Function.prototype.after = function(afterFunc){
    var that = this;
    return function(){
        var ret = that.apply(this, arguments);
        afterFunc.apply(this, arguments);
        return ret;
    }
}
```
为了避免污染原型，我们可以改写before和after方法如下
```javascript
var before = function(fn, beforefn) {
    return function() {
        beforefn.apply(this, arguments)
        return fn.apply(this, arguments)
    }
}
var after = function(fn, afterfn){
    return function(){
        var ret = fn.apply(this, arguments);
        afterfn.apply(this, arguments);
        return ret;
    }
}
```
以前置装饰为例，先执行新传入的函数，此函数就是新添加的功能，然后再执行原函数，后置装饰同理，这样我们就能在保证原函数不动的情况下，动态的为原函数添加功能。
#### AOP动态的添加函数参数
```javascript
var func = function(param) {
    console.log(param) 
}
func = func.before(function(param) {
    param.b = 'b'
})
func({a: 'a'}) // 输出{a: 'a', b: 'b'}
```

### es7装饰器
* 装饰类
```javascript
// 1. 装饰器 装饰类
@testDec
class Demo {

}
// 装饰器是一个函数
function testDec(target) {
    target.isDec = true
}
alert(Demo.isDec)

// 原理
@decorator
class A {}
// 等同于
class A {}
A = decorator(A) || A
```

* 可以传参
```javascript
// 可以传参, 需要返回一个函数
function mixins(...list) {
    return function(target) {
        Object.assign(target.prototype, ...list)
    }
}

const Foo = {
    foo() {
        alert("foo")
    }
}

@mixins(Foo)
class MyClass {

}

let obj = new MyClass()
// MyClass是没有foo函数的， 通过装饰器添加
obj.foo()
```
* 装饰方法
```javascript
class Person {
    constructor() {
        this.first = 'A'
        this.last = 'B'
    }
    // 装饰这个方法，变成可读
    @readonly
    name() {
        return `${this.first} ${this.last}`
    }
}
// name 方法名字 descriptor方法值
function readonly(target, name, descriptor) {
    descriptor.writable = false
    return descriptor
}
```
### 使用第三方库 core-decorators
```javascript
// 只读
import { readonly } from 'core-decorators'
class Person {
    @readonly
    name() {
        return '11'
    }
}
// 废弃
import { deprecate } from 'core-decorators'
class Person {
    // 如果这个方法即将废弃，可以使用这个装饰器提醒开发者
    @deprecate("即将弃用")
    facepalm(){
        ...
    }
}
```

