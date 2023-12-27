# canvas画图API整理


> canvas 是 HTML5 提供的一个用于展示绘图效果的标签. 
canvas 原意画布, 帆布. 在 HTML 页面中用于展示绘图效果. 基本语法： 
1. 使用 canvas 标签, 即可在页面中开辟一格区域. 可以设置其 width 和 height 设置该区域的尺寸. 
2. 默认 canvas 的宽高为 300 和 150. 
3. 不要使用 CSS 的方式设置宽高, 应该使用 HTML 属性. 
4. 如果浏览器不支持 canvas 标签, 那么就会将其解释为 div 标签. 因此常常在 canvas 中嵌入文本, 以提示用户浏览器的能力. 
5. canvas 的兼容性非常强, 只要支持该标签的, 基本功能都一样, 因此不用考虑兼容性问题. 
6. canvas 本身不能绘图. 是使用 JavaScript 来完成绘图. canvas 对象提供了各种绘图用的 api. 
基本使用方法： 
1. 获得 canvas 对象. 
2. 调用 getContext 方法, 提供字符串参数 '2d'. 
3. 该方法返回 CanvasRenderingContext2D 类型的对象. 该对象提供基本的绘图命令. 
4. 使用 CanvasRenderingContext2D 对象提供的方法进行绘图. 
5. 基本绘图命令 
	* 设置开始绘图的位置: context.moveTo( x, y ). 
	* 设置直线到的位置: context.lineTo( x, y ). 
   * 描边绘制: context.stroke(). 
	* 填充绘制: context.fill(). 
	* 闭合路径: context.closePath(). 

<!--more-->
示例： 
```javascript
var canvas = document.createElement( 'canvas' ); 
canvas.width = 500; 
canvas.height = 400; 
canvas.style.border = '1px dashed red'; 
document.body.appendChild( canvas ); 
// 获得 CanvasRenderingContext2D 
对象 var context = canvas.getContext( '2d' ); 
// 设置 起点 context.moveTo( 0, 0 ); 
// 绘制直线 context.lineTo( 500, 400 ); 
// 设置 起点 context.moveTo( 0, 400 ); 
// 绘制直线 context.lineTo( 500, 0 ); 
// 描边显示效果 context.stroke(); 
```

其他常用API： 
**（一）绘制线型** 
 * canvas.lineWidth=number //设置线宽
 * canvas.lineCap='butt'( 默认 ), 'round', 'square' //设置线末端的样式 
 * canvas.lineJoin='round'（圆角）, 'bevel'（平切）, 'miter'(默认)（直角） //设置线末端的连接方式 
 * canvas.lineDashOffset=number //虚线起始偏移量 
 * canvas.setLineDash（） //设置虚线偏移量，数组形式，可以传多个参数 
 * canvas.getLineDash（） //获取虚线偏移量，以数组形式返回 
 * canvas.strokeStyle=value //设置描边的颜色 
 * canvas.fillStyle=value //设置填充的颜色 这里注意：要先填充，再描边
 **（二）绘制形状** 
 1. 绘制矩形：
* canvas.strokeRect(x,y,width,height) //x,y表示矩形左上角坐标，后面两个参数表示宽高，此方法只是画出了路劲，还需要描边。 
* canvas.fillRect(x,y,width,height) //参数与上面相同，这个是填充矩形，默认填充颜色为黑色。 
* canvas.clearRect( x, y, width, height ) //参数与上面相同，清除目标矩形区域内的内容。 
2. 绘制圆弧： 
canvas.arc( x, y, radius. startAngle. endAngle, anticlockwise ) //x,y表示圆心，radius表示半斤，后面两个表示起始角度，最后一个表示方向默认为正，传入true为逆时针。 
 注意：开始绘制圆弧前，如果设置了moveTo，就会形成圆弧。 
3. 绘制文本： 
* canvas.fillText( text, x, y,maxWidth\]) //填充文本，text为文本信息，x,y表示绘制的起点坐标，maxWidth表示限制文本最大宽度，一般不设他会自动设置。 
* canvas.strokeText( text, x, y,maxWidth\]) //描边文本，参数值与上面相同。 canvas.measureText() //获取文本的一些信息，返回TextMetrics对象.这个对象有很多属性，常用width等。 
 * canvas.font（） //设置字体的一些信息，语法与CSS相同，其顺序可以是: style | variant | weight | size/line-height | family. 
 * canvas.textAlign（）//设置文字对齐方式， start( 默认 ), end, left, right, center. 
 * canvas.textBaseline（） //设置字体垂直对齐方式，可以取值： top, middle, bottom, hanging, alphabetic（字母基线）, ideographic（表意对齐） 

