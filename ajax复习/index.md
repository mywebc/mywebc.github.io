# ajax复习


> ajax是一种在不刷新页面的情况下，局部更新数据的一种技术，XMLHTTPRequest对象是ajax的基础。 使用ajax有以下4个步骤： 

**（1）创建XMLHTTPRequest对象** 

所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。 

兼容性写法如下： 
``` javascript
var xmlhttp; 
if (window.XMLHttpRequest) {
// code for IE7+, Firefox, Chrome, Opera, Safari 
xmlhttp=new XMLHttpRequest(); 
} else {
// code for IE6, IE5 
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
 } 
 ```

**（2）向服务器发送请求** 

这里我们使用open()和send()方法 open(method，url,flag),open()方法中有三个参数： 

method：请求方式，有get和post两种； 

url:要请求页面的地址，相对地址或绝对地址 

flag：true是异步，false是同步 

注意点：
1. get和post的比较：get传输数据小且不安全，post传输数据大且安全； 
2. 使用post方式时，注意添加http请求头协议； send(string),send()中有一个参数，当使用GET方式请求时，里面没有参数，使用post方式时，里面写上传输的数据； 示例get请求： 

``` js
xmlhttp.open('get','123.php',true); xmlhttp.send(); 
```
示例post请求： 
``` javascript
xmlhttp.open('post','123.php','true'); 
//添加http请求头信息 
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
xmlhttp.send("name=lili&amp;age=10"); 
```
3. 服务器的响应 responseText 获得字符串形式的响应数据。 responseXML 获得 XML 形式的响应数据。 
4. onreadystatechange 事件 当请求被发送到服务器时，我们需要执行一些基于响应的任务。 每当 readyState 改变时，就会触发 onreadystatechange 事件。 readyState 属性存有 XMLHttpRequest 的状态信息。 

下面是 XMLHttpRequest 对象的三个重要的属性： onreadystatechange属性，每当readyState属性变化时就会调用该函数； 
readyState属性，它有四种状态变化： 
   * 0: 请求未初始化 
   * 1: 服务器连接已建立 
   * 2: 请求已接收 
   * 3: 请求处理中 
   * 4: 请求已完成，且响应已就绪 status属性 200: "OK" 404: 未找到页面 所以当 readyState 等于 4 且状态为 200 时，表示响应已就绪： 
示例： 
``` javascript
xmlhttp.onreadystatechange=function() { 
    if (xmlhttp.readyState==4 &amp;&amp; xmlhttp.status==200) { document.getElementById("myDiv").innerHTML=xmlhttp.responseText; 
    } 
} 
```

* Query中的ajax 在jQuery中我们有$.get()、$.post()、$.ajax()方法来向服务器发送数据 

1. $.get() $.get(url,data,success(),datatype) url:必填； data：可选，规定请求的同时发送的数据； success()：可选，请求成功时运行的函数； datatype：可选，服务器响应的数据类型，JQuery会只能判断； 
2. $.post()方法差不多 url:必填； data：可选，规定请求的同时发送的数据； success()：可选，请求成功时运行的函数； datatype：可选，服务器响应的数据类型，JQuery会只能判断； 
3. $.ajax()方法可以替换上面两种方法，定制自由度更高,以下是常用属性； 
* $.ajax({ url:'01.php',//请求地址 
* data:'name=fox&age=18',//发送的数据 type:'GET',//请求的方式 
* success:function (argument) {},// 请求成功执行的方法 
* beforeSend:function (argument) {},// 在发送请求之前调用,可以做一些验证之类的处理 
* error:function (argument) {console.log(argument);},//请求失败调用 })
