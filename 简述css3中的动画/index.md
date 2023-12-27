# 简述CSS3中的动画


> 动画是CSS3中具有颠覆性的特征之一，可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。 

<!-- more -->

1. 在CSS3中的设置动画步骤： 
  a、通过@keyframes指定动画序列； 
  b、通过百分比将动画序列分割成多个节点； 
  c、在各节点中分别定义各属性 
  d、通过animation将动画应用于相应元素； 示例： 
  ```javascript
   //定义规则 
   @keyframes move{
     //move为规则名称 
     from{
        background-color:yellow; 
        } 
      to {
         background-color:red; 
         } } 
      //也可以以百分比的形式，设置多个节点 
      @keyframes move{ 
        0%{ 
          background-color:yellow; 
      } 50% {
         background-color:red; 
      } 75% { 
          background-color:green; 
          } }
      //最后在animation属性里应用 
      .div {
         animation:move 1s; 
        }
  ```
2. animation 中的属性简述 
* animation-name设置动画序列名称； 
* animation-duration动画持续时间； 
* animation-delay动画延时时间； 
* animation-timing-function动画执行速度，linear(平均)、ease（慢快慢）、ease-in（慢快）、ease-out（快慢）、cubic-bezier(自定义速度)； 
* animation-play-state动画播放状态，running、paused; 
* animation-direction动画逆播，normal（正常，动画结束回到第一帧），reverse（与normal相反），alternate（动画完成，在倒放逆播），alternate-reverse（与alternate相反）；
* animation-fill-mode动画执行完毕后状态，forwards（保持最后一帧的状态）、backwards（回到第一帧）；
* animation-iteration-count动画执行次数，可以是1、2、3...inifinate表示无数次；
* steps(60) 这里表示动画分成60步完成,一帧一帧的进行，这是第一个参数，第二个参数（可选）为start或end，默认情况下为end; 关于start和end 的区别：他们都规定了动画时间变化点，start是在两帧跳换前，end是在两帧跳换后； 

比如：分两步，0%-100%，盒子颜色变化从红色变到绿色 如果设置start，他在0%跳换前，动画就执行了，所以我们刷新页面一进去就看到绿色，设置end的话，我们会一直看到红色，0%前动画不会执行，跳到100%后动画结束，这是绿色状态不可见的，还会变成红色。 

总之，如果3s的动画执行时间，start和end的区别就是，到底是在3s前就执行，还是在3s后执行的区别。 参数值的顺序： 关于几个值，除了名字，动画时间，延时有严格顺序要求其它随意 最后2个小例子熟悉一下： CSS3设置帧动画达到钟表效果： 
