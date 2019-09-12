---
title: mysql 语句练习_数据库、表的基本操作
date: 
tags: 
- go
- mysql
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![åæ¥èµ, è±, å¼è±, é"è², å¤å­£, çå¼, è±ç£, åæ¥èµçå­æ®µ, æ¤ç©ç¾¤](/images/sunflower-4440930__340.jpg)

<!-- more -->

> 向数据库中插入中国省市县数据从而进行操作

查看：https://github.com/songjiabin/goproject/tree/master/goproject/src/mysql


###### 创建数据库如果不存在则创建
```Java
 create database if not exists stu;
```
##### 如果创建数据库的时候，需要用到特殊字符 比如`create`
```Java
create  database  if not exists  `create` ;
```

##### 创建的时候指定字节码
```Java
create database if not exists `teacher` charset=gbk;
```

##### 删除数据库
```Java
drop database stu;
drop database  if exists  stu; 如果存在则删除
```

##### 显示创建数据库的语法
```Java
 show create database teacher;
```
##### 修改数据库
```Java
修改数据库tearcher的编码格式为 utf8;
alter database teacher charset=utf8;
```

##### 创建表的时候出现中文
```Java
set name gbk
```


##### 创建表
> COMMENT 备注

```Java
CREATE TABLE IF NOT EXISTS teacher(
id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键',
NAME VARCHAR(20) NOT NULL COMMENT '姓名',
phone VARCHAR(11) COMMENT '电话',
`add`  VARCHAR(100) DEFAULT '地址不详' COMMENT '地址'
)ENGINE=INNODB;
```
在数据库X中创建数据Y中的表B
```Java
CREATE TABLE IF NOT EXISTS mydb.testTable(
id INT  AUTO_INCREMENT PRIMARY KEY COMMENT '主键',
NAME VARCHAR(10)
);
```

##### 查看创建表的sql语句
```Java
show create table testTable;
show create table testTable\G; 查看更加详细的数据信息 
```
##### 查看表的结构
```Java
desc  testTable;
```

##### 删除表
```Java
drop table if exists  testTable;
drop table if exists   tables1,tables2; 一次删除多个
```

##### 修改表
- 给表新增一列
```Java
alter table teacher add age int ;
alter table teacher add email varchar(20)  first; 添加email到第一的位置上
ALTER TABLE teacher ADD sex VARCHAR(20) AFTER NAME;  在name的后面添加 
```

- 删除一列
```Java
ALTER TABLE teacher DROP email; 删除指定的字段
```

- 改名字和类型
```Java
ALTER TABLE teacher  CHANGE sex  sex_x  CHAR ; 修改 sex -> sex_x ,类型为char 类型
```
- 改类型
```Java
 ALTER TABLE teacher MODIFY sex_x VARCHAR(10); 将teacher sex_x 的类型改为 varchar(10)
```

- 修改表的名字
```Java
ALTER TABLE teacher RENAME TO teacher_1;
```


##### 表操作
- 创建表
```Java
创建表 stu 
CREATE TABLE stu(
id INT AUTO_INCREMENT PRIMARY KEY COMMENT '主键',
NAME VARCHAR(20) NOT NULL,
addr VARCHAR(30) DEFAULT '地址不详',
sorce INT COMMENT '成绩'
);
```
- 插入数据
```Java
INSERT INTO stu(id,NAME,addr,sorce) VALUES(
1,'宋佳宾','北京邮电大学',100);
INSERT INTO stu VALUES ( NULL,"大个子",DEFAULT,22); 插入带有默认值的
INSERT INTO stu VALUES ( NULL,"大个子1",DEFAULT,42),(NULL,'小个子',DEFAULT,33); 一次插入多个数据
```

- 更新数据
```Java
update stu set addr="山西" where id=4;
update stu set addr="山西1",name='乔致庸'  where id=4; 修改两个

```

- 删除数据
```Java
DELETE FROM stu WHERE id =4; 
delete from stu; 清空了里面的数据，但是id还会继续之前的增长
truncate table stu; 彻底清空了，id也会重新增长
```




##### 复制表
```Java
create table stu1 select * from stu; 复制表stu 给 stu1 。这样复制的话，数据过去，但是主键过不去。

create table stu2 like stu; 复制表stu的表结构 
```

 