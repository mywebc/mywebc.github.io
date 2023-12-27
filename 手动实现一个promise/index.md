# 手动实现一个简单的promise

>一直在用promise，也想过自己能不能也实现一个简单的promise，但是一直没有时间；这不最近辞职了，时间就多起来了。当然也参考了网上很多人的实现方法。

<!-- more -->
> 本篇文章主要参考自[https://github.com/ElemeFE/node-practice/blob/master/control/promise/README.md](https://github.com/ElemeFE/node-practice/blob/master/control/promise/README.md)

>promise的意义就是能够很好的控制异步流程，避免回调地狱；首先来看一下promise的基本用法
## 基本实现效果
```javascript
let p = new Promisee((resolve, reject) => {
    setTimeout(() => {
        resolve('hello')
    }, 0)
})
p.then((val) => {
    console.log(val)
    return 'world'
})
.then((val) => {
    console.log(val)
})
```
## 基本书写
当我们想封装一个函数时，我们只关心两个东西，它需要输入什么以及它要输出什么。
* 输入一个函数接受两个回调参数
* 输出一个对象，里面为then函数，函数参数为成功回调和失败回调
```javascript
function Promisee(fn) {
    function resolve() {
    }

    function reject() {
    }
    
    fn(resolve, reject)
    // 返回一个对象
    return {
        then: function(onResolve, onReject) {
        }
    }
}
```
## 加入状态模式
promise内部使用了状态模式，根据不同的状态执行不同的逻辑
```javascript
// 状态唯一
const FULFILLED = Symbol();
const REJECTED = Symbol();
const PEDING = Symbol();

function Promisee(fn) {
    // 传入的必须是函数
    if (typeof fn !== "function") {
        throw new Error('param should be a function!')
    }
    let state = PEDING
    let value = null
    function resolve(result) {
         // 修改状态
        state = FULFILLED
        value = result
    }

    function reject(errror) {
        state = REJECTED
        value = errror
    }
    
    fn(resolve, reject)
    // 返回一个对象
    return {
        then: function(onResolve, onReject) {
            switch(state) {
                // 成功
                case FULFILLED: 
                    onResolve(value)
                break
                // 失败
                case REJECTED: 
                    onReject(value)
                break
            }
        }
    }
}
```
## 兼容异步
如果代码是异步，根据promise/a+规范，此时状态为peding,所以在peding状态下，我们需要将异步执行的回调函数先存放起来，等到状态变为FULFILLED，或者REJECTED时，再执行。
```javascript
// 状态唯一
const FULFILLED = Symbol();
const REJECTED = Symbol();
const PEDING = Symbol();

function Promisee(fn) {
    // 传入的必须是函数
    if (typeof fn !== "function") {
        throw new Error('param should be a function!')
    }
    let state = PEDING
    let value = null
    let handler = []
    
    function resolve(result) {
         // 修改状态
        state = FULFILLED
        value = result
        handler.forEach(next)
		handler = null
    }

    function reject(errror) {
        state = REJECTED
        value = errror
        handler.forEach(next)
		handler = null
    }
    
    fn(resolve, reject)
    // 分离
    function next({onResolve, onReject}) {
        switch(state) {
            // 成功
            case FULFILLED: 
            onResolve && onResolve(value)
                break
            // 失败
            case REJECTED: 
            onReject && onReject(value)
                break
            // 异步
            case PEDING:
                handler.push({ onResolve, onReject })
        }
		}
    // 返回一个对象
    return {
        then: function(onResolve, onReject) {
            next({onResolve, onReject})
        }
    }
}
```
## then链式调用
then的链式调用需要我们再返回一个promise
```javascript
// 状态唯一
const FULFILLED = Symbol();
const REJECTED = Symbol();
const PEDING = Symbol();

function Promisee(fn) {
    // 传入的必须是函数
    if (typeof fn !== "function") {
        throw new Error('param should be a function!')
    }
    let state = PEDING
    let value = null
    let handler = []
    
    function resolve(result) {
         // 修改状态
        state = FULFILLED
        value = result
        handler.forEach(next)
		handler = null
    }

    function reject(errror) {
        state = REJECTED
        value = errror
        handler.forEach(next)
		handler = null
    }
    
    fn(resolve, reject)
    // 分离
    function next({onResolve, onReject}) {
        switch(state) {
            // 成功
            case FULFILLED: 
            onResolve && onResolve(value)
                break
            // 失败
            case REJECTED: 
            onReject && onReject(value)
                break
            // 异步
            case PEDING:
                handler.push({ onResolve, onReject })
        }
		}
    // 返回一个对象
    return {
        then: function(onResolve, onReject) {
        	// 链式调用
            return new Promisee((resolve, reject) => {
                next({
                    onResolve: (val) => {
                    // 先调用上一层promise,再调用下一层并且沿用上一层返回的参数
                        resolve(onResolve(value))
                    },
                    onReject: (err) => {
                        reject(onReject(value))
                    }
                })
             })	
        }
    }
}
```


