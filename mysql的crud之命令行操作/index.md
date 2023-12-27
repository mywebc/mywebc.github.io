# MYSQL的CRUD操作之命令行

> 数据库分为关系型数据库和非关系型数据库，作为前端，对于mongodb再熟悉不过了，mongodb就是非关系型数据库，除此之外还有常见的还有redis，这种数据库的特点就是以Key,value的形式存储的，关系型数据库就很多了，mysql,oracle,db2等，本片文章主要记录我在学习MySQL关于CRUD的命令行操作。

<!-- more -->
## 登录数据库
* -u为用户名， -p为密码，我都设为root；
```
// 注意-p后面不要有空格
mysql -uroot -proot
```
## 创建数据库
* create database 数据库的名字;
* create database 数据库的名字 character set 字符集编码; 
* create database 数据库的名字 character set 字符集编码 collate 比较规则;
**注意要加分号**
```
create database student;
create database student character set utf8;
create database student character set utf8 collate utf8_general_ci;
```
## 查看数据库
* 默认的数据库不要动它
```
show databases;
show create database student; // 查看student数据库的定义语句
```
## 修改数据库
* 使用alter
```
alter database student character set gbk;
```
## 删除数据库
* 使用drop
```
drop database student;
```
## 其他数据库操作
```
use student; // 切换到student数据库
select database(); // 查看当前数据库
```
## 表的创建
* create table 表名(
    列名 列的类型 约束,
);
```
 create table employee (id int primary key auto_increment,
    name varchar(20) not null,
    birthday date,
    job varchar(30),
    salary double,
 );
```
## 查看表
```
show tables; // 查看数据库内的所有表
show create table employee; // 查看employee的建表语句
desc employee; // 查看employee的表结构
```
## 修改表
* 添加列（add），修改列（modify）,修改列名(cahnge)，删除列(drop), 修改表明（rename）,修改表的字符集（character set）
```
// 添加
alter table student add address int not null;
// 修改
alter table student modify address varchar(2);
// 修改列名
alter table student change address gender varchar(2);
// 删除
alter table student drop address;
// 改表名
rename table student to student2;
// 修改表的字符集
alter table student character set gbk;
```
## 删除表
```
drop table student;
```
## 表数据插入
* insert into 表名(列名1,列名2)values(值1,值2);
* insert into 表名 values(值1,值2); // 上面的简单写法
* insert into 表名values(值1,值2),(值1,值2),(值1,值2);// 批量插入

## 表数据删除
* delete from 表名 [where 条件]
* truncate table 表名: 先删除表,再重建表
**delete与truncate的区别**
1. delete是将数据一条一条的删除，在数据量比较少的情况下效率高；
2. truncate是先删除表,再重建表，由于更改了表结构，数据量大的情况下推荐；

## 表数据更新
* update 表名 set 列名=值, 列名=值 [where 条件]

## 表数据查询
通用格式: 
* select [distinct（去重）] [*] [列名1,列名2] from 表名 where 条件 group by ..having 条件过滤 order by 排序

**如果插入数据乱码，需要去mysql中的配置文件中my.ini57行将编码改为gbk**
