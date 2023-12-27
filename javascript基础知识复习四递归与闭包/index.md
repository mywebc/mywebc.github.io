# javascript基础知识复习（四）递归与闭包

## 递归 
1. 递归概念：在函数内调用函数自己，就是递归。 注意：递归要有结束条件，没有递归结束条件的递归，就是死递归。 
2. 使用递归的方法：化归思想。化归思想是将一个问题由难化易，由繁化简，由复杂化简单的过程称为化归，它是转化和归结的简称。 
例子：求1-100的和 利用划归思想：var sum=foo(100); 1.求foo（100）即求foo（99）+100； 2.求foo（99），即求foo（98）+99； 3.求foo（98），即求foo（97）+98； ... 最后求foo（1），就是1;//这就是约束条件 最后利用递归，函数就是：
```javascript
function foo( n ) {
    if ( n == 1 ) return 1;
    return n + foo( n - 1 );
}
```
利用递归可以解决很多问题，比如算出阶乘，算出斐波那契数列等； 
<!--more-->
二、闭包 
1. 闭包概念：有权访问另一个函数作用域内变量的函数都是闭包。 
2. 闭包的原理是作用域访问原则，及上级作用域不能访问下级作用域。 
3. 闭包要解决的问题： a)闭包内的数据不允许外界访问 b)间接访问该数据 
4. 我们知道js中函数就是一个小黑屋，外部是不能访问内部变量的，但是内部变量是可以访问外部变量的，所以为能从外部访问，利用作用域访问原则，在函数里面再建一个函数，这个内部函数在外部函数内部可以直接访问外部函数的所有变量，我们再在全局里获取这个内部函数的返回值，就可以实现对外部函数里变量的读取和修改。 读取：

```javascript
function foo() {
    var num = Math.random();    
    function func() {
        return num;    
    }
    return func;
}
var f = foo();
var res1 = f();
```
return只能用一次，那如何读取多个数据呢：
```javascript
function foo () {
    var num1 = Math.random();
    var num2 = Math.random();
    //可以将多个函数包含在一个对象内进行返回，这样就能在函数外部操作当前函数内的多个变量
    return {
        num1: function () {
            return num1;
        },
        num2: function () {
            return num2;
        }
    }
}
 var f = foo();
console.log(f.num1());
console.log(f.num2());
```
使用闭包修改数据：
```javascript
 function foo() {
            var name = "高金彪";
            var gender = "female";

            return {

                getName:function () {
                    return name;
                },
                setName:function(value){
                    name = value;
                    return name;
                },
                setGender:function(value){
                    gender = value;
                    return gender;
                },
                getGender:function(){
                    return gender;
                }

            };
        }
        var obj = foo();
        console.log(obj.getGender());
        console.log(obj.setGender("雄"));
```
闭包的作用第一点就是上面讲了，他可以访问函数内部的变量并修改，第二点可以在函数内部创建私有空间，设置安全措施、校验之类的操作，比如：
```javascript
 function foo(){
    var name = "lili";
    return {
        getName: function () {
            return name;
        },
        setName: function (value) {
        //在这里写一些逻辑验证操作
            if(value==0){
            throw "名字不能为0！！";
            return name;
                }
                else{
                        name = value;
                        return name;
                        }
        }                 
    }
}
```
最后再总结下，这里介绍了闭包的概念，基本使用，和基本作用，要加深闭包的理解还需要做很多练习。
