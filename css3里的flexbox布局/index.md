# CSS3里的flexbox布局


> 一般我们在页面中的布局都用浮动或是定位来解决，但是浮动的初衷是解决文字环绕图片的问题，但是现在CSS3中出现了flexbox布局，一个真正为布局而出现的解决方案；

 **一、如何成为弹性盒子（flexbox）?** 

 我们只要给父容器设置display:flex||inline-flex即可，这时候，父容器就是flex container（弹性容器）,里面的子元素就是flex item（弹性子元素）;还有一些其他名词我们也需要了解，如下图（图片资源来自于网络）：  在这个弹性容器中我们有两个轴main axis（主轴）从左到右，cross axis（侧轴）从上到下，axis就是轴的意思，来跟我念"阿克色死"，主轴开始的地方叫main start,结束的地方叫main end,侧轴同理也有cross start和cross end，在主轴中子元素所占据的空间叫main size，同理侧轴叫cross size。 

**二关于flex在父容器中应用的属性** 

 1. flex-direction：row（默认）||row-reverse||column||column-reverse;规定弹性子元素在主轴或侧轴的起始方向。 看如下代码，
```html
<div class="container"> 
  <div class="one">one<div>
  <div class="two">two<div> 
  <div class="three">three<div> 
<div>
``` 

效果图如下：
 然后我们给父容器添加四个属性，
 说明：
 第一个就是设display：flex后它的默认效果相当于flex-direction：row，

 第二个是row-reverse，第三个是column，第四个是column-reverse 

2. flex-wrap：nowrap||wrap||wrap-reverse;
这是个换行属性，规定如果一条轴线上放不下子元素，是否换行，换到上面还是下面。 现在我一行放四个，四个div的宽度已经超过了父容器的宽度，看看这个属性怎么用的如图：
  说明：第一个是nowrap不换行，第二个是wrap换行，第三个也是换行，区别是原来是第一行到第二行，现在是第二行到第一行，记住reverse就是反向的意思。 
3. flex-flow: || ;
这个属性可以将前面两个合并起来写，默认就是row nowrap 
4. justify-content: flex-start || flex-end || center || space-between || space-around;
规定了子元素在主轴的对齐方式 话不多说看图吧：说明：从第一张到到最后一张属性：flex-start（默认）、flex-end、center、space-between、space-around，第四个和第五个区别就是，第四个把多余的空间部分平均分配但不包括收尾项，第五个包括收尾项。 
5. align-items:flex-start||flex-end||center||baseline||stretch;
有主轴的对齐方式也有侧轴的对齐方式啦。 直接上图演示： 说明：前面三张图不用解释了吧，第四张呢css-tricks上是这样解释的baseline: items are aligned such as their baselines align;
翻译过来就是项目对齐正如他们的基线对齐，我想应该就是以他们里的文字为基准对齐，第五个在不设置高度的情况下(也是默认情况下)生效，strench即拉伸,子元素高度撑满父容器。 
6. align-content:flex-start||flex-end||center||space-between||space-around||stretch;
如果是多轴线，对所有元素规定对齐方式，是单轴线的话不起作用。 看图：说明：属性与图都是是一一对应的，这里的属性前面都讲过，看看图就明白了。

**三、关于flex在字元素中应用的属性** 

1. order：integer;
  规定子元素的排列顺序。所有元素默认都是0，就看谁是老大，谁就排在最后。 看！这里我把第一个元素order设为1，它就排到最后了，其他的默认都是0。 
  
2. algin-self
它老爸是algin-items，所以呢他们有相同的属性值，只是这个属性针对的是单个元素，还有呢他会多一个默认的auto属性值，表示继承老爸的属性，如果他没老爸，默认就是strench,这里就不演示了。 

3. flex-grow:number;
  定义子元素的放大比例。 这个属性所有的元素都是默认为0的，就是保持不变，如果有一个元素为1或者更大，他就会占满剩余空间，看下面两张图就知道了： 这里我把第五个div的flex-grow设为1,2,或1000它都是这样，只要其他的都是0; 再看下图： 这里我把所有的子元素flex-grow设置为2，第五个子元素单独设置为1 ，可以看到在第一行他们平均分配，因为他们都是2;在第二行值为2的是值为一的双倍，因为比例是12，所以看到第四个是第五个的双倍，这个比例如果是3/4,4/5,....越来越接近1的话，他们的长度也会越来越接近。这里注意负值无效，就是默认为0了。 
  
4. flex-shrink:number;
  有放大就有缩小，这个属性规定了子元素的缩小比例，默认为1。如果一个子元素的值为0，那么他的宽度就是固定的，如果他的值是1,2,3,..值越大，缩小比例越小。 

5. flex-basis:auto||length;
定义我们子元素在放大或缩小前的尺寸，可以是**px形式，定义好后，我们的放大或缩小将以这个为基准，这个没什么好说的。 

6. flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ];
这个属性可以把上面讲的三个属性合在一起写。 如果写flex：none;表示0 0 auto  如果写flex：auto;表示1 1 auto  如果只写一个值flex:1/1px;没有单位的表示grow，有单位的表示basis  如果写两个值flex:（1 1）（1 1px）;前者表示grow和shrink，后者表示grow和basis  如果写三个值就是grow shrink basis拉！ 好啦，整理结束！
