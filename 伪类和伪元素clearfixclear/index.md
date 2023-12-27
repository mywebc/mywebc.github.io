# 伪类和伪元素、clearfix、clear


1. 伪类是给选择器添加效果；伪元素是给将特殊效果添加到选择器。 伪类的效果等同于伪造一个类，伪元素的效果等同于伪造一个元素 伪类：
 伪元素：
 在CSS3中规定：伪类用一个冒号来表示，伪元素用两个冒号来表示。 

2. 清除浮动有clearfix和clear，还有重构后的clearfix， 

* clear不用了就是在容器内在所有标签后再加一个空的标签用clear：both；清除浮动，这样在浮动很多的页面中就会空很多的空标签； （
* clearfix是在父容器的下方放一个空的div，用clear:both;来清除浮动 ` .clearfix::after { content: '.';//内容为.或空 display: block;//设为块级标签 clear: both;//清除浮动 visibility: hidden;//不可见 height: 0;//高度为0 font-size: 0;//字体大小为0 } ` （
* 上面两种都是利用clear上方不能有浮动元素的规则来清除浮动的；重构后的clearfix是使父容器成为BFC（Block Formatting Context），
BFC特性之一就是可以包含浮动元素，
能形成BFC的有以下四种情形： 
  1. float值不为none，可以是left，right或both 
  2. overflow为hidden或auto或scroll 
  3. display为inline-block或table-cell或table-caption 
  4. position为absolute或fixed 所以下面两种方法都可以：
（zoom：1是为了兼容ie6） ` .clearfix { zoom: 1; display: table; width: 100%; } ` `.floatfix{ *zoom:1; } .floatfix:after{ content:""; display:table; clear:both; }`
