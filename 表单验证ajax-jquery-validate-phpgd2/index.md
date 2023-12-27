# 表单验证（ajax+jQuery Validate+PHPgd2）


> 一般在我们浏览器端负责输入数据格式的验证，在服务器端利用ajax技术查询验证输入数据是否重复 

<!-- more -->

## 一、浏览器端的验证我们可以用jQuery Validate这个插件 
1. 首先引入插件： 
``` javascript
<script src='libs/jquery-1.11.1.min.js'></script>/
//jquery插件 
<script src='libs/jquery.validate.min.js'></script>
//jQuery validate插件 
<script src='libs/messages_zh.min.js'></script>
//中文提示信息插件
```

2. 具体使用看如下例子： 
* js部分

``` js
 $(function(){ 
   //指定要验证的表单 
   $('#myform').validate({ 
     //这里是验证通过要书写回调函数 
     submitHandler:function(form){
        //提交表单方式之一 
        form.submit(); 
      } 
      //这里定义规则 
      rules:{ username:{ requried:true, minlength:2 } } 
      //这里写提示信息，如果不写，就会使用默认 
      messages:{ 
        username:{ 
          required:"请输入用户名！", minlength:"长度不少于2位！" } } 
      }) 
    }) 
```

3. 更多的其他的指令规则：规则 描述 
  * required:true 必须输入的字段。 
  * remote:"check.php" 使用 ajax 方法调用 check.php 验证输入值。 
  * email:true 必须输入正确格式的电子邮件。 
  * url:true 必须输入正确格式的网址。 
  *  date:true 必须输入正确格式的日期。日期校验 ie6 出错，慎用。
  * dateISO:true 必须输入正确格式的日期（ISO），例如：2009-06-23，1998/01/22。只验证格式，不验证有效性。 
  * number:true 必须输入合法的数字（负数，小数）。 
  * digits:true 必须输入整数。 
  * creditcard: 必须输入合法的信用卡号。 
  * equalTo:"#field" 输入值必须和 #field 相同。 
  * accept: 输入拥有合法后缀名的字符串（上传文件的后缀）。 
  * maxlength:5 输入长度最多是 5 的字符串（汉字算一个字符）。 
  * minlength:10 输入长度最小是 10 的字符串（汉字算一个字符）。 
  * rangelength:[5,10] 输入长度必须介于 5 和 10 之间的字符串（汉字算一个字符）。 
  * range:[5,10] 输入值必须介于 5 和 10 之间。 
  * max:5 输入值不能大于 5。 
  * min:10 输入值不能小于 10。 

4. 小提示：可以使用label.error自定义错误信息提示样式; 

## 二、关于验证码 要用到PHPgd库里面提供的一些api,使用前注意开启PHPgd2 具体示例： 
  ```js
    // 创建画布 
    $img=imagecreate(100, 35); 
    // 给画布分配背景颜色，mt_rand()就是返回随机整数 
    $bgcolor=imagecolorallocate($img, mt_rand(0,255), mt_rand(0,255), mt_rand(0,255)); 
    // 添加干扰线，用for循环，随机画三条 
    for ($i=0; $i < 3; $i++) { 
      // 为干扰线添加随机颜色 
      $linecolor=imagecolorallocate($img,mt_rand(0,255), mt_rand(0,255),mt_rand(0,255)); 
      // 开始画干扰线 
      imageline($img,mt_rand(0,100),mt_rand(0,35),mt_rand(0,100),mt_rand(0,35),$linecolor); } 
      // 添加干扰点,for循环，随机添加25个点 
      for ($i=0; $i <25 ; $i++) { 
        //为点添加随机颜色 
        $diancolor=imagecolorallocate($img,mt_rand(0,255), mt_rand(0,255),mt_rand(0,255)); 
        // 开始画点 
        imagesetpixel($img,mt_rand(0,100),mt_rand(0,35),$diancolor); 
        } 
      // 准备一个字符串 
      $rand_str='qwertyuiopasdfghjklzxcvbnm1234567890'; 
      // 准备一个空数组 
      $str_arr=array(); 
      // 循环四次返回4个随机字母或数字 
      for ($i=0; $i <4 ; $i++) { 
        // 随机取从0到字符串长度的下标 
        $pos=mt_rand(0,strlen($rand_str)-1); 
        // 一一放到数组里 
        $str_arr[]=$rand_str[$pos];
      } 
      // 开启sesson,把这四个数据放进session里 
      session_start(); $_SESSION['info']=$str_arr; 
      // 准备绘制，可以用imagestring(不支持中文)或者imagettftext(支持中文但需要下载字体) 
      //用imagesttftext绘制 
      $x_start=100/5; 
      // foreach用于遍历输出数组，不知道循环次数 
      foreach ($str_arr as $key) { $fontcolor = imagecolorallocate($img, mt_rand(0,255), mt_rand(0,255), mt_rand(0,255)); imagettftext($img, 20, mt_rand(-15,15), $x_start, 50/2, $fontcolor, "C:/Windows/Fonts/Verdana.TTF", $key); $x_start +=20;
      //遍历后单个字符沿X轴 
      +20 
      } 
      // 填充 
      imagefill($img,0,0,$imgcolor); ob_clean(); 
      // 输出图像格式 
      header('content-type:image/jpeg'); 
      // 输出图像 
      imagejpeg($img); 
      // 摧毁图像 
      imagedestroy($img); 
``` 


1. 我们在HTML中创建一个img,src属性指向这个PHP页面即可显示出来，如：src=data.php  点击图片刷新属性： 
2. 验证验证码是否正确，利用ajax传送到后端，后端如下判断： 
  ```php
    <?php header('content-type:text/html;charset= utf-8'); 
    // 接受验证码 
    $yzm=$_POST['num']; session_start(); 
    // 把数组里的数据都取出来 
    $data=implode('',$_SESSION['info']); 
    if($yzm==$data){ echo'1'; } ?> 
  ``` 
注意：我也不知道为什么，一开始老是验证不了，最后是需要再单独建一个PHP，再在里面写上验证代码就好了。
