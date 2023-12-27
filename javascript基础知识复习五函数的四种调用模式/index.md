# JavaScript基础知识复习（五）函数的四种调用模式


**一、函数模式** 
这个我们再熟悉不过了。
```JavaScript
function foo(){
    console.log(this);
}
foo()；//这里函数名调用，就是函数模式
```
注意：这里的this指的是window全局对象；
 **二、方法模式** 
 函数放在对象内，是对象的一个属性，我们调用函数，这就是方法模式。
```javascript
var obj={
  foo:function(){
      console.log(this);
  }
}
obj.foo();
```
<!--more-->
注意：这里的this指的是调用这个方法的对象。
 **三、构造函数模式**
```javascript
        function Person(){
            console.log(this);
        }
        var obj =new Person();
```
注意：这里的this指的是new创建出来的对象。
 **四、上下文模式**
  这里我们用apply()和call()来自定义this的指向；
   这两个方法在Function的原型中，所以任何函数都可以使用它们； 这里简单介绍一下这两个方法：
  call和apply的两个参数 
  1.第一个参数都是要把this修改成的对象 
  2.当函数需要参数的时候，那么apply是用数组进行参数的传递
  3.而call是使用单个的参数进行传递 call和apply使用 
    1.call用于确定了函数的形参有多少个的时候使用 
    2.apply用于函数的形参个数不确定的情况 比如：
```javascript
function person(){
  console.log('hi');
}
function student(){
   person.call(this);
}
var stu=student();
stu();
```
这里的this指的是student，意思就是student这个对象来执行person里面的内容； 这样函数studengt里没有方法，却打印出了person里的语句，这里用apply或call一样的。
