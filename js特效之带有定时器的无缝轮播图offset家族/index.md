# js特效之带有定时器的无缝轮播图（offset家族）


1.了解offset家族，这里用到offsetLeft(盒子距离左侧的距离)，与style.left不同，style.left可被赋值且盒子要定位。动画原理：盒子本身的位置+位长，用offsetLeft来获取值，style.left来赋值。以下是动画的简单封装。
```javascript
 function animate(ele,target) {
            //每次调用计时器时都要清除计时器
            clearInterval(ele.timer);
            //调用计时器
            //位长让它有正有负
            var speed=target>ele.offsetLeft ? 10:-10;
            ele.timer=setInterval(function () {
                var val=target-ele.offsetLeft;
                ele.style.left=ele.offsetLeft+speed+"px";
                if(Math.abs(val)<=10){
                    ele.style.left=target+"px";
                    clearInterval(ele.timer);
                }
            },30);
        }
```
2.思路：实现无缝滚动，先复制第一张图片到ul最后，当滚动到第六张时，将ul的offsetLeft值赋值为0（此时转到第一张），并将ul 的索引值赋值为1，切换到第二张，js代码如下：  
```javascript
  var all = document.getElementById("all");
  var screen = all.firstElementChild || all.firstChild;//all元素的第一个元素的第一个子节点
  var imgWidth = screen.offsetWidth;
  var ul = screen.firstElementChild || screen.firstChild;
  var div = screen.lastElementChild || screen.lastChild;
  var spanArr = div.children;
  var ol = screen.children\[1\];
  //2.复制第一张图片所在的li,添加到ul的最后面。
  var ulNewLi = ul.children\[0\].cloneNode(true);//复制ul中第一个节点
  ul.appendChild(ulNewLi);//添加到ul的最后,现在有6个li
  //3.给ol中添加li，ul中的个数-1个，并点亮第一个按钮。
  for(var i=0;i<ul.children.length-1;i++){
  var olNewLi = document.createElement("li");//创建一个li列表
  olNewLi.innerHTML=i+1;//在li列表里写上数字，并且要减一，因为他是从0开始的
  ol.appendChild(olNewLi);//再把这个li添加到ol中去
            }
   var olLiArr = ol.children;//找到ol中所有子节点
   olLiArr\[0\].className="current";//给第一个li添加一个类
   //4.鼠标放到ol的li上切换图片
   for(var i=0;i<olLiArr.length;i++){
        //排他思想
        olLiArr\[i\].index = i;//自定义属性，索引值绑定到index上
        olLiArr\[i\].onmouseover = function () {
           for(var j=0;j<olLiArr.length;j++){ 
olLiArr\[j\].className=""; } this.className="current"; key=square=this.index;
//要让定时器和索引值同步 animate(ul,-this.index*imgWidth); } }
 //5.添加定时器 var timer=setInterval(autoPlay,1000); var key=0;
//定义两个定时器,都等于0 var square=0; function autoPlay() { key++; 
//我们要对两个计时器进行约束 if(key>olLiArr.length){
 ul.style.left=0;//图片滑动到最后一张接下来跳转到第一张，然后在滑动到第二张
                    key=1;
                }
               animate(ul,-key*imgWidth);
               square++;
               if(square>olLiArr.length-1){
                   square=0;
               }
                for (var i=0;i<olLiArr.length;i++){//用排他思想
                    olLiArr\[i\].className="";//全都排除
                }
                olLiArr\[square\].className="current";//移到哪边，哪边就就有这个属性
            }
            //鼠标放上去清除定时器，离开启动定时器
            all.onmouseover=function () {
                clearInterval(timer);
            }
            all.onmouseout=function () {
                setInterval(autoPlay,1000);
            }
            //6.左右切换图片（鼠标放上去隐藏，移开显示）
            all.onmouseover=function () {
                div.style.display="block"
                clearInterval(timer);
            }
            all.onmouseout=function () {
                div.style.display="none"
                timer=setInterval(autoPlay,1000);
            }
            spanArr\[0\].onclick=function () {
                key--;
                //我们要对两个计时器进行约束
                if(key<0){
                    ul.style.left=-imgWidth*(olLiArr.length)+"px";
                    //现在给他5个图片的移动距离且是负值，让它一下子跳到最后最后一张图片
                    key=olLiArr.length-1;
                }
                animate(ul,-key*imgWidth);
                square--;
                if(square<0){
                    square=olLiArr.length-1;
                }
                for (var i=0;i<olLiArr.length;i++){
               //用排他思想 olLiArr\[i\].className="";//全都排除
 } 
olLiArr\[square\].className="current"; } spanArr\[1\].onclick=function () { autoPlay(); } 
//移动分装 
function animate(ele,target) { 
      clearInterval(ele.timer); 
      var speed=target>ele.offsetLeft ? 10:-10;
             ele.timer=setInterval(function () {
             var val=target-ele.offsetLeft;
             ele.style.left=ele.offsetLeft+speed+"px";
             if(Math.abs(val)<=10){
              ele.style.left=target+"px";
                 clearInterval(ele.timer);
                    }
                },10);
            }
```
