# 选项卡应用


> 很多时候我们需要用到选项卡应用，比如手机京东的商品分类； js代码：

<!-- more -->

```javascript
window.onload=function(){
    var tabLi=document.querySelectorAll('.tab li');
    var tabDiv=document.querySelectorAll('.content Div');
    for (var i = 0; i < tabLi.length; i++) {
       tbLi[i].index=i;
       tabLi[i].onclick=function(){
       for (var i = 0; i < tabLi.length; i++) {
	   tabLi[i].className='';
           tabDiv[i].style.display='none';
	      }
       this.className='current';
       tabDiv[this.index].style.display='block';
			};
		};
	}
```

