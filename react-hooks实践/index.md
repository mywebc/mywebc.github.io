# React Hooks实践

> React Hook 是React16.8提出来的一个新特性，其意义就在于我们可以让函数组件变得跟类组件一样有能力管理自己的状态，这意味着我们以后写的所有组件都可以是Function，对于初学者来说降低了学习成本（压根不用管this是个啥），而且我们可以自定义hook，能够提高状态逻辑的复用，从而也便于维护。

<!-- more -->
## State Hooks
### useState
* react为我们提供了useState这个函数，使用此函数就可以取代我们之前的每个组件的状态管理了；
```javascript
import { useState } from 'react'
function count() {
    // useState函数为我们解构了一个变量num和一个setNum函数，并且将变量初始值赋值为“0”
    const [num, setNum] = useState("0");
    // 我们在这里读取num的值， 并且每点击一次按钮都会使num+1
    return <button onClick={() => {setNum(num + 1)}}>{num}</button>
}
```
* 当我们每次改变num的值时，就会重新执行count函数，不过不用担心，num的初始化只会执行一次;

### useReducer
* 其实useState也是基于useReducer的，用过redux的人对reducer不陌生，reducer是一个纯函数，会根据action中type的不同返回改变后新的状态;
```JavaScript
import { useReducer } from 'react'
// 创建一个reducer
function myReducer(state, action){
    switch(action.type) {
        case "add": return state + 1
        case "minus": return state -1
        default:
         return state
    }
}
// 函数组件 
function count() {
    // 传入reducer和初始值
    const [num, dispatchNum] = useReducer(myReducer, "0");
    return <button onClick={() => {dispatchNum({type: "add"})}}>{num}</button>
}
```
## Effect Hook
* Effect Hook可以看作是代替了一些生命周期函数
```javascript
import { useState, useEffect } from 'react'

function count() {
    const [num, setNum] = useState("0");
    useEffect(() => {
        const interval = setInterval(() => {
            setNum(num + 1)
        }, 1000)
        // 最后要return一下，记得清除定时器
        return () => clearInterval(interval)
    }, [num])
    return <button onClick={() => {setNum(num + 1)}}>{num}</button>
}
```
* useEffect函数中第一个参数为组件第一次渲染时执行的函数（最后return的是定时器卸载的函数，可以看作是componentWillUnMount生命周期内做的事），第二参数表示依赖的值，只有依赖的值变化时，useEffect才会重新执行；

## Context Hook
* 顾名思义这是一个上下文hook

```javascript
// 新建一个context文件myContext.js
import React from 'react'
export default React.createContext("")
```
```javascript
// 在另一个文件中引入
import MyContext from './myContext'
// 通过MyContext.Provider 传值
<MyContext.Provider value="test">
    <Component/>
</MyContext.Provider>
```
```javascript
// 在组件中使用
import {useContext} from 'react'
import MyContext from './myContext'
function test() {
    const contextValue = useContext(MyContext)
    return <div>{contextValue}</div>
}
```
## Ref Hook
```javascript
import {useRef} from 'react'
function testRef() {
    const inputRef = useRef()
    useEffect(() => {
        console.log(inputRef)
    })
    return <input ref={inputRef}>测试ref</input>
}
```
## 自定义Hook
* hook其实就是一个函数，在这个函数里我们也可以使用其它hook,自定义的hook要以use开头,下面是官网上的例子，已经很好了；
```javascript
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```


