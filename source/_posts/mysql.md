---
title: mysql  
date: 2018-08-13 14:38:35
tags: 
- python
- mysql 
copyright: true
categories: python
---



<blockquote class="blockquote-center">相信就是强大。怀疑只会抑制能力，而信仰却是力量。</blockquote>

<!-- more -->

> mysql 平常记录

### 基本命令

#### 启动服务
net start MySQL

#### 打开
mysql -u root(用户名) -p

#### 查看当前mysql的版本
select version();


#### 显示当前的时间
select now();


#### 远程连接
mysql -h ip(地址) -u (用户名) -p 



### 数控操作

#### 创建数据库
create database mysqlA(数据库名字) [charset=utf8] ;


#### 删除数据库 
drop database mysqlA(数据库名字);  


#### 查看所有的数据库
show databases ;

#### 使用哪个数据库
use mySqlA(数据库的名字); 

#### 查看当前使用是哪个数据库
select database() ;



### 数据库表操作
#### 查看当前使用的数据库中有哪些表
show tables; 


#### 删除数据库中某张表
drop  table  car(表名) ；

#### 创建表格

- auto_increment 自增的
- primary key 主键
- default 默认
- not null 不为空 
```Java 
create table student(
    id int auto_increment primary key not null,
	name varchar(10) not null ,
	gender bit default 1 not null,
	address varchar(20) not null,
	isDelete bit default 0 not null 
   );
```   



#### 查看表的格式
`desc student(表名);`



#### 查看创建表的时候用到的语句
`show  create table student(表名) ; `


#### 重命名表名
`rename table student(旧表) to  student_v(新表名)`


#### 修改表的结构  add|change|drop 
`alter table cars(表名) add  isDelete bit default 0;`


### 数据操作

#### 增
- 全列插入
```Java
insert into student values(0,"tom",1,"北京石景山","1")
```   
- 缺省插入
```Java
  insert into student (name,gender,address,isDelete) values("joy",0,"河北衡水","0")
```
- 多行插入
 ```Java
insert into student values(0,"鸣人",1,"北京石景山","1"
   ),(0,"卡卡西",0,"北京爱","1")
 ```
 
#### 删
- 删除表中的所有数据
`delete from student`
- 删除 id=4 的数据
`delete from student where id=4;`


#### 改
```Java  
update   student  set name ="需要修改的成为的值" where id = 1;```


#### 查 
- 查询表中的全部数据
 `select * from student(表名)`
- 查询具体的某些字段
`select name,id from student;`
- 用别名来代表具体的字段
`select name as n,id as i from student;`
- 消除重复行
`select distinct gender from student ;`

##### 条件查询
- `select * from student where id>8 ;`

##### 逻辑运算符
###### and  
`select * from student where id >8 and id<10 ;`
######  or
`select * from student where id = 8 or id = 10 ;`
###### not
- % 表示任意多个字符 
`select * from student where name like "张%";`
- _表示一个字符
`select * from student where name like "张_";`

###### in 表示在一个非连续的范围内
id 为 1和3 的数据
`select * from student where id in (1,3)`
###### between...and... 表示在一个连续的范围内
id 在 1到3 之间的数据
`select * from student where id between 1 to 3 `
###### null 
- is null 
`select * from student where address is null`
- is not null 
`select * from student where address is  not null`

##### 聚合
- count() 数量求和

id > 1的总数
`select count(*) from student where id>1`

id > 1 列名为name的一共有多少
`select count(name) from student where id > 1`

- max() 最大 
id最大的是几
`select max(id) from student ;`


- min() 最小
id最小的是几
`select min(id) from student `


- sum()总和
`select sum(id) from student`


- avg()平均值
`select avg(id) from student`


- group by  
查询学生表中，用gender进行分组。并计算数量，聚合到一起。
`select gender , count(*) where students group by gender;`


- having 
- 是对原始数据集进行筛选。having是对分组后的数据进行筛选
`select count(*),gender from students group by gender having gender =0`


- order by asc (正序 默认)  desc 倒序

```select *from students order by  id   asc|desc```

按照 age 排序。如果相同的话。那么按照 id 排序 
```select *from students order by  age asc ,id desc```


- limit 分页

从 0 开始 的 前两条数据 

`select * from students limit 0,2;`

`select * from students where id > 1  limit 0,2;`


- 关联 


