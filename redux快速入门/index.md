# redux快速入门

> redux和vuex一样，当项目数据复杂时，都是用来集中管理数据的，不过redux是单向数据流绑定,这就使得redux中的store必须放到所有组件最顶层，此篇文章是在看《深入浅出react与redux》以及实战React16.4开发简书项目后的总结。

<!-- more -->
## Redux的前身flux
在flux中，首先要引入Dispatcher（redux没有这个）， 用以派发action，action改变view层，view层同步model数据层；flux会为每一个组件单独建立一个store，在每一个组件中初始化store,如下：
```javascript
const CounterStore = object.assign({}, EventEmitter.prototype, {
    getCounterValues: function() {
        return counterValues;
    },
    emitChange: function(){
        this.emit(CHANGE_EVENT);             
    },
    addChangeListener: function(){
        this.on(CHANGE_EVENT, callback);     
    },
    removeChangeListener: fucntion(){
        this.removeListener(CHANGE_EVENT, callback);     
    }
})
```
EventEmitter是node.js中event模块的一个对象，是监听事件的一个封装，不用太追究；上面扩展了EventEmitter.prototype，给当前组件的store绑定了几个事件；初始化store之后，还要与dispatcher做联系，这时候会用到dispatcher的一个register函数，接受参数为action；

Dispatcher是唯一的，如果有很多个组件，Dispatcher与他们每一个组件都要做联系，通过waitFor函数来决定各个store之间的执行顺序，但这就是flux最大的缺点，因为如果store很多的话，依赖关系会越来越复杂，逻辑关系会难以维护。
## Redux
在Redux中没有了Dispatcher的概念，store倒是有一个dispatch方法，用以来传递action给store
![reduxFlow](/images/reduxFlow.jpg)
如图,解释一下流程
* 在组件内部会调用actionCreators，用于创建action对象，并且通过dispatch传送给store；
* store接受到后，会交给reducer（纯函数） 处理；
* reducer函数处理后会返回新的store状态；
* store里的数据发生变化会自动触发视图层变化，结束；

下面会一一解释store，reducer，action如何创建，并且如何走通。
首先创建一个store文件夹，下面会有四个js，分别是
* index.js // 作为入口，统一引入到这里
* actionCreators.js // 用来统一创建对象
* constants.js // 统一定义action的type,方便后期维护log
* reducer.js // 用来处理action的纯函数

### 关于action
> 分割action为actionCreators和contants,方便管理维护

actionCreators.js
```javascript
import * as constants from './constants'
// type为常量，从constants中引入，方便log
export const sayHello = (text) =>({
	type: constants.SAY_HELLO，
        text
})
```
constants.js
```javascript
export const SAY_HELLO = 'SAY_HELLO'
```
这个其实还是跟vuex中action一样的，很好理解。
### 创建store
> 调用createStore创建store，参数传入处理函数reducer

```javascript
import { createStore } from 'redux'
import reducer from './reducer.js'
// 调用createStore创建store，参数传入处理函数reducer
const store = createStore(reducer);
export default store
```
### 编写reducer
> reducer默认接受state初始状态，和action对象，更改后的state

```javascript
import * as constants from './constants'
// 设置state初始值 
const defaultState ={
    text: ''
}
// 默认暴露一个纯函数
export default (state = defaultState, action)=> {
    switch(action.type) {
        case constants.SAY_HELLO:
            return {...state,  action.text}
		default: 
			return state
	}
}
```
> 改有的文件都已经创建好，现在就可以捋一遍

首先在组件内部，我们想要改变store里的数据，首先要引入刚刚创建好的action，调用store的dispatch方法将action传递给store；
```javascript
// 引入store
import store from './store'
// 引入action
import * as actionCreators from './actionCreators' 
// store.dispatch派发action
store.dispatch(actionCreators.sayHello('你好！'))
```
store收到action后会自动交给reducer处理，reducer根据其actionType的不同做不同的操作，并且返回更改后的st ate；
```javascript
export default (state = defaultState, action)=> {
    switch(action.type) {
        case constants.SAY_HELLO:
        // 扩展运算符
            return {...state,  action.text}
		default: 
			return state
	}
}
```
组件内部通过store.subscribe()的方法能够监听store的变化，store一旦变化就可以调用store.getState()替换当前组件的store；
```javascript
    // store数据变化后触发 handleCHange函数
    store.subscribe(this.handleCHange)
    // handleCHange函数 替换组件内部store数据
    handleCHange() {
        store.setState({
            store.getState()
        })
    }
```
store里面的数据必须用setState更改，setState可以是一个对象，也可以是一个函数，return一个对象，后一种写法推荐，因为官网就是这么写的。
数据更新那么视图层就会自动更新，结束。

## React-Redux
> 在react中有为react量身定做的react-redux库，其用法也有一些细微差别
### react-redux自带的provider组件
```javascript
<Provider store={store}>
    <Hello/>
</Provider>
```
从组件根部传入store，在其所有子组件中都可以使用，具体如下：
```javascript
const mapStateProps = (state) =>({
    text: state.text
})
const mapDispathProps = (dispath) =>{
    return {
       changeText() {
            //这里dispatch一个action
        } 
    }
}
// 通过connect连接当前组件
export default connect(mapStateProps, mapDispathProps)(Hello)
```
* mapStateProps：顾名思义就是将state当作props传入当前组件；
* mapDispathProps：这个就是将Dispatch当作props传入组件；
* 这两个需要在组件外定义，然后当作参数传入connect中；

### combineReducers
一般一个组件都会定义一个store，那么多的store，里面有那么多的reducer.js,我们可以用combineReducer来合并； 
```javascript
import { combineReducers } from 'redux'
import { reducer as homeReducer } from '../store'
const reducer = combineReducers({
	header: homeReducer // 可以为每一个reducer取一个名字
})
```
那么我们在组件获取状态时也要加上名字,改成如下：
```javascript
const mapStateProps = (state) =>({
    text: state.header.text
})
```
## 快速开始模板
Github: [https://github.com/mywebc/react_start](https://github.com/mywebc/react_start)
