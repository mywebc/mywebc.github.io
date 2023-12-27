# 关于cookie和session的区别以及在PHP中的用法

>  自学了5个月，我最近才听说cookie和session....之前做验证码接触了session,现在整理一下： 

<!-- more -->

## 二者定义： 
cookie：是服务器发送到客户端的一串文本数据，由客户端保存，每次请求时都会发送cookie，用于保持会话期间持久数据传递； session：session在服务器端，是通过cookie实现的，在发给浏览器的cookie中有唯一标识SESSONID,用于记录不同的用户的状态； 

## 整个过程是这样的：
客户端向服务器发送请求后，服务器会在响应头中返回cookie信息，其中包含唯一标识SESSIOND,客户端收到后就会存储起来，之后客户端的每次请求都会包含cookie信息，session会找到cookie中的sessionid，并且根据它找到对应的用户，及其存储在服务器的数据。 
## 二者区别： 
1. cookie存储在浏览器中，session存储在服务器中； 
2. cookie并不安全，别人可以篡改你的cookie信息,单个cookie存储数据不超过4k,一个浏览器最多存储20-50个cookie； 
3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。 注意：浏览器是可以禁用cookie的，与此同时session也会失效，这是我们还有两种方法 
* URL重写，把SESSIONID加到URL后，即'sid=XXX'的形式； 
* 表单隐藏：在HTML加一个隐藏表单如下： 

```html
<form name="testform" action="/xxx"> 
  <input type="hidden" name="jsessionid" value="ByOK3vjFD75aPnrF7C2HmdnV6QZcEbzWoWiBYEnLerjQ99zWpBng!-145788764"> <input type="text"> 
</form> 
```

 ## 在PHP中cookie用法 
 setcookie（名称，值，失效时间，域，路径，安全标志） 第一个和第二个参数是必填，其他都是可选 一般我们都这样设置几个参数 
```php 
setcookie('name','lili',time()+3600)
//创建或更新cookie 
setcookie（'name','',time()-3600）
//删除cookie，值设置为空，时间为负数 
setcookie（'name',null）
//这样删除也可以 //用$_COOKIE['name']读取，在读取时，先要判断是否为空值 
if(isset($_COOKIE['name'])){ $Name=$_COOKIE['name']; } 
//或者这样 
If(!empty($_COOKIE['name'])){ $Name=$_COOKIE['name']; }
```
 ## 在PHP中session用法 
 在使用session前我们都要初始化session_start()（他会创建一个唯一SESSIONID）,在这之前不能有输出语句。
 ```php 
 $_SESSON['name']='lili'
 //赋值 
 $Name=$_SESSION['name']
 //读取值 
 session_unset['name']
 //删除值（逐个） 
 $_SESSION=array();
 //整个删除 
 session_destroy();
 //最后整个摧毁
 session session_is_registered();
 // 检查变量是否被登记为会话变量,如果是返回TRUE 
 session_name();
 //设置或获取当前session的名称 
 session_set_cookie_params():
 //设置session的生存期，必须在session_start()之前调用; 
 session_save_path() ;
 //设置session保存路径，必须在session_start()之前调用; 
 ```

