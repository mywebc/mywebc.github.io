# 原生js封装ajax


> 面试大概率会考到

<!--more-->

```javascript
// 你传入的应该是一个对象 
function ajax(obj) {
 // 先判断是否是对象 
 if(obj instanceof Object){ 
 	// 设置一些默认值 
 	obj.type = obj.type || 'get' 
 	obj.dataType = obj.dataType || 'json' 
 	obj.async = obj.async || 'true' 
 	// 将数据序列化 
 	params = encodeUrl(obj.data) 
 	// 获取xhr对象 
 	var xhr = getXhr() 
	xhr.responseType=obj.dataType;
 	 if(obj.type === 'get') {
 	  obj.url = obj.url + '?' +params xhr.open('get', obj.url, obj.async) xhr.send() 
 	} 
 	  if(obj.type === 'post') {
 	   xhr.open('post', obj.url, obj.async) xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded') xhr.send(params) 
		} 
 	   xhr.onreadystatechange = function(){ 
 	   	if(xhr.readystate === 4 && xhr.status === 200) { 
 	   		obj.success(xhr.responseText) }else { 
 	   			obj.error(xhr.status) } 
			} 
		}else { 
			// 否则返回什么都不做 
			return obj
			} 
		} 
		// 获取xhr对象 
function getXhr(){
	if(window.XMLHttpRequest){
			return new XMLHttpRequest() 
	}else { 
			return new ActiveXObject('Microsoft', XMLHTTP)
		} 
	}
	
// 将URL序列化，encodeURLComponent编码 
function encodeUrl(params){ 
// 如果是对象的话 
if(params instanceof Object){ 
	var arr = \[\] 
	for(var item in params){ 
		arr.push(encodeURIComponent(item) + '=' + encodeURIComponent(params\[item\]))
	} 
		// 再加一个随机数，避免浏览器缓存 
		arr.push('randNum=' + Math.random()) 
		return arr.join('&') }else { 
		return params 
	} 
} 
	ajax({ 
		url:'http://www.baidu.com', 
		data:'get', 
		dataType:'json', 
		data:{ name:'haha', age: 12 }, 
		success:function(data){ 
			console.log('成功')
			}, 
		error:function(data) { 
			console.log(data)
			} 
		}) 
```
