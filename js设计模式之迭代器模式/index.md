# js设计模式之迭代器模式

> 迭代器模式是指提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。总结两点，第一顺序访问一个集合，第二使用者无需知道集合的内部（封装）。

<!-- more -->
### 迭代器演示
```javascript
class Iterator {
    constructor(container) {
        this.list = container.list
        this.index = 0
    }
    next() {
        if(this.hasNext()) {
            return this.list[this.index++]
        }
        return null
    }
    hasNext() {
        if(this.index >= this.list.length) {
            return false
        }
        return true
    }
}

class Container {
    constructor(list) {
        this.list = list
    }
    // 生成遍历器
    getIterator() {
        return new Iterator(this)
    }
}
// 测试
let arr = [1,2,3,4,5]
let container = new Container(arr)
// 生成遍历器
let iterator = container.getIterator()
while(iterator.hasNext()) {
    console.log(iterator.next())
}
```
### es6中的迭代器
 js中已经有很多的函数默认内置迭代器 Set, forEach,这些数据类型都有[Symbol.iterator]这个属性可以通过Array.prototype[Symbol.iterator]来测试
以下为es6的iterator示例
```javascript
// 自己封装迭代器函数
function each(data) {
    // 生成的这个对象就有next方法
    let iterator = data[Symbol.iterator]()
    // 手动next
    console.log(iteator.next()) // {value: 1, done: false}
    console.log(iteator.next()) // {value: undefined, done: true}

    let item = {done: false}
    while(!item.done) {
        item = iterator.next()
        if(!item.done) {
            console.log(item.value)
        }
    }
}
```
es6不需要每次都要书写以上函数，它提供了一种新语法for...of...
for...of...能够遍历带有遍历器特性的数组即它有[Symbol.iterator]这个属性
### Generator
Generator函数返回的对象中也有[Symbol.iterator]这个属性，说明它也实现迭代器的接口，不过现在应该也不怎么用了。

