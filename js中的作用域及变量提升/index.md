# js中的作用域及变量提升


**一、作用域** 
###  作用域有：块级作用域和词法作用域； 
1. 块级作用域：使用{}包裹起来的，里面的变量外面是不可以访问的，js没有块级作用域，js里外面是可以访问{}里的变量的; 
2. 词法作用域：词法( 代码 )作用域, 就是代码在编写过程中体现出来的作用范围. 
代码一旦写好, 不用执行, 作用范围就已经确定好了. 这个就是所谓词法作用域.js中有词法作用域。
   
在 js 中词法作用域规则: 
1. 函数允许访问函数外的数据. 
2. 整个代码结构中只有函数可以限定作用域. 
3. 作用域规则首先使用提升规则分析 
4. 如果当前作用规则中有名字了, 就不考虑外面的名字 
### 作用域链 
概念：只有函数可以制造作用域结构， 那么只要是代码，就至少有一个作用域, 即全局作用域。

凡是代码中有函数，那么这个函数就构成另一个作用域。如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域。 将这样的所有的作用域列出来，可以有一个结构: 函数内指向函数外的链式结构。就称作作用域链。 如下代码:
```javascript
function f1() {
    function f2() {
    }
}

var num = 456;
function f3() {
    function f4() {    
    }
}
```
它的作用域链图就是： 绘制作用域链的步骤: 
* 看整个全局是一条链, 即顶级链, 记为 0 级链 
* 看全局作用域中, 有什么成员声明, 就以方格的形式绘制到 0 级练上 
* 再找函数, 只有函数可以限制作用域, 因此从函数中引入新链, 标记为 1 级链 
* 然后在每一个 1 级链中再次往复刚才的行为 变量的访问规则 
* 首先看变量在第几条链上, 在该链上看是否有变量的定义与赋值, 如果有直接使用 
* 如果没有到上一级链上找( n - 1 级链 ), 如果有直接用, 停止继续查找. 
* 如果还没有再次往上刚找... 直到全局链( 0 级 ), 还没有就是 is not defined 
* 注意,同级的链不可混合查找 二、变量提升 1.js代码的执行分为两个步骤： 
* 预解析 JavaScript代码在预解析阶段，会对以var声明的变量名，和function开头的语句块，进行提升操作 
* 然后再执行 第一个阶段就是变量提升的过程，解释器会先找到var已经声明的变量和function开头的语句，并把他们提升到代码开头，然后再执行。 例子1：
```js
func();
function func(){
     alert("Funciton has been called");
}

// 提升后：

function func(){
     alert("Funciton has been called");
}
func();
```
例子2：
```js
alert(a);
var a = 1;

提升后：

var a; //这里是声明
alert(a);//变量声明之后并未有初始化和赋值操作，所以这里是 undefined
a = 1;
```
2.复杂点的情况 （1）函数同名：后面的会覆盖前面的 比如：
```js
func1();
function func1(){
     console.log('This is func1');
}

func1();
function func1(){
     console.log('This is last func1');
}

// 提升后：

function func1(){
     console.log('This is func1');
}

function func1(){
     console.log('This is last func1');
}

func1();
func1();
```
提升后，因为后面会覆盖前面的，所以结果都为：This is last func1。 (2)变量和函数名同名 当出现变量声明和函数同名的时候，只会对函数声明进行提升，变量会被忽略。 比如：
```js
alert(foo);
function foo(){}
var foo = 2;

// 提升后：

function foo(){};
alert(foo);
foo = 2;
```
（3）声明提升并不是将所有的声明都提升到window对象下面，提升原则是提升到变量运行的环境(作用域)中去。 比如：
```js
function showMsg()
{
    var msg = 'This is message';
}
alert(msg); // msg未定义

// 提升后：

function showMsg()
{
    var msg;
    msg = 'This is message';
}
alert(msg); // msg未定义
```
（4）提升是分段的，其实就分script标签的。两个script标签之间是分开的。 （5）函数表达式不提升 比如：
```js
function showMsg()
func();
var func = function(){
    alert("我被提升了");
};
```
这里会报错：func is not a function；
