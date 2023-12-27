# javascript基础知识复习（二）面向对象


**一、什么是面向对象？与面向过程又有什么关系？** 
1. 面向对象是一种思维方式，解决的重点放在对象上； 
2. 面向过程也是一种思维方式，解决的重点放在详细的步骤上； 举个例子：如何去超市买东西？ 
    * 从面向对象的角度出发，我们只需要一辆车，还有自己就好了； 
    * 从面向过程的角度出发，我们要先拿钱，打开门，开车，进超市； 一句话概括它们之间的关系就是：面向对象是面向过程的封装。

 **二、js中的对象** 

 js中键值对的集合就是对象。 

**三、创建对象的几种方法** 

1. 使用字面量创建对象
```javascript
var 0={name:'lili',age:'12'};
```
这种方法会造成代码冗余，资源浪费； 

2. 使用内置构造函数创建对象
```javascript
var o =new Object();
o.name='lili';
o.age='12';
```

创建的代码是空对象，需要为其手动添加对象属性； 

3. 工厂模式
```javascript
function createObj(age,name){
var o=new Object();
o.age=age;
o.name=name;
o.sayHi=function(){
   alert('你好')；
   }；
return o;
}
var o1=new createObj(){'12','lili'};
```
每次调用时都会为这个函数在内存中开辟空间，浪费资源。 

4. 自定义构造函数，注意函数名要大写
```javascript
function MyObj(name,age){
   this.name=name;
   this.age=age;
   this.sayHi=function(){
   console.log('hi');
  };
}
var myobj=new MyObj('lili','12');
```
this指的是new出来的对象（即调用者）,和工厂模式差不多，每次调用的时候都会为里面的函数开辟新的内存地址，但是函数的代码功能却是相同的，造成内存资源浪费。

5. 自定义构造函数模式原型模式组合 所以我们需要引入原型概念，将构造函数里的属性和方法分开，将方法放到原型里实现共享。 
    原型概念：在构造函数创建出来的时候，系统会默认的帮构造函数创建并关联一个神秘的对象，这个对象就是原型，原型默认的是一个空的对象。
    原型中的属性和方法，都可以被使用该构造函数创建出来的对象使用。给原型添加属性和方法可以使用构造函数的prototype属性，所以第四种方法可以这样修改：
```javascript
function MyObj(name,age){
   this.name=name;
   this.age=age;
}
MyObj.prototype.saiHi=function(){
  console.log('hi');
}
var myobj=new MyObj('lili','12');
myobj.sayHi();//可以调用
```
这种模式是目前用的最多的,这里是通过构造函数的prototype属性访问原型，也可以对象的__proto__属性访问（主要调试用，不推荐这样使用）。 

6. 动态的原型模式 调用函数前先判断是否需要用到原型，如果需要就初始化，不需要的话，这样比第五种就少了初始化这个步骤。用instanceof或者typeof来判断其类型。
```javascript
function Person (name,age,job){
//属性
this.name=name;
this.age=age;
this.job=job;
//方法
if(typeof this.sayName!='function'){
Person.prototype.sayName=function(){
   alert('this.name');
   }
 }
}
```

7. 寄生构造函数模式

```javascript
 function Person(name, age, job){
            var o = new Object();
            o.name = name;
            o.age = age;
            o.job = job;
            o.sayName = function(){
                alert(this.name);
            };    
            return o;
        }
        
        var friend = new Person("Nicholas", 29, "Software Engineer");
        friend.sayName();  //"Nicholas"
```

这个模式其实很少用，跟工厂模式差不多，只不过工厂模式没有通过new来生成对象。假设我们需要为对象添加一个额外的方法，可以使用这个模式。因为如果在构造函数中直接添加，会污染其他对象，如果在原型添加，会使所有的实例继承这个方法，没必要，所以我添加的这个方法是跟构造函数和其原型是没有任何关系的。下面我想为对象colors添加一个toPipedString方法就可以用这个模式。

```javascript
 function Person(name, age, job){
            var o = new Object();
            o.name = name;
            o.age = age;
            o.job = job;
            o.sayName = function(){
                alert(this.name);
            };    
            return o;
        }
        var friend = new Person("Nicholas", 29, "Software Engineer");
        friend.sayName();  //"Nicholas"
```
8. 稳妥构造函数模式 稳妥构造函数采用的是与寄生构造函数模式相似的模式，除了下列两点不同：

* 创建对象的实例方法不引用this；
* 不使用new调用构造函数；,可以在一些安全环境下使用这个模式，函数里的私有属性是不可以被修改的。

```javascript
function Person(name, age, job) {
   //创建要返回的对象
    var o = new Object();
    //可以在这里定义私有变量和函数
     o.name=name;
     o.age=age;
    //添加方法
    o.sayName = function() {
        alert(name);
    }

    //返回对象
    return o;
}
  var friend = Person('Nicholas', 29, 'Software Engineer');
    friend.sayName(); // Nicholas
    Person.name='lili';
    console.log(friend.name);//Nicholas,修改不了name
    console.log(friend.age);//29
```

**五、关于原型其它方面** 

1. 原型链：每个构造函数都有原型对象，每个对象都会有构造函数，每个构造函数的原型都是一个对象，那么这个原型对象也会有构造函数，那么这个原型对象的构造函数也会有原型对象，这
样就会形成一个链式的结构，称为原型链。 

2. 属性搜索原则：
    1. 当访问一个对象的成员的时候，会现在自身找有没有,如果找到直接使用， 
    2. 如果没有找到，则去当前对象的原型对象中去查找，如果找到了直接使用， 
    3. 如果没有找到，继续找原型对象的原型对象，如果找到了，直接使用， 
    4. 如果没有找到，则继续向上查找，直到Object.prototype，如果还是没有，就报错。 

3. 替换原型对象
```javascript
 function Person(){
        }
        Person.prototype.sayHello = function () {
            console.log("nice to meet you");
        }
        var p = new Person();
        //下面这种格式（使用字面量的方式赋值）就是替换了原型对象
        Person.prototype = {
        };
```
原型对象被替换后我们访问原来原型对象中的方法是访问不到的，原型对象中有一个constructor属性，指向构造函数，我们需要手动赋值，保证构造函数---原型----对象之间这样的三角关系，格式  constructor  函数名 

4. 原型继承 原型继承：利用原型中的成员可以被和其相关的对象共享这一特性，可以实现继承这种 实现继承的方式，就叫做原型继承。（原型继承会在js中的继承中讲到） 最后给出原型链的完整图：
