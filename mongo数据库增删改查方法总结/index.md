# mongo数据库增删改查方法总结


### 介绍
mongo数据库是nosql（非结构性数据库）的一种，存储格式不是传统的表、行、字段，它的数据库是由多个集合组成，每个集合包含很多文档，每个文档就是一个json。具体的看下图就知道了：  
所有的操作都是在命令行里操作的，当然网上也有很多可视化的工具，比如mongo VUE。

 mongo下载地址：[https://www.mongodb.com/download-center?jmp=nav#community](https://www.mongodb.com/download-center?jmp=nav#community) 

 mongoAPI文档：[https://docs.mongodb.org/manual/](https://docs.mongodb.org/manual/) 

 下载安装好后，添加mongo环境变量，在命令行里输入mongod --dbpath f:\\mongo,开启数据库，路径为f:\\mongo，

 另起命令行窗口，输入mongo连接数据库，接下来就可以进行以下操作了。 
 其它基础命令： 
 * show dbs:查看所有数据库 
 * db:查看当前数据库 
 * use student:切换到student数据库，如果没有则创建 
 * db.student.drop();删除student集合 
 * db.dropDatabase();删除当前所在数据库 
 * db.getCollectionNames();获取当前数据库下的所有文档 cls:清屏
### 插入
* db.student.insert({"name":"小明"}); 
* 可以在外面写好json文件，使用命令导入： 
mongoimport --db test --collection restaurants --drop --file primer-dataset.json 
1. -db test 想往哪个数据库里面导入 
2. --collection restaurants 想往哪个集合中导入 
3. --drop 是否把集合清空 
4. --file primer-dataset.json 哪个文件

### 查找
* db.restaurants.find() 
1. 精确匹配： db.student.find({"score.shuxue":70}); 
2. 多个条件： db.student.find({"score.shuxue":70 , "age":12}) 
3. 大于条件： db.student.find({"score.yuwen":{$gt:50}}); 
4. 或者。寻找所有年龄是9岁，或者11岁的学生 db.student.find({$or:\[{"age":9},{"age":11}\]}); 
5. 查找完毕之后，打点调用sort，表示升降排序。 db.restaurants.find().sort( { "borough": 1, "address.zipcode": 1 } )

### 修改
* 先要找到再修改，使用$set db.student.update({"name":"小明"},{$set:{"age":16}}); 
* 查找数学成绩是70，把年龄更改为33岁： db.student.update({"score.shuxue":70},{$set:{"age":33}}); 
* 更改所有匹配项目：添加multi: true： db.student.update({"sex":"男"},{$set:{"age":33}},{multi: true}); 
* 完整替换，不出现$set关键字了： db.student.update({"name":"小明"},{"name":"大明","age":16});

### 删除
* db.restaurants.remove( { "borough": "Manhattan" } ) 

 只删除一个：
* db.restaurants.remove( { "borough": "Queens" }, { justOne: true } )
