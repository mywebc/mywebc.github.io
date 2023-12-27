# 移动端的touch事件


> 移动端的touch主要用到一下三种： 

<!-- more -->

* touchstart://手指放到屏幕上时触发 
* touchmove:指在屏幕上滑动式触发 
* touchend://手指离开屏幕时触发 每个触摸事件被触发后，会生成一个event对象，
* event//手对象里额外包括以下三个触摸列表 
* touches: //当前屏幕上所有手指的列表 
* targetTouches: //当前dom元素上手指的列表，尽量使用这个代替touches changedTouches: //涉及当前事件的手指的列表，尽量使用这个代替touches 这些列表里的每次触摸由touch对象组成，
touch对象里包含着触摸信息，
主要属性如下： 
 * clientX / clientY: //触摸点相对浏览器窗口的位置
 * pageX / pageY: //触摸点相对于页面的位置 
 * screenX / screenY: //触摸点相对于屏幕的位置 小案列：  
