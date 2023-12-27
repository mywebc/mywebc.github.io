# js设计模式之单例模式

> 单例模式的定义: 保证一个类仅有一个实例，并且提供一个访问它的全局访问点。意义为减少内存开支，减少变量冲突。

<!-- more -->
### 常见的应用场景
1. 全局的window对象，Jquery中的$对象
2. vuex和redux中的state
3. 系统间各种模式的通信协调上

### 模式实现
最简单的实现，对象字面量的方法
```JavaScript
let singleton  = {
    age: 11,
    name: "frank"
}
```
对象复杂的时候，就需要构造函数，简单的来说当new的时候先判断实例是否存在，如果存在直接返回，不存在就创建一个再返回，这样保证了返回的实例就是同一个。

```JavaScript
class Singleton  {
    constructor() {
        this.instance = null;
    }
    // 定义一个静态方法，实例化只能通过静态方法。 
    static getInstance(name) {
        if(!this.instance) {
            this.instance = new Singleton();
        }
        return this.instance;
    }
}
let a = Singleton.getInstance()
let b = Singleton.getInstance()
console.log(a === b)
```
### 模拟登陆框
```javascript
class LoginForm {
    constructor {
        this.state = 'hide'
    }
    show() {
        // 如果当前已经显示
        if(this.state === 'show') {
            alert('已经显示')
            return
        }
        this.state = 'show'
        console.log('登录框显示成功')
    }
    hide() {
         // 如果当前已经显示
        if(this.state === 'hide') {
            alert('已经隐藏')
            return
        }
        this.state = 'hide'
        console.log('登录框隐藏成功')
    }
}
LoginForm.getInstance = (function(){
    let instance
    return function () {
        if (!instance) {
            instance = new LoginForm()
        }
        return instance
    }
})()

// 测试
let login1 = LoginForm.getInstance()
login1.show()
let login2 = LoginForm.getInstance()
login2.hide()
``` 




