# js中对数组的一些操作


对于angular，还是先放放，国庆期间看了点VUe.js,最近这两天在FCC（free code camp）刷题，以前学的知识又忘了，看到数组的几个方法怕记不住，还是写下来吧。 
1. 插入与删除
 * push：末尾插入；
 * pop:末尾删除； 
 * shift:开头删除； 
 * unshift:开头添加；

2. map 迭代数组，在回调函数中处理相关逻辑，并且返回新数组，不会改变原来数组 比如：
```js
var oldArray = [1,2,3,4,5]; 
var newArray = oldArray.map(function(val){ return val+3; }); 
```
3. reduce 迭代数组，作用将数组累加到一个值中，两个常用参数（总共4个），preValue和currValue; 比如： 
```js
var array = [4,5,6,7,8]; 
var singleVal = 0; 
singleVal = array.reduce(function(pre,curr){ return pre+curr; },0); 
```
4. filter（过滤器） 
```js
var oldArray = [1,2,3,4,5,6,7,8,9,10]; 
var newArray = oldArray.filter(function(val){ return val<6; }); 
```
5. sort 方法，你可以很容易的按字母顺序或数字顺序对数组中的元素进行排序。 比如：从小到大排列 
```js
var array = [1, 12, 21, 2]; 
array.sort(function(a, b) { return a - b; }); 
``` 
6. reverse 方法来翻转数组，无参数。 
比如： 
```js
 var array = [1,2,3,4,5,6,7]; 
 var newArray = []; 
 newArray = array.reverse(); 
 ```
 7. concat,将两个数组内容合并到一个数组中去。 比如：用concat 把 otherArray 拼接在 oldArray 的后面： 
 ```js
 newArray = oldArray.concat(otherArray); 
 ```
8. split将指定字符串分割成数组 split（“”）单个字符分割 split（“ ”）以空格分割 他还有第二个可选参数，规定返回多少个数组。 
9. join，将数组转化成字符串，并且可以指定连接符 
```js
 var joinMe = ["Split","me","into","an","array"]; 
 var joinedString = ''; 
 joinedString = joinMe.join(' '); 
 ```
