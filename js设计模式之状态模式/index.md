# js设计模式之状态模式

>状态模式往往会带来代码量的增加，但是它也许是解决某些需求场景的最好方法，状态模式的关键是区分事物内部的状态，事物内部的状态往往会带来事物行为的改变。

<!-- more -->
### if/else/switch的情况
实际开发中，我们可能会遇到使用大量if/else/switch的情况，根据不同的状态，通过判断执行不同状态对应的逻辑操作，比如如下
```javascript
//执行动作
function doAction(state){
	//state0
	if(state === '0'){
		console.log('执行0');
	}
	//state1
	if(state === '1'){
		console.log('执行1');
	}
	//state2
	if(state === '2'){
		console.log('执行2');
	}
}
```
状态变多即使用switch也会越来越繁琐，这时候就可以使用状态模式了。
### 状态模式改善
状态模式，就是将每一种条件作为对象内部的一种状态，面对不同的判断结果，我们只需选择不同的状态便可.
```javascript
var ResultState = function(){
	//各种情况的业务逻辑保存在内部状态中
	var states = {
		state1:function(){
			console.log('情况一的业务逻辑');
		},
		state2:function(){
			console.log('情况二的业务逻辑');
		},
		state3:function(){
			console.log('情况三的业务逻辑');
		}
	}
	//获取某一状态的对应逻辑并执行
	function show(state){
		states[state] && states[state]();
	}
	return {
		doActionByState: show
	}
}();
// 传入状态，即会执行状态所对应的业务逻辑。
ResultState.doActionByState('state1');
```
### promise
es6中的promise内部就是一个状态机。


