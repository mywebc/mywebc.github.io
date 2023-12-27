# 数组内元素随机排序的三种方法


> ## 一、利用数组方法sort

<!-- more -->
```js
// 数组的sort方法，如果a>b,则正序排，a
```
## 二、经典的洗牌算法
 ```js
 // 我们先定义一个随机函数,用于取到x-y区间的随机数 
 function getRandom(min, max) { 
   // +1是为了取到上限， 
   return Math.floor(Math.random() * (max - min + 1) + min) 
   } 
   function shuffle(arr) { 
     // for 循环，随机替换掉每一个数组元素 
     for(var i = 0; i < arr.length; i++) { 
       // 取一个随机数 
       var j = getRandom(0,arr.length) 
       var x = arr[j] 
       arr[i] = arr[j] 
       arr[j] = x 
      }
      return arr 
  }
 ```
## 三、push进新数组
```js
// 递归思想，每执行一次，取一个数组索引的随机数，如此反复，直到数组长度为一，结束循环 
function randomArr(arr, newArr) { 
  if(arr.length === 1) {
     newArr.push(arr[0]) 
     return newArr 
     } 
     // 取随机数 
     var random = Math.ceil(Math.random() * arr.length) - 1 
     // 随机push 
     newArr.push(arr[random])
      // 旧数组删除对应位置 
      arr[random].splice(random, 1) 
      // 再次调用 
      return randomArr(arr, newArr) 
  } 
```
