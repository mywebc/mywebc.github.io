# angular  js 获取天气预报


> 首先了解一下跨域：跨域就是从一个网站去请求另一个网站中的内容。 
要想实现跨域，现在主流的做法是用jsonp。 

<!-- more -->

## 一、jsonp的原理 我们发现HTML标签是可以请求外网的内容的，
* 比如img的src设置为外网的图片地址，是可以读取到的，那么我们可以新建一个script标签，设置它的src属性就是外网的地址，那么我们就能读取到外网的内容，jsonp会用到ajax方法，但是其实它跟ajax是没有关系的，jsonp是get请求。 
## 二、去百度拿天气数据 百度车联网API，
* 里面百度给我们提供了一个url: http://api.map.baidu.com/telematics/v3/weather?location=北京&output=json&ak=E4805d16520de693a3fe707cdc962045 其中ak指的是开发者密钥，这个我们需要注册一下百度账号，再点击获取密钥就可以了。  
## 三、在angular js 中有很多的内建服务，比如$http、$log、$location等等，
* 在我们需要用他们的时候，就要在控制器内传入他们，如下： 
```js
 weather.controller('WeatherController',['$scope','$http','$log',function($scope,$http,$log){ } 
 ```
 * 这里传入三个参数，$scope,$http,$log，告诉angular js我们需要他们，angularjs 
就会以参数的形式传进来供我们使用，这里的$http就是供我们向服务端发送异步请求的，而$log里提供了一系列的打印方法。 
完整代码如下： 
```html
<!DOCTYPE html> 
<html lang="en"> <head> <meta charset="UTF-8"> 
<title>weather</title> 
</head> 
<body ng-app='weather'> 
<div ng-controller='WeatherController'> 
<button ng-click='get()'>获取南京的天气</button> <table> 
<!-- 视图 --> 
<tr ng-repeat="item in weatherData"> 
<td>{ {item.date}}</td> 
<td><img ng-src="{ {item.dayPictureUrl}}" alt=""></td> 
<td><img ng-src="{ {item.nightPictureUrl}}" alt=""></td> 
<td>{ {item.temperature}}</td> <td>{ {item.weather}}</td> 
<td>{ {item.wind}}</td> </tr> </table> </div> 
<script src='libs/angular.min.js'></script> <script> 
// 定义模板 
var weather=angular.module('weather',[]); 
// 定义控制器 
weather.controller('WeatherController',['$scope','$http','$log',function($scope,$http,$log){ $scope.get=function(){ $http({ url:'http://api.map.baidu.com/telematics/v3/weather', method:'jsonp', 
// params会自动的帮我们把数据拼接到URL上 
params:{ location:'南京', output:'json', ak:'0A5bc3c4fb543c8f9bc54b77bc155724', 
//JSON_CALLBACK只是一个占位符，必须写。 
callback:'JSON_CALLBACK' 
} 
}) 
.success(function(data){ $log.log(data); $scope.weatherData=data.results[0].weather_data; }); } }]); 
</script> 
</body> 
</html> 
``` 
