# javascript基础知识复习（一）基本概念


**一、js的组成** 

* ECMAScript：js的核心，规范了js的语法； 
* DOM：文档对象模型，提供了操作DOM元素的API； 
* BOM：浏览器对象模型，对浏览器对象API的操作； 

**二、js的数据类型** 

* 简单数据类型（也称基本数据类型）：Undefined,Null,Boolean,String,Number; 
* 复杂数据类型：Object，本质由一组无序的名值对组成； 

**三、js中的部分关键字** 

* in：判断属性是否存在于对象中，for in遍历对象的键； 
* typeOf:判断对象的类型，返回是String类型； 
* delete： 
    1. 删除对象的属性; 
    2. 删除未使用var声明的变量; 
    3. 返回值为boolean 表示是否删除成功; 注意：删除的属性如果不存在，返回true; 删除的如果是原型中的属性，返回true 但是删除不成功; 
* ==和=== 
    (a)==：判断值是否相等； 
    (b)===：判断数据类型和值是否相等； 
* ||和&&； 

**四、js中的值类型和引用类型** 

* 值类型（又叫基本类型）：存储的是数据本身的值，有数值、布尔值、null、 undefined; 
值类型赋值：将数据赋值一份给新的变量，两份变量单独存在，互不影响； 
* 引用类型：存储的是数据的地址，而其中的数据会在内存中单独存储； 
引用类型赋值：会将存储数据的地址赋值一份，两个地址指向同一个变量，因此， 通过其中一个地址修改变量时，另一个地址指向的变量也会改变； 

**五、js中的异常处理** 

* 基本格式：
```javascript
try{
//可能出现异常的代码
}
catch(e){
//e就是出现异常的异常信息
//出现异常后的处理代码
}
finally{
//不管有没有出现异常，都会执行的代码
//一般用来释放资源
}

```
如何手动抛出异常： throw 任何东西， catch中会抓到该东西
