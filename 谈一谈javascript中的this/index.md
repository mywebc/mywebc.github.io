# 总结一下JavaScript中的this

> 有的时候我们总是被JavaScript中的this搞得晕头转向，因为它的不确定性，也被经常拿来当作考题，我们也经常听到网上最认同的说法：“谁调用this,this就指向谁”，那么this到底是什么呢，最近就this总结了一下。

<!-- more -->
> 本篇文章主要参考《你不知道的JavaScript》（上）

> 话说草稿老早就写了，好像忘发了。
### 为什么要使用this？
我们先要知道一个前提，在JavaScript中 **万物皆对象**，而函数在对象中又是 **一等公民**，对象与对象之间通过 **原型**联系，那对象和函数之间如何联系呢，答案就是 **this**
#### 首先如果没有this会是什么情况？
```javascript
// 只要切换上下文对象，就可以复用此函数，不用针对每个对象写一遍函数
function sayName(context){
    console.log(context.name)
}
var me = {
    name: 'kyle'
}
var you = {
    name: 'frank'
}
// 如果没有this，我们只能显示的传入对象
sayName(me)
```
```javascript
// 函数的上下文为对象的情况
var obj = {
    name: 'frank'
    sayName: function(context) {
        console.log(context.name)
    }
}
// 如果没有this，我们只能显示的传入对象
obj.sayName(obj)
```
**我们看到如果没有this，要想函数与对象产生关联，只能手动传入这个对象，那JavaScript的创始者想Java有个this，不如JavaScript也搞个this吧，干脆我就默认帮忙隐式传递这个对象得了！于是就发明了this这个关键字！**
所以结论是this关键字能够隐式的传递对象，当然了也提供了call,apply函数允许我们手动显式传递，上面的情况就可以这样写了。
```javascript
var obj = {
    name: 'frank'
    sayName: function() {
        console.log(this.name)
    }
}
// 隐式传递了obj,等同于obj.sayName.call(obj)
obj.sayName()
```

### 那关于this指向的规则
一句话总结：调用位置决定了this的绑定对象;大概有四种情况：
1. 默认绑定
2. 隐式绑定
3. 显示绑定
4. new绑定

#### 默认绑定
也就是在全局作用域中调用则指向window（严格模式下为undefined）
可以认为在其他规则无法应用的下调用的默认绑定规则。
```javascript
var a = '1'
function foo() {
    console.log(this.a) // '1'
}
function foo() {
    'use strict'
    console.log(this.a) // undefined
}
```
#### 隐式绑定
上面默认绑定的调用位置为全局，即调用位置的上下文对象为window，也有可能调用位置被其他上下文对象“包裹”，这时候js会帮你猜（隐式绑定）比如：
```javascript
var obj = {
    a: 1,
    foo: function() {
        console.log(this.a) // js猜你想用的是当前上下文中的这个a,所以就传给你。
    }
}
```
上面的上下文对象为obj,即this就会被隐式绑定到obj这个对象上
**注意**调用位置只作用于最后一次，比如如下:
```javascript
function foo() {
    console.log(this.a)
}
var obj2 = {
    a: 42,
    foo: foo
}
var obj1 = {
    a: 2,
    obj2: obj2
}
obj1.obj2.foo();
```
上面的例子中foo函数最后的调用位置为obj2中，所以最后打印出来的是42。
#### 显式绑定
显示绑定就是使用JavaScript给我们提供的call,apply,和bind函数，手动指定this,指哪儿就是哪儿.比如：
```javascript
var obj = {
    a: 2,
    foo: function() {
        console.log(this.a)
    }
}
obj.foo.call({a: 10})
obj.foo.apply({a: 10})
obj.foo.bind({a: 10})()
```
以上我们使用call和apply手动的指定了this，即a为10；
#### new绑定
首先简要概述一下new操作符干了什么
```javascript
// 第一步：创建一个对象 
var a = {}
// 第二步：既然new出来的一个实例，肯定要挂到原型上即
a.__proto__ = F.prototype // __proto__是每个实例都具有的属性
// 第三步：改变this指向
F.call(a)
// 第四步
return a
```
在第三步的时候，new就帮我们手动的改变了this的指向；
### 优先级
this应用的这四种规则当然也有优先级
new绑定 > 显示绑定 > 隐式绑定 > 默认绑定 
### 总结一下
研究JavaScript中的this本来就没有太多意义，因为现在连箭头函数都舍弃了this，这都是JavaScript开始的坑，不过大体还是需要知道滴。
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
