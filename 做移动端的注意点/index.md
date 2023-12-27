# 做移动端的注意点

> 做移动端的注意点

<!-- more -->

1. 通栏中，不管多大的分辨率，左边和右边的距离都是不变的，我们可以将其input宽度设为100%，再在其父盒子中给出padding,父盒子不要给宽度； 
2. 很多时候样式都会被覆盖，在属性值后加上!important或者在前面加上其父级；
3. sublime中alt+F3全局修改同一单词； 
4. 在浏览器中方向是从左往右，从上往下； 
5. 为了代码的通用性，动态获取dom元素，不能出现magicNumber; 
6. 用js设置自定义属性比如dataset\['index'\]，读取是this.dataset.index，但是用读取的值进行算数运算时不准确; 
7. 在选择器中，.header div指父盒子header下所有div；.header>div指父盒子header的所有下一级div;
