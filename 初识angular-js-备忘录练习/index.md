# 初识angular js ,备忘录练习


> 一个备忘录小练习，可以写入计划和删除计划。 进一步了解了一些指令的使用：
 
<!--more-->

1. ng-repeat='(key,item) in items': 用于循环输出，加在需要循环输出的元素上,items是控制器上变量名字，item是变量数组中单个元素的别名，key表示索引值，从0开始。 
2. ng-model='text' 用于向控制器传送数据，体现双向绑定数据特性，注意表单要加上ng-submit='add()'; 
3. ng-click='add()' 点击事件，在控制器上可以书写对应的函数; 
4. splice(index，0，items) 删除或插入数组，index表示从第几位删除或插入，0表示几个元素，items可选表示数组； 

