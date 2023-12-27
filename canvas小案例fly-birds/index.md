# canvas小案例：fly birds


> 我们可以看到这个游戏由背景蓝天，土地，小鸟，管道四部分组成，在开始游戏我们必须保证全部加载完所有图片资源，这里书写一个函数传进所有图片资源，再通过回调函数的方式返回加载好的图片资源 


```javascript
// 思路： 
// （1）遍历imgUrl,统计imgUrl次数，创建img对象 
// （2）调用img对象onload方法，所有加载完成后给fn 
function loadedImg(imgUrl,fn){ 
	// 用来保存图片资源 
	var imgObj={}; 
	var tempImg,loaded=0,imgLenght=0; 
	for(var key in imgUrl){ 
	// 每循环一次加一下，统计次数 
	imgLenght++; 
	// 获取img对象 tempImg=new Image(); 
	// 把所有图片资源依次循环赋值temImg.src 
	tempImg.src=imgUrl\[key\]; 
	// 然后再把这张图片依次存入imgObj imgObj\[key\]=tempImg; 
	// 注册所有图片加载事件 tempImg.onload=function(){ 
		// 统计加载的次数 
		loaded++;
			// 如果加载的次数与之前循环的次数相同的话，说明所有图片加载完毕 
			if(loaded=imgLenght){ 
			// 把加载的资源给回调函数 
			fn(imgObj); 
			} 
		}; 
	} 	
} 
```
1. 绘制背景 
和轮播图差不多，准备两张相同的背景图，在计时器里调用上下文对象的drawImg（）方法不断的向左偏移重绘即可，其构造函数如下 
```javascript
// 书写绘制背景函数 
function Sky(ctx,img,speed){ 
	this.ctx=ctx; 
	this.img=img; 
	this.width=this.img.width; 
	this.height=this.img.height; 
	Sky.len++; 
	this.x=this.width * (Sky.len-1); 
	this.y=0; 
	this.speed=speed||2; 
	} Sky.len=0; 
	// 给原型增加方法 Sky.prototype={
	 // 重新指向构造函数 constructor: Sky,
	 // 绘制背景 draw:function(){ 
	 	this.ctx.drawImage(this.img,this.x,this.y);
	 	 }, 
	 	 update:function(){ 
	 	 	// 让它往右走 this.x-=this.speed; 
		if(this.x<-this.width){ 
			this.x += this.width * Sky.len;
				}
				} 
				} 
				//我们再在loadImg函数中调用即可 
				// 先把图片调用出来 
				loadedImg({ beiJing:'./images/sky.png', bird:'./images/bird.png', land:'./images/land.png', pipeDown:'./images/pipeDown.png', pipeUp:'./images/pipeUp.png', 
				}, function(imgObj){ 
				// 实例化天空 var sky=new Sky(vas,imgObj.beiJing,2); 
				var sky2=new Sky(vas,imgObj.beiJing,2);
				// 让canvas的宽高与背景图自适应 
				ctx.width=imgObj.beiJing.width; 
				ctx.height=imgObj.beiJing.height; 
				// 让背景动起来 setInterval(function(){ 
					//第一张图片 
				sky.update(); 
				sky.draw(); 
				//第二张图片 
				sky2.update(); 
				sky2.draw(); },50) 
				})

 ```
2. 绘制大地同理 
3. 绘制小鸟同理，需要额外为画布添加一个点击事件，点击时让小鸟的this.y(y轴坐标)往上移动。 
4. 绘制管道的思路：每一组的管道，上管道和下管道的长短随机生成，但是中间的间距是一定的，我们将中间的距离固定，上管道的长为随机生成，下管道的长即画布高减去大地高减去中间间距即可也是随机的，让它移动起来方法也是和前面一样的。  
