# PC端JD首页


在做的过程中，遇到自己忘了的，或者要注意的，都记下来了； 
1. 精灵图的定位的中心坐标是左上角； 
2. 去掉点击input后的边框：input：focus{outline：none}； 
3. 输入框和搜索框有间隙不对齐：可以给vertical-algin:middle,margin给负值；
4. 小三角可以用css3中的transparent来做，比如下拉： .arrow{ width:0; height:0; border-left:20px solid transparent; border-right:20px solid transparent; border-top:20px solid #0066cc; } 也可以在父盒子里放一个菱形，让菱形偏移，父盒子截断； 
5. 顶栏中每个标签都有个小竖线，要用专门的li来放； 
6. z-index注意只对定位元素有效,正负数设置层叠关系； 
7. a标签一内嵌全乱了;
8. 移动transfom,过渡效果transition;
9. scrollTop兼容性写法： var scrollTop = document.documentElement.scrollTop || window.pageYOffset || document.body.scrollTop; 
10. 计时器效果 小时 var h=Math.floor(totalTime/3600); 分钟 var m=Math.floor(totalTime%3600/60); 秒 var s=totalTime%60; 在放li中时因为是两位，注意一个math.floor(h/10)，一个h%10； 
11. 网站的小图标在网站的后面加favicon.ico即可获得， link rel="shortcut icon" href=" favicon.ico"的形式添加 
12. 梳理一下轮播图的思路： 
* 如果有三张图，因为用到过渡效果，复制最后一个放在第一个前，第一个放在最后一个图后； 
* 自动换图版：用transform移动一个图片宽度长度，用transition添加过渡效果，到第四张时，在过渡结束事件中将索引值改为1，并且为索引值添加对应类； 
* 点击下方索引跳动版：添加onmouseover事件，为下标绑定一个自定义索引值用this.index形式，然后将这个新索引值给原来的图片的索引值； 
* 左右点击版：右击index++，加上transform移动，transition过渡，左击一样；（在过渡结束事件中添加index<1的情况） 鼠标在图片上或按钮上都要清除计时器，离开在启动计时器。 