4. 绘制图形： 
* canvas.drawImage( img, dx, dy ) //img是图像对象，可以是img ，video，或者是另一个canvas，x,y为放置的位置。 
注意这里的图片对象必须是已经加载完的，可以把它放到onload事件里。 
* canvas.drawImage(img, dx, dy, dWidth, dHeight) //与上面不同的是，这里多了两个参数指定图像宽高，这样是可以压缩图形的。 
* canvas.drawImage(img, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight) //这里面的意思就是把前面指定图像中的位置，剪切下来放到canvas指定区域中去。 

 **（三）变换** 
 * canvas.scale(x,y) a)参数 x 控制水平缩放倍率. 传参 1 表示不作缩放, 传入大于 1 的数字表示扩大. B)参数 y 控制垂直缩放倍率. 传参 1 表示不作缩放, 传入大于 1 的数字表示扩大. 
 * canvas.translate(x,y) a)x 表示水平移动, 正数向右, 负数向左. b)y 表示垂直移动, 正数向下, 负数向上. canvas.rotate(radian) 
 1. 正数表示顺时针旋转, 负数表示逆时针旋转. canvas.transform(a,b,c,d,e,f) a)其中 a 有时又标记为 m11. 它表示水平缩放. 
 2. 其中 b 有时又标记为 m12. 它表示水平倾斜. 
 3. 其中 c 有时又标记为 m21. 它表示垂直倾斜. 
 4. 其中 d 有时又标记为 m22. 它表示垂直缩放. e)其中 e 有时又标记为 dx. 它表示水平移动. f)其中 f 有时又标记为 dy. 它表示垂直移动. 
 * canvas.setTransForm(a,b,c,d,e,f) a)他会重置transform属性，只会影响后来的变换，会以相同参数运行transform（）； 

 **（四）保存和回滚**
* canvas.save()//保存状态 
* canvas.restore()//回滚上一次的状态 这里注意：如果 save 两次, 就需要 restore 两次, 才可以恢复到最先的状态. 绘制好后的canvas保存 
1. 直接右击另存为 
2. 或者使用 js 代码将其保存为 base64 编码的字符串Canvas.ToDataURL( type, encoderOptions )type 表示输出类型. 
例如: image/png 或 image/jpeg 等encoderOptions 表示图片输出质量, 其取值在 0 到 1 之间. 
如果是 1, 表示无损压缩, 必须使用 image/jpeg 或 image/webp 才起作用

**（五）渐变和图案** 
* canvas.createLinearGradient( x0, y0, x1, y1 ) 
  1. 该方法返回一个 CanvasGradient 对象. 用于描述渐变的方式. 
  2. 该方法有两个参数, 用于表示线型渐变的方向与位置. 
  3. 使用的时候, 首先创建一个 CanvasGradient 对象, 然后利用 addColorStop 方法添加颜色区间. 
  4. 方法语法: CanvasGradient.addColorStop( rate, color ). 
  5. 该方法用于设置在某个比例位置的颜色是什么. rate 的取值是 0 到 1 之间. 
  6. 可以添加多个渐变点. 
  7. 然后将该对象赋值给 *Style 属性即可. 
  示例： 
  ```javascript
  var canvasGradient = ctx.createLinearGradient( 0, 25, 200, 25 );
   canvasGradient.addColorStop( 0, 'blue' ); 
   canvasGradient.addColorStop( 1, 'red' ); 
   ctx.fillStyle = canvasGradient; 
   ctx.fillRect( 0, 100, 200, 50 ); 
   ```
   * canvas.createRadialGradient( x0, y0, r0, x1, y1, r1 ) 
   1. 该方法实现放射渐变, 渐变的是在两个圆之间. 一般会使用两个内含关系的圆. 
   2. 前三个参数分别表示其中一个圆的圆心的坐标, 以及半径. 
   3. 后三个参数分别表示另一个圆的圆心的坐标, 以及半径. 
   4. 绘制渐变效果用法与线性渐变一样. 
   ```javascript
   var x = cas.width / 2, 
   y = cas.height / 2, 
   r = 100; 
   var g = ctx.createRadialGradient( x + r * 2 / 3, y - r * 2 / 3, 0, x + r / 3, y - r / 3, r * 4 / 3 ); 
   g.addColorStop( 0, '#fff' );
    g.addColorStop( 1, '#f00' ); 
    ctx.fillStyle = g; 
    ctx.arc( x, y, r, 0, 2 * Math.PI );
     ctx.fill(); 
  ```
  * canvas.createPattern( img, repetition ) 
  1. 该方法表示使用图片来填充的设置方法. 需要两个参数, 一个是图片, 一个是重复的方式. 
  2. 图片允许是 img 标签, 图片, canvas 等对象 
  3. 可选择的重复方式与 CSS 一致. 有: repeat, repeat-x, repeat-y, no-repeat. 
  4. 如果是 空或"", 但不是 undefined, 默认就是 repeat. 
  
 **（六）阴影** 
 * 在 Canvas 中还可以给绘制的内容设置阴影. 但是一般不这么用, 因为性能不高. 相关属性: 
 1. canvas.shadowBlur 属性表示模糊程度. 
 2. canvas.shadowColor 属性表示模糊颜色. 
 3. canvas.shadowOffsetX 属性表示模糊位置 x 坐标偏移. 
 4. canvas.shadowOffsetY 属性表示模糊位置 y 坐标偏移.
