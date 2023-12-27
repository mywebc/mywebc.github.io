# js设计模式之适配器模式

>适配器模式是一种简单设计模式，主要是用来解决老接口不兼容的问题，对于不兼容的老接口，我们没必要重写，只需创建一个适配器即可。

<!-- more -->

### 获取城市接口例子
现在我们有一个接口用来获取广东省所有的城市，如下
```javascript
    // 模拟接口，返回广东城市数据
    let getGuanDongCity = function() {
        let guanDongCity = [
            {
                name: 'shenzhen',
                id: 11
            },
            {
                name: 'guangzhou',
                id: 12
            }
        ]
        return guanDongCity
    }
    // 定义渲染函数
    let render = function(fn) {
        document.write(JSON.stringify(fn()))
    }
    // 渲染
    render(getGuanDongCity)
```
然后我们发现这个接口显示的城市并不齐全，我们又找到了另外一个齐全的接口作为补充，这个接口返回的数据格式和原来返回的数据格式不一样，这时候我们要改写原来的接口吗，nono,我们只要为原来的接口包装一个适配函数即可。
```javascript
    // 新的补充接口的数据格式
    let guanDongCity = {
        shenzhen: 11,
        guangzhou: 12,
        zhuhai: 13
    }
```
为原来接口编写适配函数
```javascript
    let addressAdapter = function( oldAddressfn ) {
        let address = {}
        let oldAddress = oldAddressfn()
        oldAddress.map((ele) => {
            address[ele.name] = ele.id
        })
        return function(){
            return address
        }
    }
    // 这样渲染的数据格式就一样了
    render(addressAdapter(getGuanDongCity))
```
### 那些似曾相识的场景
1. jquery的封装
```javascript
// 一个项目之前封装了一个ajax方法如下
 ajax({
    url: '/getData',
    type: 'post',
    data: {
        id: 1
    }
})
// 现在我们需要这么调用ajax
$.ajax({
    url: '/getData',
    type: 'post',
    data: {
        id: 1
    }
})
```
我们不可能修改每一处调用ajax的地方,我们可以在封装ajax的地方做一层适配器如下
```javascript
var $ = {
    ajax:function(options) {
        return ajax(options)
    }
}
```

仔细想想，其实这种模式我们每天都在用，当后台返回的数据结构不满足前端页面所需要的结构时，而后台接口又很难改时，我们只要需要编写一个适配函数即可。（这不就是我每天经常干的嘛）

