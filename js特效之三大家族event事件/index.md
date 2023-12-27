# js特效之三大家族、event事件


老是忘，好好整理一下。 
三大家族：offset（位移）、scroll（卷页）、client（可视区）。 
### offset家族( 5个属性) 
* offsetWidth和offsetHeight：检测盒子自身的宽高； 注意：包含盒子的padding和border。 offsetWidth=width+padding+border; offsetHeight=height+padding+border； 
* offsetLeft和offsetTop：检测距离父盒子有定位的左侧和上面的距离； 注意：父盒子没有定位，往上找有定位的盒子，如果都没有以body为准，从父盒子的 padding开始算，border不算。 
* offsetParent：用于获取该元素中有定位的最近父级元素； 注意：如果当前元素的父级元素都没有进行定位,那么offsetParent为body。 （4）与style.width/height/top/left的比较； 
 1. offset家族只可以只读，而style系列可以读写； 

 2. offset家族返回的是数值类型（四舍五入），style系列返回的是字符串（带px），特殊情况：在父盒子有定位的情况下，offsetLeft==style.left（没有 px）; 

 3. offsetLeft 和 offsetTop 可以返回没有定位的元素的left值和top值,而style 不可以。

### scroll家族（4个属性） 
* scrollWidth和scrollHeight：检测内容的宽高； 注意：如果内容超出盒子，显示内容的高度，如果不超出盒子，IE567可以显示实际内 容宽高，IE8+火狐+谷歌显示盒子大小。

* scrollTop和scrollLeft（有兼容性问题）：网页被浏览器遮住的头部和左边的部 分； 

* 处理兼容性问题： - 未声明 DTD（谷歌只认识他）（火狐IE9+认识他） 
document.body.scrollTop/scrollLeft - 已经声明DTD（IE678只认识他）(火狐IE9+认识他) 
document.documentElement.scrollTop/scrollLeft - 火狐/谷歌/ie9+以上支持的(不管DTD) 
window.pageYOffest/pageXOffest 
兼容性写法： var scrollTop = window.pageYOffset ||document.documentElement.scrollTop || document.body.scrollTop||0; 封装scroll方法 function Scroll() { return { "left": window.pageXOffset || document.body.scrollLeft || document.documentElement.scrollLeft || 0, "top": window.pageYOffset || document.body.scrollTop || document.documentElement.scrollTop || 0 }; } 用scroll().top或scroll().left来获取值。 

### client家族（6个属性） 
* clientWidth和clientHeight：网页可视区内容宽高； 注意：盒子调用：指盒子本身宽高（包括padding），HTML/body调用指可视区域大 小。
* clientX和clientY:鼠标距离可视区域左侧、上面的距离(event调用）； 
* clinetTop和clientLeft:盒子的border宽高； event事件：在触发某个事件时都会产生对象event，里面包含了一些事件信息。 为了兼容ie678，可以这样写 event=event||window.event; 其中有6重要属性： （1）pageX和pageY：光标在网页中的位置(距离左侧和上侧)；//参照点为左上角，算滚动区域 （2）screenX和screenYY：光标在屏幕中的位置(距离左侧和上侧)；//相对于屏幕 （3）clientX和clientY:光标在浏览器中位置(距离左侧和上侧)；//参照点为左上角，不会算上滚动区域
