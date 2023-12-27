# ES6笔记（四）扩展对象的功能性


### 对象的语法的扩展

#### (1)对象的属性或属性值可以直接传入变量

``` ts
let id = 12; let value = 22; let obj = {id:value} 
```

#### (2)在对象中方法的简写

``` ts
//es5 
let obj = { 
    handle:function(){ 
        //dosomething 
        } 
    } 
//es6 
let obj = {
     handle(){ 
        //dosomething 
        } 
    } 
```
<!--more-->
#### (3)属性初始值简写

``` ts
//ES5 
function a(id) {
        return { id: id }; 
     }; 
     //ES6
const a = (id) => ({ id }) 
```

### es6对象新增的方法

#### (1)Object.is()

比较两种数据类型，以前可以用===或者==，现在可以用Object.is()，不过Object.is()比较严格： 
``` ts
console.log(+0 === -0)
//true 
console.log(Object.is(+0, -0))
//false
```

#### (2)Object.assign()

Object.assign()这个方法早有接触过，合并对象以及浅拷贝,具体看如下示例： 
``` ts
 object.assign(target,...obj) 
```
注意一下，obj中会覆盖target中重复的属性

### 对象被遍历自动枚举

示例： 
``` ts
const state = { id: 1, 5: 5, name: "eryue", 3: 3 } 
Object.assign(state, null) 
//{"3":3,"5":5,"id":1,"name":"eryue"} 
``` 
还有其他的object.keys()和for...in都会枚举。

### 对象原型的增强

Object.setPrototypeOf()还有super()。
