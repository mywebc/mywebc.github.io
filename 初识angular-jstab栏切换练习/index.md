# 初识angular js，tab栏切换练习


> 看到了angular js， angular js是一款MVC前端框架，以数据和逻辑作为驱动核心，有很多特性：模块化，双向数据绑定，语义化标签等，与之类似的还有VUE，REACT等框架。 MVC是一种开发模式，由模型（Model）、视图（View）、控制器（Controller）3部分构成。 基本用法： 

<!-- more -->

1. 首先script标签引入angular js 框架； 
2. 其次定义模板，第一个参数为模板名称，第二个参数为一个空数组,表示依赖的其他模块； 
3. 然后定义控制器，第一个参数为控制器名称，第二个参数除最后一个是函数外，其他都是字符串，表明此控制器的依赖关系， 控制器里书写逻辑数据；
4. 最后绑定模板，绑定控制器； 具体示例代码如下： 
```javascript
 <!-- 绑定模块 -->
  <body ng-app='Tabs'> 
  <!-- 绑定控制器 --> 
  <div class="container" ng-controller='TabController'> 
  <ul> 
  <li ng-class="{current:type=='first'}" ng-click='switch("first")'>tab1</li> 
  <li ng-class="{current:type=='second'}" ng-click='switch("second")'>tab2</li>
   <li ng-class="{current:type=='third'}" ng-click='switch("third")'>tab3</li> 
   </ul> 
   <div ng-switch on='type'> 
   <div ng-switch-when='first'>这里是内容一哦</div> 
   <div ng-switch-when='second'>这里是内容二哦</div>
    <div ng-switch-when='third'>这里是内容三哦</div> 
    </div> 
    </div> 
    <!-- 引入angularjs 框架 --> 
    <script src="../libs/angular.min.js"></script> <script type="text/javascript"> 
    // 定义模板
     var Tabs = angular.module('Tabs',[]);
     // 定有控制器 Tabs.controller('TabController',['$scope',function($scope){ 
     	// 这个$scope就是空的对象，指的就是Model $scope.type='first'; $scope.switch = function(type){
     	 $scope.type=type; 
     	 } }]); 
     	 </script> 
     	 </body> 
```
* ng-bind是angular js中的指令，和我们HTML标签中src一样，只不过换个叫法，angular js中还有其他常用的指令，比如ng-class,ng-switc,ng-show等等，更多指令后续再来复习。 掌握好基础用法后，我们常见的简单的Tab栏切换，也可以用这个来做，具体demo如下。



