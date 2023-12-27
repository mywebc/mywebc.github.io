# javascript基础知识复习（三）js中的继承


js中共有6种继承 
**一、原型链**
```javascript
function SuperType(){
    this.prototype=true;
}
SuperType.prototype.getSuperValue=function(){
    return this.property
}
function SubType(){
   this.subPrototy=false;
}
//继承了SuperType
SubType.prototype=new SuperType();
SubType.prototype.getSubValue=function(){
    return this.subProperty;
}
var instance = new SubType();
console,log(instance.getSuperValue);//true
console.log(instance.getSubValue);//false
```
以上代码我们可以看到定义两个构造函数，里面有属性，原型里面有方法，我们让SubType的原型等于SuperType的实例，通过替换了SubType的默认原型对象实现继承，我们可以看到SubType的实例成功调用SuperType里的方法，之后我们又为其手动添加了一个新的方法也是可以调用的。 

注意：继承原型后使用字面量添加方法，会使继承无效，因为已经替换了原型，我们在上一篇中介绍原型时已经讲过。 
缺点：（1）继承后的原型对应的构造函数它所有的实例都会共享原型中的方法。 （2）继承的子类和父类，子类是不能向父类传参数的。 

**二、借用构造函数**

```javascript
function SuperType (){
       this.colors=['red','blue'];
}
function SubType(){
//用call或者apply将调用对象换成SuperType实现对SuperType函数的继承
//this指向当前对象
       SuperType.call(this);
}
var o=new SubType();
o.colors.push('black');
console.log(o);//red,blue,black
var o1=new SubType();
console.log(o1)//red,blue

//利用call()或apply()在子类函数中调用父类函数实现继承，我们可以看到SubType的实例并不共享属性；而且子类也是可以向父类传参的

function SuperType (name){
       this.name=name;
}
function SubType(){
       SuperType.call(this，'lili');
       this.age=12;
}
var o=new SubType();
console.log(o.name)//lili
console.log(o.age)//12
```
我们可以看到子类传入参数后，调用父类函数直接赋值给属性name。为了确保父类的属性不会覆盖子类，在调用父类函数后再添加新属性。 缺点：方法都在构造函数中定义，无法函数复用，父类的原型中的方法对于子类也是不可见的。

 **三、组合继承（原型链和借用构造函数）**
 
```javascript
function SuperType(){
   this.name=name;
   this.colors=\['red','blue'\];
}
SuperType.prototype.sayName=function(){
   alert(this.name);
}
function SubType(name,age){
   SuperType.call(this,name);
   this.age=age;
}
SubType.prototype=new SuperType();
SubType.prototype.sayAge=function(){
   alert(this.age);
}
var o=new SubType('lili','23');
o.colors.push('black');
alert(o.color);//red,blue,black
alert(o.name);//lili
alert(o.age);//23
var o1=new SubType('hi','21');
alert(o1.name);//hi
alert(o1.age);//21
```
我们可以看到父类函数里有两个属性，原型里有一个方法，用原型链继承原型里的方法，这样他们的方法共享，借用构造函数的方法继承了父类函数里的属性，属性不共享，这样每个实例都有自己的属性，和使用相同的方法了。 组合继承避免了原型链和借用构造函数的缺陷，是JavaScript中最常用的继承模式。 

**四、原型式继承** 

《JavaScript语言精粹》作者克罗克福德提出了一个方式来实现继承
```javascript
function object(o){
            function F(){}
            F.prototype = o;
            return new F();
        }
  
        var person = {
            name: "Nicholas",
            friends: \["Shelby", "Court", "Van"\]
        };
        
        var anotherPerson = object(person);
        anotherPerson.name = "Greg";
        console.log(anotherPerson.name);//Greg
        anotherPerson.friends.push("Rob");
        
        var yetAnotherPerson = object(person);
        yetAnotherPerson.name = "Linda";
        console.log(yetAnotherPerson.name);//Linda
        yetAnotherPerson.friends.push("Barbie");
        
        alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"
        alert(person.name);//Nicholas，名字不会被修改
```
我们可以看到object函数中创建了一个临时性的构造函数，将传入的参数等于构造函数的原型，最后返回这个构造函数的实例； 这种原型式继承要求必须要有一个对象作为另一个对象使用的基础。所以我们要先定义一个person对象，之后我们通过函数定义两个实例，并将参数person传入，两个实例成员都有对象person的属性，也可以修改。 ES5中推出了Object.create()规范了原型式继承，方法接受两个参数，第一个就是你准备的参数，第二个（可选）就是为一个新对象定义额外属性的对象。
```javascript
var person = {
            name: "Nicholas",
            friends: ["Shelby", "Court", "Van"]
        };
        
        var anotherPerson = Object.create(person);
        anotherPerson.name = "Greg";
        anotherPerson.friends.push("Rob");
        
        var yetAnotherPerson = Object.create(person);
        yetAnotherPerson.name = "Linda";
        yetAnotherPerson.friends.push("Barbie");
        
        alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"

Object.create()存在兼容性问题，因此不推荐使用; 解决兼容性问题封装一个自定义函数：

 function create(obj){
            if(Object.create){
                return Object.create(obj);
            }else{
                function F() {
                }
                F.prototype = obj;
                return new F();
            }
        }
```

**五、寄生式继承** 

寄生式继承也是克罗克福德提出的
```javascript
function createAnother(original){
//object函数不是必须的，任何能返回对象的函数都可以
     var clone=object(original);
     clone.sayHi=function(){
          alert('hi');
};
return clone;
}
var person={
  name :'lili';
  friends:['jack','van'];
};
var anotherPerson=createAnother(person);
anotherPerson.sayHi();//hi
```
寄生式继承的思路跟工厂模式差不多，封装一个函数，函数接受一个参数，参数就是将要作为新对象基础的对象，然后把这对象传给object函数，返回值给clone，再为clone对象添加一个方法，最后返回clone对象。我们可以看到person返回了一个新对象- anotherPerson，新对象不仅有person 的所有属性和方法，还有自己的sayHi方法 缺点：还是函数不能复用的问题; 

**六、寄生组合式继承** 

我们之前说过Js的继承用的最多的是第三种组合继承，但是他也有缺点，他会调用两次超类型构造函数（父类函数），第一次调用会创建实例属性name和colors，第二次调用又会如此，这样就覆盖了原来的属性，所以寄生组合式继承是对第一次调用函数的优化，只需创建一个超类型函数的副本即可，这样子类就只调用一次超类型构造函数。
```javascript
function  inheritPrototype(subType,SuperType){
  var prototype=Object(SuperType. prototype);//赋值一份父类的原型
  subType.prototype=prototype;//将赋值的原型指向子类的原型
  preototype.constructor=subType;//赋值的原型里的constructor失效需要重新指向
}
//所以原来的组合继承可以这样改
function SuperType(){
   this.name=name;
   this.colors=\['red','blue'\];
}
SuperType.prototype.sayName=function(){
   alert(this.name);
}
function SubType(name,age){
   SuperType.call(this,name);
   this.age=age;
}
//SubType.prototype=new SuperType();//第一次调用父类函数
//替换成这个
inheritPrototype(subType,SuperType);
SubType.prototype.sayAge=function(){
   alert(this.age);
}
```
