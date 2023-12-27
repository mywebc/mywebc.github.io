# ES6笔记（三）函数


### 函数参数的默认值

以前es5的做法是这样的： 
```ts 
function log(x) { x = x || 10; console.log(x) } 
``` 
这样做的坏处是有的时候我想传一个空字符，也会被修改为默认值 es6的做法直接在括号内写上默认值 
```ts 
function log(x=10) { console.log(x) } 
``` 
这种写法就没有上面那种问题，需要注意的是函数参数一般会放在arguments里，你用ES6这种方法的话，arguments里面是没有滴。
### reset参数
```ts 
function say(a,b,...items){ 
    // dosomething...
     } 
```
当我们不知道参数个数时，可以用展开运算符...的形式，主义他只能放在最后一个参数的位置，而且他跟arguments不同，他是真数组，意味着你可以用Push，pop等方法。
### 箭头函数

ES6 允许使用“箭头”（=>）定义函数。如下： 
```ts 
var a = (v) => v; 
``` 
两点注意： 
1. 函数参数没有或者只有一个时可以省略括号，返回值只有一个时也可以省略； （
2. 箭头函数没有自己的this，他的this指的是上下文的this,也就是说我们再也不用that = this或者self=this这样的形式了。当然箭头函数也要看场合使用，比如不要在object和原型里面定义箭头函数,因为这时候的this是指向window的。

### 尾调用优化
尾调用就是在函数的内部最后调用函数需要注意下面的都不算： 
```ts 
// 情况一 
function f(x){ let y = g(x); return y; } 
// 情况二 
function f(x){ return g(x) + 1; } 
// 情况三 
function f(x){ g(x); } 
``` 
尾调用优化的原理： 当我们调用一个函数a时，在内存中会形成一个call frame（调用帧），里面记录着a的变量信息，如果在函数a里面再调用函数B,同样会形成一个call frame位于a的调用帧上方，如此嵌套调用，多个call frame就会形成call stack(调用栈)，所谓优化就是在a的调用帧之上b的调用帧如果用不到a里面的信息的话，a的调用帧就会舍弃掉。 注意：ES6中的尾调用优化只在use strict下生效，看下面例子：

```ts 
function factorial(n, total = 1) { 
    if (n === 1) return total; 
    return factorial(n - 1, n * total); 
    } 
factorial(5) // 120 
```
