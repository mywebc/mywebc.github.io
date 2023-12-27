# 旋转轮播图


> 与一般轮播图不同，用缓动动画做，我们需要简单封装一个缓动框架

<!--more-->
```js
   function animate (ele,json,fn){
      clearInterval(ele.timer);
      ele.timer = setInterval(function () {
       //开闭原则
       var bool = true;
       //遍历属性和值，分别单独处理json
       //attr == k(键)    target == json\[k\](值)
       for(var k in json){//遍历Jason里面的属性k
        //四部
        //获取他的属性时就需要单独处理
        var leader;
        //如果获取里面的属性是opacity时
        if(k==="opacity"){
            leader=getStyle(ele,k)*100||1;//获取到的是小数0.3
         }else {
             leader= parseInt(getStyle(ele,k)) || 0;//否则老样子
                } leader + step;
         //3.赋值
         //赋值时也要单独处理
         if(k==="opacity"){
              ele.style\[k\]=leader/100;//这种火狐chromeIE9以上都 可以
              //兼容IE678
              ele.style.filter="alpha(opacity="+leader+")";//这时leader是乘100的在这里透明度是以百分比
         //如果是层级，一次性赋值成功，不需要缓动赋值
              }else if (k==="zIndex"){
                   ele
         //1.获取步长
         var step = (json\[k\] - leader)/10;
         //2.二次加工步长
         step = step>0?Math.ceil(step):Math.floor(step);
         leader =.style.zIndex=json\[k\];
                    }
                   else {
                        ele.style\[k\] = leader + "px";
                    }
            //4.清除定时器
            //判断: 目标值和当前值的差大于步长，就不能跳出循环
            //不考虑小数的情况：目标位置和当前位置不相等，就不能清除清除定时器。
                 if(json\[k\] !== leader){
                     bool = false;
                  }
              }
            //只有所有的属性都到了指定位置，bool值才不会变成false；
            if(bool){
                clearInterval(ele.timer);
                //所有程序执行完毕了，现在可以执行回调函数了
                //只有传递了回调函数，才能执行
                    if(fn){
                        fn();
                    }
                }
            },25);
	}
```
* 兼容方法获取元素样式可以用这个
```js
        function getStyle(ele,attr){
            if(window.getComputedStyle){
                return window.getComputedStyle(ele,null)\[attr\];
            }
            return ele.currentStyle\[attr\];
        }
```
* 另外js中四中操作数组的方法： json.push();//在最后添加 json.pop();//删除最后一位 json.unshift();//在最前面添加 json.shift();//删除第一位  
