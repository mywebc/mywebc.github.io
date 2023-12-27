# js设计模式之代理模式

>代理模式的定义：为一个对象提供代用品或占位符，以便控制对它的访问；注意使用者无权访问目标对象的。

<!-- more -->

* 最常见的就是科学上网了

代理模式根据其目的，也有好多种代理
1. 保护代理（代理掉不必要的请求）
2. 虚拟代理 （创建开销很大的对象时，可以先用一个小对象代替，等到真正使用时再创建）
3. 缓存代理 （为一些开销大的运算提供暂时的存储，如果下次传的参数一致时，可以返回这个结果）
4. 其他代理（在js中适用性不高）

### 保护代理
```javascript
// 这个类外面是无权访问的
class ReadImg {
    constructor(fileName) {
        this.fileName = fileName
        this.loadFormDIsk() // 加载，模拟
    }
    loadFormDIsk() {
        console.log('loading...' + this.fileName)
    }
    display() {
        console.log('display...' + this.fileName)
    }
}

// 定义代理函数
class ProxyImg {
    constructor(fileName) {
        this.realImg = new ReadImg(fileName)
    }
    display() {
        this.realImg.display()
    }
}
let proxyImg = new ProxyImg('1.png')
```
#### e6中的proxy
明星经纪人案例
```javascript
let start = {
    name: 'steven',
    age: 24,
    phone: '12334445'
}

// 经纪人来代理
let agent = new Proxy(start, {
    get: function(target, key) {
        if(key === 'phone') {
            // 给经纪人自己的号码
            return '111121111'
        }
        if(key === 'price') {
            // 经纪人来报价
            return 1232333
        }
        return target[key]
    },
    set: function(target, key, val) {
        if(key === 'customPrice') {
            if(val < 10000) {
                throw new Error("太低")
            }else {
                target[key] = val
                return true
            }
        }
    }
})
```
### 虚拟代理
图片预加载
```JavaScript
var myImage = (function(){
    var imgNode = document.createElement("img")
    document.body.appendChild(imgNode)
    return {
        setSrc: function(src) {
            imgNode.src = src
        }
    }
})()
var proxyImage = (function(){
    var img = new Image
    img.onload = function(){
        myImage.setSrc(this.src)
    }
    return {
        setSrc: function(src) {
            // 占位图片
            myImage.setSrc("http://123.png")
            img.src = src
        }
    }
})()
// 通过代理设置图片地址，在onload之前会出现占位图片
proxyImage.setSrc("http://pic.jpg")
```
### 缓存代理
比如vue中的计算属性computed,它和methods的区别就是可以缓存数据


