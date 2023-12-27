# 数组去重的三种方法


> ## 一、利用indexOf方法剔除

<!-- more -->

```js
var newArr = [] 
function removeArr (arr) {
   for (var i = 0;i < arr.length; i++){ 
     // 如果等于-1的话，push进newArr 
     if(newArr.indexOf(arr[i] === -1)) { 
       newArr.push(arr[i]) 
       } 
      } 
      return newArr 
  } 
```
## 二、利用sort方法相邻比较

```js
function removeArr(arr) {
   // 先排序，这样相同的值都是相邻的 
   arr.sort() 
   // 先把第一个值放进新数组 
   var newArr = [arr[0]] 
   for(var i = 1;i < arr.length;i++) { 
     // 如果旧数组的值与新数组的值不一样就Push i
     f(arr[1] !== newArr[newArr.length - 1]) { 
       newArr.push(arr[i]) 
       } 
      } 
    return newArr 
  } 
```

## 三、利用对象属性名是否重复
