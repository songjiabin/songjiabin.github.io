---
title: 重学mysql
tags: 
- mysql
- go
copyright: true
categories: mysql
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![](/images/1575373608933.png)



<!-- more -->

### 基本简单的操作

#### 数据库，表的基本操作

##### 创建数据库

```mysql
create database sjb charset utf8;
```

##### 查看所有的数据库

```mysql
show databases;
```

##### 使用哪个数据库

```mysql
use sjb;
```

##### 删除一个数据库

```mysql
drop database sjb;
```

##### 修改数据库的名字

```mysql
想什么呢？数据库创建好后是不能改名字的
```

##### 查看所有表

```mysql
show tables;
```

##### 创建一个简单的表

```mysql
create table stu(
-> snum int ,
-> sname varchar(10)
-> )engine myisam charset utf8;
engine--> 引擎和表的性能相关
```

#####删除表

```mysql
drop table sjb;
```

#####  修改表名

```mysql
rename table oldername to newname;
```

##### 插入数据往表中

```mysql
insert into tab_stu values 
-> (1,'宋佳宾'),
-> (2,'sjb');
```

##### 清空表数据(快速清空)

```mysql
truncate tab_stu(表名)；
```

##### 彻底删除数据库(未完待续)

```mysql
delete table tab_stu;
```

##### 关于中文乱码的问题

> 有的时候字符集乱码。是因为我们连接时候使用的时候使用的终端是`gbk`,但是连接是数据库是`utf8`,这个时候就导致了字符集乱码。解决方案是在终端使用 `set names utf8`。让当前终端的字符集和数据库的字符集是统一的。

#####  tee操作

> 将我敲得`sql`及结果都输出到一个`sql`文件中

```mysql
tee /home/sql.sql  将我写入的sql及其结果都输入到sql.sql中去。
```

#### 表的增删改查

> 先创建一个班级表

 ```mysql
create table class(
id int primary key auto_increment,         # 主键并自增
sname varchar(10) not null default '', 	   # 姓名
gender char(1) not null default '',        # 性别 
company varchar(20) not null default '',   # 公司
salary decimal(6,2) not null default 0.00, # 工资 decimal(6,2) 总共6位数，小数点后面两位
fanbu smallint not null default 0 		   # 饭补	
)engine myisam charset utf8;
 ```

##### 查看表的数据类型

```mysql
desc class # 查看表的结构
```

##### 插入数据

> 向那个表插入？插入哪些值？插入的值是什么？

```mysql
insert into class 
(id,sname,gender,company,salary,fanbu)
values
(1,'宋佳宾','男','头条',8888.88,222);
```

> 插入部分数据

```mysql
insert into class 
(sname,gender,salary)
values
('鸣人','男',9999.88);
```

> 如果插入所有列数据的话，可以简写为下面的

```sql
insert into class 
values
(3,'卡卡西','男','新浪',4444.22,222);
```

> 插入多条数据

```sql
insert into class
(sname,salary,company)
values
('张飞',1.23,'张氏集团'),
('刘备',0.26,'皇室宗亲'),
('孙策',1.23,'江东子弟');
```

#####  更新数据

> 修改哪个表？哪个列？哪一行？

```sql
# 修改表 class 将其中的饭补改为20
update class 
set fanbu = 20
where id = 1;  
```

> 修改多个列

```sql
update class 
set 
gender='女',     # 修改多个列的话 用 , 隔开 
company = '江鸟公司'
where name = '卡卡西';
```

> 查找多个条件

```sql
update class
set 
gender='男',
company='大保健集团' 
where sname = '卡卡西' and  gender ='女';
```

##### 删除数据

> 删除哪个表？删除哪一行？

```sql
delete from class
where id = 2 ; # 条件  删除哪个行？
```

> 删除 工资大于8888的数据

```sql
delete from class 
where salary>8888;
```

> 删除表中的所有数据

```sql
delete from class ;  #警告 删除所有的数据
```

##### 查询数据

> 查询哪个表？查询哪些列？

```sql
select sname,company,salary from class where id = 4;
+--------+--------------+--------+
| sname  | company      | salary |
+--------+--------------+--------+
| 刘备   | 皇室宗亲     |   0.26 |
+--------+--------------+--------+
```

### 建表（列类型）

#### mysql 三大数据类型 

##### 数值型

- Tinyint (1字节) 具体指：`-126~127` 或`0~255` 。一个字节是8位，正数的话表示`0~2^8-1`。或者是`-2^7~2^7-1`如果是表示负数的话，前面的一位`0`表示正数，`-1`表示负数。

- smallint 
- mediumint  
- int 
- bigint 

![](/images/1575373872570.png)

> 当我需要存储数据的时候，无非就是正数或者负数。怎么声明呢？

###### 声明正负值 

先声明一个表作测试

```sql
create table class2(
    -> sname varchar(20) not null default '',                       
    -> age tinyint not null default 0   # tinyint测试下他能表示的值                       
    -> )engine myisam charset utf8; 
```

插入数据

```sql
insert into class2 
    -> (sname,age)
    -> values 
    -> ('刘备',28);
# 查询结果是：
select age from class2;
+-----+
| age |
+-----+
|  28 |
+-----+
```

因为插入的不是临界值，所以并不能测试处理表示的范围。因为负数的范围是`-128~127`，那么插入一个`128`试试，如果`128`报错，那么说明插入的默认范围就是`-128~127`。

```sql
insert into class2 
    -> (sname,age)
    -> values 
    -> ('大龄剩男',128);
报错了:
ERROR 1264 (22003): Out of range value for column 'age' at row 1。
原因很是明显，超出了能存储的范围了。说明`tinyint`存储的默认范围就是-128~127
```

- **unsigned** 表示无符号的（就是表示的是正数，不能表示负数）

添加一列`score`分数，分数不能为负数，所以加了修饰 `unsigned`

```mysql
alter table class2 add score tinyint unsigned not null default 0;
```

- M （声明多少行）

- zerofill （零填充）

> `zerofill`和`M`一起连用才有意义。举个栗子，我们的学生学号，位数都是相同的。如`001`和`138`位数都是3位。当前面的位数不够使用0来填充的。比如（**tinyint(5) 相当于tinyint(M)**）`M`就是补零的宽度。



```mysql
# 给表添加sum列，类型我smallint类型，一共是五位，位数不够用零来凑， zerofill不能为负数
alter table class2  add sum smallint(5) zerofill not null default 0 ;
#查看结构
desc class2;
+-------+-------------------------------+------+-----+---------+-------+
| Field | Type                          | Null | Key | Default | Extra |
+-------+-------------------------------+------+-----+---------+-------+
| sname | varchar(20)                   | NO   |     |         |       |
| age   | tinyint(4)                    | NO   |     | 0       |       |
| score | tinyint(3) unsigned           | NO   |     | 0       |       |
|# 可以看到下面 unsigned 自动填充了是因为当补齐0的时候，负数是不可行的
|sum   | smallint(5) unsigned zerofill  | NO   |     | 00000   |       |
+-------+-------------------------------+------+-----+---------+-------+
# 插入数据--》
insert into class2 (sname,sum) values ('卢布',1);
insert into class2 (sname,sum) values ('张飞',20);
 select * from class2;
+--------+-----+-------+-------+
| sname  | age | score | sum   |
+--------+-----+-------+-------+
| 刘备   |  28 |     0 | 00000 |
| 卢布   |   0 |     0 | 00001 |
| 张飞   |   0 |     0 | 00020 |
+--------+-----+-------+-------+
```

##### 浮点数(float)

> 需要研究的无非就是左边可以存几位，右边可以存几位。如1.23 或者 234.2 等

```mysql
# M代表总的位数 D表示小数点后面的个数 如：Float(6,4)表示的范围是 -9999.99~9999.99
Float(M,D) 
```

创建一个表`xinshui`(薪水表)

```mysql
create table  xinshui (
    -> sname varchar(20) not null default '',
    -> gongzi float(6,2) 
    -> )engine myisam charset utf8;
#插入数据
insert into xinshui
    -> (sname,gongzi)
    -> values 
    -> ('sjb',9999.99);

```

**注意：float类型的值也可以用`unsigned`来修饰，表示不能使用负数

添加一列 奖金

```mysql
alter table xinshui add jiangjin float(5,2) unsigned not null default 0;
查看表
desc xinshui;
+----------+---------------------+------+-----+---------+-------+
| Field    | Type                | Null | Key | Default | Extra |
+----------+---------------------+------+-----+---------+-------+
| sname    | varchar(20)         | NO   |     |         |       |
| gongzi   | float(6,2)          | YES  |     | NULL    |       |
| jiangjin | float(5,2) unsigned | NO   |     | 0.00    |       |
+----------+---------------------+------+-----+---------+-------+
# 添加一个错误的奖金（负数）
insert into xinshui 
(sname,jiangjin)
values
('宋佳宾',-253.44);
ERROR 1264 (22003): Out of range value for column 'jiangjin' at row 1
```

##### 浮点数（decimal）

> 这个比`float`更加精确，而`float`当位数长的话会有损精度

举个栗子

```mysql
# 创建账户的表
create table account(
    sname varchar(20) not null default '',
    zh1 float(9,2),  # 账户1 为float(9,2)类型
    zh2 decimal(9,2) # 账户2 为decimal(9,2)类型
)engine myisam charset utf8; 
# 插入数据
insert into account  (sname,zh1,zh2) values ('sjb',1234567.22,1234567.22);
# 查询结果可以看到
select * from account;
+-------+------------+------------+
| sname | zh1        | zh2        |
+-------+------------+------------+
| sjb   | 1234567.25 | 1234567.22 | #很明显了，float缺失精度，而decimal则正常
+-------+------------+------------+
```

##### 字符串

- char（定长）

> char(N) 定长字符串 。因为存储时占的长度是一样的，所有查找的时候效率要高。但是，当存储的字节达不到要求的长度N时，会用空格在尾部自动补齐N个长度。会造成资源空间的浪费。

- varchar（变长）

> varchar(N) 存储0~N个字符。不用空格补齐，但是会在列前，用`1~2`个字符来标志该列的内容长度

创建一个表，来试试`varchar 和 char`的区别

```mysql
create table test(
    ca char(6) not null default '',
    vca varchar(6) not null default ''
)engine myisam charset utf8; 
```

插入数据

```mysql
insert into test 
values
('hello','hello');

insert into test
values
('aa ','aa ');


select * from test;
+-------+-------+
| ca    | vca   |
+-------+-------+
| hello | hello |
+-------+-------+
# 上面是没有任何区别的，
# 这个时候用concat(连接的意思)
select concat(ca,'!'),concat(vca,'!') from test;
+----------------+-----------------+
| concat(ca,'!') | concat(vca,'!') |
+----------------+-----------------+
| hello!         | hello!          | 
| aa!            | aa !            |#可以看到varchar 类型后面有之前存储的空格
+----------------+-----------------+#而char类型之前保存的空格，不见了

char类型如果存储的不够定长，那么会用空格补齐，当取出来的时候，会自动去除空格。所以我们刚开始加入的空格不见了！

```

**注意：varchar 和 char限制的字符，不是字节**

- text 文本，存储量会很大。当时用text的时候，不能给其设置默认值
- blob 二进制类型，用来存储图像，音频等二进制信息。（老老实实存进去，然后原样子取出来。防止因为字符集的问题导致数据的丢失）



##### 日期时间类型

###### date

> 存储年月日 ( YYYY-MM-DD 存储范围： 1000-01-01 到 9999-12-31)

```mysql
create table test3(
    star varchar(20) not null default '',
    birth date not null default '2019-11-01'
)engine myisam charset utf8;
#存入数据
insert into test3
values
('宋佳宾','2019-01-17');
```

###### time

> 时间类型()

```mysql
#给test3 增加一列，增加一列签到的时间列
alter table test3 add sign time not null default '00:00:00';
#desc test3;
+-------+-------------+------+-----+------------+-------+
| Field | Type        | Null | Key | Default    | Extra |
+-------+-------------+------+-----+------------+-------+
| star  | varchar(20) | NO   |     |            |       |
| birth | date        | NO   |     | 2019-11-01 |       |
| sign  | time        | NO   |     | 00:00:00   |       |# 时间列
+-------+-------------+------+-----+------------+-------+

#插入数据
insert into test3 
(star,sign)
values
('天天','19:34:22');

```

###### datetime

> 年月日时分秒YYYY-mm-dd HH:mm:ss

```mysql
create table test4(
    sname varchar(20) not null default '',
    logintime datetime not null default '2000-01-01 00:00:00'  # 这里要写入一个准确的时间，不然建表错误
)engine myisam charset utf8;

#插入数据
insert into test4
(sname,logintime)
values
('张三','2019-12-25 15:22:22');
```

###### timestamp

> 自动获取当前的时间并插入到该列中。

```mysql
create table test5(
    ts timestamp default CURRENT_TIMESTAMP,
    id int 
)engine myisam charset utf8;
#desc 
desc test5;
+-------+-----------+------+-----+-------------------+-------+
| Field | Type      | Null | Key | Default           | Extra |
+-------+-----------+------+-----+-------------------+-------+
| ts    | timestamp | NO   |     | CURRENT_TIMESTAMP |       |
| id    | int(11)   | YES  |     | NULL              |       |
+-------+-----------+------+-----+-------------------+-------+
 
insert into test5
(id)
values
(1),(2),(3);
 
#再次插入 
insert into test5
(id)
values
(1),(2),(3); 

select * from test5;
+---------------------+------+
| ts                  | id   |
+---------------------+------+
| 2019-12-04 17:48:31 |    1 |
| 2019-12-04 17:48:31 |    2 |
| 2019-12-04 17:48:31 |    3 |
| 2019-12-04 17:49:08 |    4 |
| 2019-12-04 17:49:08 |    5 |
| 2019-12-04 17:49:08 |    6 |
+---------------------+------+
```

###### year

> 年份  只占一个字节（所以范围是1901~2155）

```mysql
create table test6(
    thing varchar(20) not null default '',
    ye year not null default '0000'
)engine myisam charset utf8;

# desc test6;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| thing | varchar(20) | NO   |     |         |       |
| ye    | year(4)     | NO   |     | 0000    |       |
+-------+-------------+------+-----+---------+-------+
 
insert into test6 
values 
('肥水之战',1923);
```

##### 建表语句

> 练习

|        列名称         |                            列类型                            | 默认值 | 是否是主键 |
| :-------------------: | :----------------------------------------------------------: | :----: | :--------: |
|          id           |                      int unsigned(正数)                      |        |     是     |
|       username        |                         varchar(20)                          |   ''   |            |
|        gender         | char(1)  1代表一个字符,占用一个字节。tinyint 一个字节也是可以的(0:女,1:男)。 |        |            |
|        weight         |                    tinyint unsigned(正数)                    |        |            |
|         birth         |                     date（占用3个字节）                      |        |            |
|    salary（工资）     |            decimal(8,2) 浮点型 最大值是999999.99             |        |            |
| lastlogin上次登录时间 |                   datetime（定长8个字节）                    |        |            |
|   info（个人简介）    |                        varchar(1500)                         |        |            |

> 分析，上面的表。除了`username` 和`info`两个列其他都是定长的。当是定长的话，在查找的时候会提高查找的速度。这个时候我们可以将两个`varchar`类型的改为`char`类型，虽然可能会存储上浪费空间，但是提高了速度!具体看下

```mysql
varchar(20)--->char(20) 	浪费空间可以接受
varchar(1500)--->char(1500) 浪费的空间就有点儿多了
所以这个时候需要把 info 拿出来，单独成为一张表。用户经常操作的数据放到一张表，其他的放在表中去。
#
lastlogin 上次登录时间 使用datetime 不是很理想，在实际开发中用int型时间戳
```

> 开始真正的建表了要

```mysql
create table member(
    id int unsigned auto_increment primary key,#主键并且自增长
    username char(20) not null default '',
    gender char(1) 	not null default '',
    weight tinyint unsigned not null default 0,
    birth date not null default '2000-01-01',
    salary decimal(8,2) not null default 0.00,
    lastlonin int unsigned not null default 0
)engine myisam 	 charset utf8; #设置引擎
```

###### 查看建表语句

```mysql
show create table m1(表名);
show create table m1;
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                       |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| m1    | CREATE TABLE `m1` (
  `uid` int(10) unsigned NOT NULL,
  `username` char(20) NOT NULL DEFAULT '',
  `gender` char(4) NOT NULL DEFAULT '',
  `birth` date NOT NULL DEFAULT '2000-01-01',
  PRIMARY KEY (`uid`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8 |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

```



##### 修改表

先创建一个表

```mysql
create table m1(
id int unsigned auto_increment primary key   
)engine myisam charset utf8;
```

###### 新增列

> alter  table  表名  (增加、修改、删除)  列名称  列类型  列参数 

```mysql
# 给m1表添加username列
alter table m1 add username char(20) not null default '';
# 给m1添加一个生日列
alter table m1 add birth date not null default '2000-01-01';
```

给`m1`添加列`gender`并将其放到`username`后面。

```mysql
alter table m1 add gender char(1) not null default '' after username;
```

 给`m1`添加列，在最前面。使用`first`

```mysql
#在m1上面的最上面添加 pid 列
alter table m1 add pid int not null default 0 first ;
#查看表
desc m1;
+----------+------------------+------+-----+------------+----------------+
| Field    | Type             | Null | Key | Default    | Extra          |
+----------+------------------+------+-----+------------+----------------+
| pid      | int(11)          | NO   |     | 0          |                |#跑到了最上面
| id       | int(10) unsigned | NO   | PRI | NULL       | auto_increment |
| username | char(20)         | NO   |     |            |                |
| gender   | char(1)          | NO   |     |            |                |
| birth    | date             | NO   |     | 2000-01-01 |                |
+----------+------------------+------+-----+------------+----------------+
```

###### 删除列

```mysql
# 删除表 m1的 pid 列 
alter table m1 drop pid;
```

###### 修改列类型

```mysql
# 修改表 
alter table m1 modify  gender char(4) not null default '';
```

###### 修改列的名字及其类型

```mysql
# 修改 表 m1 id字段 修改为uid 类型为 int  unsigned
alter table m1 change id uid int unsigned  
```

###  查

> 在学习查之前先建立一个小型的商城表。

```mysql
商品表
goods_id, //商品id 
cat_id,	  // 分类
goods_sn, // 商品分类
goods_name, //商品的名字
click_count, //商品的点击量 
goods_number,//库存量
marker_price,//市场价
shop_price,//本店价
add_time,//添加时间
is_best, //是否最好的
is_new, //是否最新上架 
is_not  //是否最热商品

# 开始创建库
create database myGoods charset  utf8;

# 开始创建表
create table goods (
  goods_id mediumint(8) unsigned primary key auto_increment, #商品id 
  goods_name varchar(120) not null default '',				 #商品名字	
  cat_id smallint(5) unsigned not null default '0',			 #商品分类
  brand_id smallint(5) unsigned not null default '0',  		 #商品的品牌
  goods_sn char(15) not null default '', 					 
  goods_number smallint(5) unsigned not null default '0',    #商品的数量
  shop_price decimal(10,2) unsigned not null default '0.00', #商品的现在价格
  market_price decimal(10,2) unsigned not null default '0.00',#商品的市场价
  click_count int(10) unsigned not null default '0'           #点击量
) engine=myisam default charset=utf8;

# 插入数据
insert into goods values (1,'kd876',4,8,'ecs000000',1,1388.00,1665.60,9),
(4,'诺基亚n85原装充电器',8,1,'ecs000004',17,58.00,69.60,0),
(3,'诺基亚原装5800耳机',8,1,'ecs000002',24,68.00,81.60,3),
(5,'索爱原装m2卡读卡器',11,7,'ecs000005',8,20.00,24.00,3),
(6,'胜创kingmax内存卡',11,0,'ecs000006',15,42.00,50.40,0),
(7,'诺基亚n85原装立体声耳机hs-82',8,1,'ecs000007',20,100.00,120.00,0),
(8,'飞利浦9@9v',3,4,'ecs000008',1,399.00,478.79,10),
(9,'诺基亚e66',3,1,'ecs000009',4,2298.00,2757.60,20),
(10,'索爱c702c',3,7,'ecs000010',7,1328.00,1593.60,11),
(11,'索爱c702c',3,7,'ecs000011',1,1300.00,0.00,0),
(12,'摩托罗拉a810',3,2,'ecs000012',8,983.00,1179.60,13),
(13,'诺基亚5320 xpressmusic',3,1,'ecs000013',8,1311.00,1573.20,13),
(14,'诺基亚5800xm',4,1,'ecs000014',1,2625.00,3150.00,6),
(15,'摩托罗拉a810',3,2,'ecs000015',3,788.00,945.60,8),
(16,'恒基伟业g101',2,11,'ecs000016',0,823.33,988.00,3),
(17,'夏新n7',3,5,'ecs000017',1,2300.00,2760.00,2),
(18,'夏新t5',4,5,'ecs000018',1,2878.00,3453.60,0),
(19,'三星sgh-f258',3,6,'ecs000019',12,858.00,1029.60,7),
(20,'三星bc01',3,6,'ecs000020',12,280.00,336.00,14),
(21,'金立 a30',3,10,'ecs000021',40,2000.00,2400.00,4),
(22,'多普达touch hd',3,3,'ecs000022',1,5999.00,7198.80,16),
(23,'诺基亚n96',5,1,'ecs000023',8,3700.00,4440.00,17),
(24,'p806',3,9,'ecs000024',100,2000.00,2400.00,35),
(25,'小灵通/固话50元充值卡',13,0,'ecs000025',2,48.00,57.59,0),
(26,'小灵通/固话20元充值卡',13,0,'ecs000026',2,19.00,22.80,0),
(27,'联通100元充值卡',15,0,'ecs000027',2,95.00,100.00,0),
(28,'联通50元充值卡',15,0,'ecs000028',0,45.00,50.00,0),
(29,'移动100元充值卡',14,0,'ecs000029',0,90.00,0.00,0),
(30,'移动20元充值卡',14,0,'ecs000030',9,18.00,21.00,1),
(31,'摩托罗拉e8 ',3,2,'ecs000031',1,1337.00,1604.39,5),
(32,'诺基亚n85',3,1,'ecs000032',4,3010.00,3612.00,9);
```

> 进行查询操作

#### 一些常用的筛选符号

> \> = < ...符号的运用

```mysql
# 查询商品主键为32的商品
select * from goods where goods_id =32;
# 查出不属于第三个栏目的商品信息
select goods_name ,cat_id from goods where cat_id != 3;
# 本店价格大于300的商品信息
select * from goods where shop_price>300;
```

#### in 

> 表示在哪些值之中

```mysql
# 取消商品类别是4和11的商品信息
select * from goods where cat_id in (4,11);
# 取出商品类别不是3和不是11的所有商品信息
 select * from goods where cat_id not in (3,11);
#上面的使用and来实现
select * from goods where cat_id!=3 and cat_id!=11;
```

#### between and之间

> 从一个值到一个值之间 （包括两端的值）

```mysql
# 取出商品价格>=100 <=500的商品信息
select *from goods where shop_id between 100 and 500;
```

#### 综合复习

```mysql
# 取出价格大于100且小于300，或者大于4000且小于5000的商品信息
select * from goods where (shop_price >=100 && shop_price<=300) or (shop_price>=4000&&shop_price<=5000);
# 取出第三个商品类别下 价格小于1000或者大于3000。同时点击量大于5的商品
select * from goods where cat_id =3 and (shop_price < 1000 or shop_price >3000) and click_count > 5;  
```

#### 模糊查询

> % 匹配任意字符
>
> _ 匹配任意一个字符

```mysql
#查出以为 诺基亚开头的商品
select * from goods where goods_name like "诺基亚%";
#查询以诺基亚N开头，后面只有两个字符的商品信息
select *from goods where goods_name like "诺基亚N__";
```

#### 将列变成变量来计算

> 查询现价比市场的价格便宜多少钱  

```mysql
# 查看现在手机的价格比当前市场的价格便宜多少
 select goods_name,market_price-shop_price as 差价 from goods  where 1;
+----------------------------------------+----------+
| goods_name                             | 差价     |
+----------------------------------------+----------+
| kd876                                  |   277.60 |
| 诺基亚n85原装充电器                    |    11.60 |
| 诺基亚原装5800耳机                     |    13.60 |
| 索爱原装m2卡读卡器                     |     4.00 |
| 胜创kingmax内存卡                      |     8.40 |
| 诺基亚n85原装立体声耳机hs-82           |    20.00 |
| 飞利浦9@9v                             |    79.79 |
| 诺基亚e66                              |   459.60 |
| 索爱c702c                              |   265.60 |
| 索爱c702c                              | -1300.00 |
| 摩托罗拉a810                           |   196.60 |
| 诺基亚5320 xpressmusic                 |   262.20 |
| 诺基亚5800xm                           |   525.00 |
| 摩托罗拉a810                           |   157.60 |
| 恒基伟业g101                           |   164.67 |
| 夏新n7                                 |   460.00 |
| 夏新t5                                 |   575.60 |
| 三星sgh-f258                           |   171.60 |
| 三星bc01                               |    56.00 |
| 金立 a30                               |   400.00 |
| 多普达touch hd                         |  1199.80 |
| 诺基亚n96                              |   740.00 |
| p806                                   |   400.00 |
| 小灵通/固话50元充值卡                  |     9.59 |
| 小灵通/固话20元充值卡                  |     3.80 |
| 联通100元充值卡                        |     5.00 |
| 联通50元充值卡                         |     5.00 |
| 移动100元充值卡                        |   -90.00 |
| 移动20元充值卡                         |     3.00 |
| 摩托罗拉e8                             |   267.39 |
| 诺基亚n85                              |   602.00 |
+----------------------------------------+----------+
31 rows in set (0.00 sec)
```

#### 栗子练习

```mysql
# 先创建一个表
create table num(
 num int 
);
#插入数据
insert into num values 
(3),
(12),
(15),
(25),
(23),
(29),
(34),
(37),
(32),
(45),
(48),
(52);
# 现在将num [20,29]之间改为20。
# 将 num [30,39]之间改为30

# sql语句如下
// floor 的意思是将 将值向下取整
update num set num =floor(num/10)*10 where num>=20 and num<=30;
```

#### 统计函数

##### max 求最大

```mysql
# 查出最贵的商品的价格
select max(shop_price) from goods;
```

##### min 求最小 

```mysql
# 查出最便宜的商品价格
select  min(shop_price) from goods;
```

##### sum 求和

```mysql
# 统计下本店一共有多少商品
select sum(goods_number) from goods;
```

##### avg 平均数

```mysql
# 查看所有商品的平均价格
select avg(shop_price) from goods;
```

#####  count 求数量（总行数）

> select  count(*) from goods; 数所有的行数

```mysql
# 本店有多少种商品
select count(cat_id) as 种类 from goods;
```

#####  group_concat (拼接)

> 自带 , 分割符

```mysql
# 取出第四个栏目所有商品的goods_id
select goods_id, cat_id from goods where cat_id=4;
+----------+--------+
| goods_id | cat_id |
+----------+--------+
|        1 |      4 |
|       14 |      4 |
|       18 |      4 |
+----------+--------+
# 取出这个这个goods_id后并拼接起来
select group_concat(goods_id,'') from goods where cat_id=4 group by cat_id;
+---------------------------+
| group_concat(goods_id,'') |
+---------------------------+
| 1,14,18                   |
+---------------------------+
```



#### 分组

> 一般情况下是 分组后再统计

```mysql
# 计算每个栏目下的库存量之和
select  cat_id ,  sum(goods_number) from goods  group by cat_id;
+--------+-------------------+
| cat_id | sum(goods_number) |
+--------+-------------------+
|      2 |                 0 |
|      3 |               203 |
|      4 |                 3 |
|      5 |                 8 |
|      8 |                61 |
|     11 |                23 |
|     13 |                 4 |
|     14 |                 9 |
|     15 |                 2 |
+--------+-------------------+
9 rows in set (0.00 sec)
```

#### having 

> 查出来的临时表，可以再使用`having`在进行查询。
>
> having是对结果集操作，where 针对表做的操作

```mysql
# 查询本店比市场价省的钱，并且要求省钱200元以上的取出来。
select goods_name ,(market_price-shop_price) as chajia from goods having chajia > 200; 
```

##### 练习

```mysql
# 查询每种商品所以积压的货款
select goods_name, shop_price,goods_number, (shop_price * goods_number) as allPrice from goods   having allPrice >200; 
# 查询该店挤压的总货款
select sum(shop_price * goods_number) as 总货款 from goods;
# 查询每个栏目下挤压的货款
select cat_id,sum(shop_price * goods_number) from goods order by cat_id;
# 查询栏目下挤压的货款>20000的
select cat_id,sum(shop_price * goods_number) as allPrice from goods group by cat_id   having  allPrice>20000;
# 查询本店价比市场价省的钱，且筛选出省钱200以上的商品(having实现)
select goods_name ,(shop_price-market_price) as price  from goods having price>200; 
#针对上面我们使用where实现
select goods_name ,(shop_price-market_price) as price  from goods where  (shop_price-market_price)>200;
```

##### 有难度的练习

```mysql
# 创建表
create table result (
    name varchar(20),
    subject  varchar(20),
    score tinyint 
)engine myisam charset  utf8;
# 插入数据
insert into result 
      values 
      ('张三','数学',90),
      ('张三','语文',50),
      ('张三','地里',40),
      ('李四','语文',55),
      ('李四','政治',45),
      ('王五','政治',30);
# 查询两门或者两门以上 不及格者的平均成绩

--- 第一种 --- 
# 1、查询所有人的平均分
select  name,avg(score) from result group by name;
# 2、在筛选出每个人挂科的情况
select name,score<60 from result;
# 综合来说
select name,avg(score),sum(score<60) as num from result group by name having num>=2;

--- 第二种 --- 
# 两门或者两门以上 不及格者的平均成绩
# 1、先找挂科数>=2的人。
 select name ,count(1) as 挂科数 from result where score<60 group by  name having count(1)>=2 ;
+--------+----------+
| name   | 挂科数    |
+--------+----------+
| 张三   |        2 |
| 李四   |        2 |
+--------+----------+
# 2、再算平均分
 select name ,avg(score) from result where name in (
    ->     select name from (select name ,count(1) as 挂科数 from result where score<60 group by  name having count(1)>=2) as temp 
    -> ) group by name;
+--------+------------+
| name   | avg(score) |
+--------+------------+
| 张三   |    60.0000 |
| 李四   |    50.0000 |
+--------+------------+
2 rows in set (0.00 sec)
```

#### 排序

> 语法 order by  列名   desc(降序)  asc(升序)

```mysql
select * from goods  order  by goods_id desc ;
```

##### 需要用多个字段进行排序

```mysql
# 先按照 cat_id 进行降序   再按照 shop_price 升序
select *from goods order by cat_id desc ,shop_price asc;
```

#### 固定的条数 limit

> limit [a,b]   a是偏移量（跳过的行数）   b是一共取出几个来

```mysql
# 取出 3到5 的数据来 
select goods_name from goods  order by shop_price limit 2,3;
```

#### 语句的执行顺序

> where-->group by-->having-->order by-->limit 

##### 综合语句练习

```mysql
# 查询出 每个栏目下 id号最大（最新）的一条商品
select  goods_name, cat_id  from goods group by cat_id ;
```

####  子查询

##### from子查询

```mysql
# 查询本网站最新的(goods_id最大)的一条商品，不能用排序 order by 
# 1.使用order by 
select max(goods_id)  as maxId  from goods group by goods_id  order by maxId  desc limit 0,1;
# 2、不使用order by
# 2-1 查询最大的 id 
select  max(goods_id) from goods;
# 2-2 使用子查询
select * from goods where goods_id = (select  max(goods_id) from goods);


# - - - 
# 1、先使用order by 对cat_id升序，在使用 goods_id 降序
select * from goods order by cat_id asc,goods_id desc;
# 2、将处理的数据在进行分组
select * from (select * from goods order by cat_id asc,goods_id desc) as temp group by cat_id;# 这个再新版本是不行的，因为group by 不会查询与其无关的数据 
```

##### exists 子查询

```mysql
# 先创建一张类别表
create table category (
cat_id smallint unsigned auto_increment primary key,
cat_name varchar(90) not null default '',
parent_id smallint unsigned
)engine myisam charset utf8;
# 插入数据
INSERT INTO `category` VALUES
(1,'手机类型',0),
(2,'CDMA手机',1),
(3,'GSM手机',1),
(4,'3G手机',1),
(5,'双模手机',1),
(6,'手机配件',0),
(7,'充电器',6),
(8,'耳机',6),
(9,'电池',6),
(11,'读卡器和内存卡',6),
(12,'充值卡',0),
(13,'小灵通/固话充值卡',12),
(14,'移动手机充值卡',12),
(15,'联通手机充值卡',12);
# 查询有商品的栏目 （有的栏目没商品，）
# 为一种方式
select * from category where cat_id in (select cat_id from goods group by cat_id);
# 使用 exists (如果后面的判断为真，晒出来，否自不筛出来)
select *from category where exists (select * from goods where goods.cat_id=category.cat_id);
```

#### 奇怪的null

> 当建表的时候 会加上 `not null` `default ""或者 0`之类的 为什么呢？ 

```mysql
# 创建表
 create table test9 (
    -> sname varchar(10)
    -> )engine myisam charset utf8;
insert into test9 values
('a'), # 正常
('null'), # 字符串null 
(null);   # null 

# 查询所有的非null
select * from test9 where sname!=null;
Empty set (0.00 sec)
# 查询所有的等于null的
select * from test9 where sname=null;
Empty set (0.00 sec)
# 结果发现上面查询处理的数据是不对的

看下面
select null=null;
+-----------+
| null=null |
+-----------+
|      NULL |
+-----------+

 select null!=null;
+------------+
| null!=null |
+------------+
|       NULL |
+------------+

所有null是一个比较特殊的运算符 必须使用 is null 、 is not null 

select * from test9 where sname is not null;
+-------+
| sname |
+-------+
| a     |
| null  #字符串 null |
+-------+

# 查询为null的情况
select *from test9 where sname is null;
+-------+
| sname |
+-------+
| NULL  |
+-------+
1 row in set (0.00 sec)

```

**所以来说啊，null 不好比较 因为碰到比较运算，一律返回null 。只能使用 is null 或者 is not nul进行比较。而且效率不高不利用提高索引效果。因此在建表的时候往往声明 not null default 0 / ''**

#### 左连接

##### 左连接预备

集合A （1,2,3） 集合B(4,5,6)

集合A*集合B=集合C 

那么集合C的可能是（（1,4）（1,5）（1,6）（2,4）（2,5）（2,6）（3,4）（3,5）（3,6））

集合C的有A*B种可能性  A的数量乘以B的数量=9

> **一张表就是一个集合，每一行就是一个元素**

那么如果实现两个表相乘呢？

```mysql
# 创建表 a
create table a (
    id tinyint,
    sname varchar(10) 
)engine myisam charset utf8;
# 创建表 b
create table b(
    id tinyint,
    aname varchar(20)
)engine myisam charset utf8;

# 插入数据
insert into a values
 (1,'小明'),
 (2,'小红'),
 (3,'小张');

insert into b values
(66,'老虎'),
(88,'猴子');

# 两张表一起查询 
select * from a,b;
+------+--------+------+--------+
| id   | sname  | id   | aname  |
+------+--------+------+--------+
|    1 | 小明   |   66 | 老虎   |
|    1 | 小明   |   88 | 猴子   |
|    2 | 小红   |   66 | 老虎   |
|    2 | 小红   |   88 | 猴子   |
|    3 | 小张   |   66 | 老虎   |
|    3 | 小张   |   88 | 猴子   |
+------+--------+------+--------+

# 发现和刚才两个集合相乘得到的结果是一样的。

# 当两个表有字段关联的时候
select goods_name, goods.cat_id,category.cat_id ,cat_name from goods,category  where goods.cat_id=category.cat_id;
+----------------------------------------+--------+--------+---------------------------+
| goods_name                             | cat_id | cat_id | cat_name                  |
+----------------------------------------+--------+--------+---------------------------+
| kd876                                  |      4 |      4 | 3G手机                    |
| 诺基亚n85原装充电器                    |      8 |      8 | 耳机                      |
| 诺基亚原装5800耳机                     |      8 |      8 | 耳机                      |
| 索爱原装m2卡读卡器                     |     11 |     11 | 读卡器和内存卡            |
| 胜创kingmax内存卡                      |     11 |     11 | 读卡器和内存卡            |
| 诺基亚n85原装立体声耳机hs-82           |      8 |      8 | 耳机                      |
| 飞利浦9@9v                             |      3 |      3 | GSM手机                   |
| 诺基亚e66                              |      3 |      3 | GSM手机                   |
| 索爱c702c                              |      3 |      3 | GSM手机                   |
| 索爱c702c                              |      3 |      3 | GSM手机                   |
| 摩托罗拉a810                           |      3 |      3 | GSM手机                   |
| 诺基亚5320 xpressmusic                 |      3 |      3 | GSM手机                   |
| 诺基亚5800xm                           |      4 |      4 | 3G手机                    |
| 摩托罗拉a810                           |      3 |      3 | GSM手机                   |
| 恒基伟业g101                           |      2 |      2 | CDMA手机                  |
| 夏新n7                                 |      3 |      3 | GSM手机                   |
| 夏新t5                                 |      4 |      4 | 3G手机                    |
| 三星sgh-f258                           |      3 |      3 | GSM手机                   |
| 三星bc01                               |      3 |      3 | GSM手机                   |
| 金立 a30                               |      3 |      3 | GSM手机                   |
| 多普达touch hd                         |      3 |      3 | GSM手机                   |
| 诺基亚n96                              |      5 |      5 | 双模手机                  |
| p806                                   |      3 |      3 | GSM手机                   |
| 小灵通/固话50元充值卡                  |     13 |     13 | 小灵通/固话充值卡         |
| 小灵通/固话20元充值卡                  |     13 |     13 | 小灵通/固话充值卡         |
| 联通100元充值卡                        |     15 |     15 | 联通手机充值卡            |
| 联通50元充值卡                         |     15 |     15 | 联通手机充值卡            |
| 移动100元充值卡                        |     14 |     14 | 移动手机充值卡            |
| 移动20元充值卡                         |     14 |     14 | 移动手机充值卡            |
| 摩托罗拉e8                             |      3 |      3 | GSM手机                   |
| 诺基亚n85                              |      3 |      3 | GSM手机                   |
+----------------------------------------+--------+--------+---------------------------
这个时候 就查出来了两个表中相同的数据了 

# 不过两张表全相乘 效率太低了.

# 这个时候需要左连接了
# 左边的表 left join 右边的表  on 条件  
goods left join category  on  goods.cat_id=category.cat_id; #这个是左连形成的临时表
# 对 临时表 进行在查询
select goods_name, goods.cat_id ,cat_name from ( goods left join category  on  goods.cat_id=category.cat_id);
```

**注意的是：要将	  left  join on  看成表来进行操作  可以很多张表来连接查询**

##### 左连接练习

```mysql
# 取出第四个栏目下的商品，以及商品的栏目名
select goods_name,cat_name,goods.cat_id from (goods left join category  on  goods.cat_id=category.cat_id) where goods.cat_id=4;
```

##### 左右内连接的区别

> `A left join b`  等价于   `B right join  A` 所以尽量使用左连接，处于移植时候的兼容性考虑

#####  内连接的特性(`inner  join on `)

内连接是左右连接的交集(`inner join on `)

##### 外连接

外连接就是左右连接的并集，但是在`mysql`中不支持外连接。

##### 左连接的demo

```mysql
# 创建男士表
create table man (
    mname varchar(20),
    other char(1)
)engine myisam charset utf8;

insert into man values
('屌丝','A'),
('李四','B'),
('王五','C'),
('高富帅','D'),
('郑七','E');
 
# 创建女士表
 
create table women(
    wname varchar(20),
    other char(1)
)engine myisam charset utf8;

insert into women values
('空姐','B'),
('大S','C'),
('阿娇','D'),
('张柏芝','D'),
('林黛玉','E'),
('宝钗','F');
 
# 左连接查询
# 1、左连接的语句
# 1-1 当以man为左边的时候
man left join women  on man.other=women.other 
select man.* ,women.* from (man left join women  on man.other=women.other );
+-----------+-------+-----------+-------+
| mname     | other | wname     | other |
+-----------+-------+-----------+-------+
| 李四      | B     | 空姐      | B     |
| 王五      | C     | 大S       | C     |
| 高富帅    | D     | 阿娇      | D     |
| 高富帅    | D     | 张柏芝    | D     |
| 郑七      | E     | 林黛玉    | E     |
| 屌丝      | A     | NULL      | NULL  |
+-----------+-------+-----------+-------+
当左边表示man的时候，会匹配所有的字段，如果找不到对应的字段会以 null。如屌丝没有女朋友。
| 屌丝      | A     | NULL      | NULL  |
当一个人有两个相匹配的话，那么会出现两行，如下：
| 高富帅    | D     | 阿娇      | D     |
| 高富帅    | D     | 张柏芝    | D     |

# 1-2 当以women为左边的时候
women left join man   on man.other=women.other
select man.*,women.* from (women left join man   on man.other=women.other);
+-----------+-------+-----------+-------+
| mname     | other | wname     | other |
+-----------+-------+-----------+-------+
| 李四      | B     | 空姐      | B     |
| 王五      | C     | 大S       | C     |
| 高富帅    | D     | 阿娇      | D     |
| 高富帅    | D     | 张柏芝    | D     |
| 郑七      | E     | 林黛玉    | E     |
| NULL      | NULL  | 宝钗      | F     |
+-----------+-------+-----------+-------+
这次是以women表为左边，进行关联man表。所以women中的数据是全部都有的，如果没有那么用null表示。
| NULL      | NULL  | 宝钗      | F     |

```

#### union 

> 合并两条或者多挑语句的结果

```mysql
# 查询价格小于100元或者价格大于4000元的商品。
# 1、使用or
select * from goods where shop_price <100 or shop_price>4000;
# 2、使用union 合并多个语句的结果
select * from goods where shop_price <100;
select *from goods where shop_price > 400;
# 将两个语句合并
select * from goods where shop_price <100 union select *from goods where shop_price > 400;
```

##### 当列表不一致的时候

当列名不一致的时候，以第一个`sql`的取出来的列名为准。

##### 使用的场景

只要是`union `相关联的表的字段（列）数量是一样的，那么这可以进行关联。

```mysql
select shop_price,goods_name from goods union select mname,other from man;
+------------+----------------------------------------+
| shop_price | goods_name                             |
+------------+----------------------------------------+
| 1388.00    | kd876                                  |
| 58.00      | 诺基亚n85原装充电器                    |
| 68.00      | 诺基亚原装5800耳机                     |
| 20.00      | 索爱原装m2卡读卡器                     |
| 42.00      | 胜创kingmax内存卡                      |
| 100.00     | 诺基亚n85原装立体声耳机hs-82           |
| 399.00     | 飞利浦9@9v                             |
| 2298.00    | 诺基亚e66                              |
| 1328.00    | 索爱c702c                              |
| 1300.00    | 索爱c702c                              |
| 983.00     | 摩托罗拉a810                           |
| 1311.00    | 诺基亚5320 xpressmusic                 |
| 2625.00    | 诺基亚5800xm                           |
| 788.00     | 摩托罗拉a810                           |
| 823.33     | 恒基伟业g101                           |
| 2300.00    | 夏新n7                                 |
| 2878.00    | 夏新t5                                 |
| 858.00     | 三星sgh-f258                           |
| 280.00     | 三星bc01                               |
| 2000.00    | 金立 a30                               |
| 5999.00    | 多普达touch hd                         |
| 3700.00    | 诺基亚n96                              |
| 2000.00    | p806                                   |
| 48.00      | 小灵通/固话50元充值卡                  |
| 19.00      | 小灵通/固话20元充值卡                  |
| 95.00      | 联通100元充值卡                        |
| 45.00      | 联通50元充值卡                         |
| 90.00      | 移动100元充值卡                        |
| 18.00      | 移动20元充值卡                         |
| 1337.00    | 摩托罗拉e8                             |
| 3010.00    | 诺基亚n85                              |
| 屌丝       | A                                      |
| 李四       | B                                      |
| 王五       | C                                      |
| 高富帅     | D                                      |
| 郑七       | E                                      |
+------------+----------------------------------------+
```

##### union后的结果集可以在进行操作

```mysql
select * from goods where shop_price <100 union select *from goods where shop_price > 400 order by shop_price asc;
```

##### union 重复行的问题(union all 为不去重)

```mysql
# 当两个表中数据是一模一样的时候 会自动去重，
select * from goods where cat_id=4 union select * from goods where cat_id=4 order by shop_price asc;
# 当两个表中数据是一模一样的时候 不想去重
select * from goods where cat_id=4 union all select * from goods where cat_id=4 order by shop_price asc;
```



##### 案例

```mysql
# 用 union 取出第四个栏目，和第五个栏目的商品，并按价格排序 升序
select * from goods where cat_id=4;
select * from goods where cat_id=5;
select * from goods where cat_id=4 union select * from goods where cat_id=5  order by shop_price asc;


# 用 union 查出第三个栏目下，价格前三高的商品，和第四个栏目下，前2高的商品。
select * from goods where cat_id=3 order by shop_price desc limit 3;
select * from goods where cat_id=4 order by shop_price desc limit 2;
# 进行合并
(select * from goods where cat_id=3 order by shop_price desc limit 3) 
union 
(select * from goods where cat_id=4 order by shop_price desc limit 2);
```

##### 案例2

```mysql
# 创建表A1
create table A1(
    -> id int,
    -> num int
    -> )engine myisam charset utf8;
alter table A1 modify id char(1);   
insert into  A1 values ('a',5),('b',10),('c',15),('d',10);

    
# 创建表A2
create table A2(
id int,
num int
)engine myisam charset utf8;
alter table A2 modify id char(1);   
insert into  A2 values ('b',5),('c',15),('d',20),('e',99);

- - - 查询 
select * from A1;
+------+------+
| id   | num  |
+------+------+
| a    |    5 |
| b    |   10 |
| c    |   15 |
| d    |   10 |
+------+------+

select * from A2;
+------+------+
| id   | num  |
+------+------+
| b    |    5 |
| c    |   15 |
| d    |   20 |
| e    |   99 |
+------+------+

# 要求是：查出来的结果如下：
+------+------+
| id   | num  |
+------+------+
| a    |    5 |
| b    |   15 |
| c   |    30 |
| d    |   30 |
| e    |   99 |
+------+------+

# 1、使用左连接
select A1.*,A2.*  from  A1 left join A2 on A1.id =A2.id;
+------+------+------+------+
| id   | num  | id   | num  |
+------+------+------+------+
| b    |   10 | b    |    5 |
| c    |   15 | c    |   15 |
| d    |   10 | d    |   20 |
| a    |    5 | NULL | NULL |
+------+------+------+------+
# 2、使用子查询
select  A1.num,A2.num from (select A1.*,A2.*  from   A1 left join A2 on A1.id =A2.id ) as temp ;


--- 换种思路
select * from A1 
union all
select * from A2;
+------+------+
| id   | num  |
+------+------+
| a    |    5 |
| b    |   10 |
| c    |   15 |
| d    |   10 |
| b    |    5 |
| d    |   20 |
| e    |   99 |
+------+------+
上面取出来了两个表的合并
# 再次进行分组
select id,sum(num) from (select * from A1 
union all # 这里必须不能去重
select * from A2) as temp group by id;
```

#### 函数

##### 一些常见的函数

```mysql
select 3-2;
+-----+
| 3-2 |
+-----+
|   1 |
+-----+
select bin(7); # 将7转成二进制
+--------+
| bin(7) |
+--------+
| 111    |
+--------+
select hex(28); # 转成16进制
+---------+
| hex(28) |
+---------+
| 1C      |
+---------+
# 对
```

###### abs（是绝对值）

```mysql
abs是绝对值
select abs(3-5); # abs是绝对值
+----------+
| abs(3-5) |
+----------+
|        2 |
+----------+
```

###### 对小数取整

```mysql
#舍弃取整 直接舍弃小数
select floor(23.37);
+--------------+
| floor(23.37) |
+--------------+
|           23 |
+--------------+
# 给第四个栏目下的商品表的价格 88 折
 select goods_name,floor(shop_price*0.88)  from goods where cat_id =4;
+-----------------+------------------------+
| goods_name      | floor(shop_price*0.88) | # 商品88折扣，在取整
+-----------------+------------------------+
| kd876           |                   1221 |
| 诺基亚5800xm    |                   2310 |
| 夏新t5          |                   2532 |
+-----------------+------------------------+
```

###### 向上取整

```mysql
select ceiling(3.01);
+---------------+
| ceiling(3.01) |
+---------------+
|             4 |
+---------------+
```

###### rang(随机数)

```mysql
# 随机产生一个0-1之间的小数
select  rand();
+----------------------+
| rand()               |
+----------------------+
| 0.000075901858579276 |
+----------------------+

#给每个商品随机生成一个5-15元之间的随机红包，购买的时候数据赠送。
select floor(rand()*10+5);

```

######  查看 ascii 码

```mysql
select ascii('a');
+------------+
| ascii('a') |
+------------+
|         97 |
+------------+
```

######  计算字符串的字节个数 length

```mysql
select length('中华民国');# utf-8 一个字占3个字节
+------------------------+
| length('中华民国')     |
+------------------------+
|                     12 |
+------------------------+
```

###### 计算字符长度 char_length

```mysql
 select  length('中华民国') as （中华民国）字节长度 ,char_length('中华民国') as （中华民国）字符长度;
+--------------------------------+--------------------------------+
| （中华民国）字节长度               | （中华民国）字符长度           |
+--------------------------------+--------------------------------+
|                             12 |                              4 |
+--------------------------------+--------------------------------+
```

###### 反转字符串(按照字符串反转) reverse

```mysql
select  reverse(goods_name) from goods;
```

###### 出现的位置 position in 

```mysql
# 如果出现
select position('@' in '111@333');
+----------------------------+
| position('@' in '111@333') |
+----------------------------+
|                          4 |
+----------------------------+
# 如果没有出现
select position('@' in '111333');
+---------------------------+
| position('@' in '111333') |
+---------------------------+
|                         0 |
+---------------------------+
```

###### 截取 right

```mysql
# 从右边截取 
select right('北京阿里巴巴',4);
+-------------------------------+
| right('北京阿里巴巴',4)       |
+-------------------------------+
| 阿里巴巴                      |
+-------------------------------+

```

###### 综合案例

```mysql
# 创建表
create table test14(
    name varchar(20),
    email varchar(20)
)engine myisam charset utf8;
# 插入数据
insert into test14 values
('张三','张三@163.com'),('lily','lily@126.com'),('mr gao','gao@emial.com');
# 取出每个人邮箱@的后缀
select * from test14;
+--------+----------------+
| name   | email          |
+--------+----------------+
| 张三   | 张三@163.com   |
| lily   | lily@126.com   |
| mr gao | gao@emial.com  |
+--------+----------------+

select name , right(email,char_length(email)-position('@' in email)) from test14;
# 解析
char_length(email)  #获取email 字符串的字符长度
position('@' in email) #获取email中@的位置

```

##### 日期时间函数

###### 当前日期时间 now() 

```mysql
select now();
+---------------------+
| now()               |
+---------------------+
| 2020-01-07 17:24:05 |
+---------------------+
```

###### 日期 curdate

```mysql
select curdate();
+------------+
| curdate()  |
+------------+
| 2020-01-07 |
+------------+
```

######  时间 curtime

```mysql
select curtime();
+-----------+
| curtime() |
+-----------+
| 17:26:39  |
+-----------+
```

 ###### 查询某日是哪一周的第几天 dayofweek

```mysql
# 当前是第几周
select dayofweek(now());
+------------------+
| dayofweek(now()) |
+------------------+
|                3 |#西方：周日为第一天的开始
+------------------+
# select dayofweek('2024-10-11 11:33:30');
----------------------------------+
| dayofweek('2024-10-11 11:33:30') |
+----------------------------------+
|                                6 |
+----------------------------------+
```

###### 查询第几周  week

```mysql
# 实例
create table jiaban(
    num int ,#加班的时长
    dt date#哪天加班 
)engine myisam charset utf8;

insert into jiaban values
(6,'2012-09-01'),
(7,'2012-09-02'),
(8'2012-09-03'),
(9,'2012-09-04'),
(10,'2012-09-05'),
(11,'2012-09-06'),
(12,'2012-09-07'),
(13,'2012-09-08'),
(14,'2012-09-09'),
(15,'2012-09-10'),
(16,'2012-09-11'),
(17,'2012-09-12');

# 按周统计加班数
select *,  week(dt) from jiaban;
+------+------------+----------+
| num  | dt         | week(dt) |
+------+------------+----------+
|    6 | 2012-09-01 |       35 |
|    7 | 2012-09-02 |       36 |
|    8 | 2012-09-03 |       36 |
|    9 | 2012-09-04 |       36 |
|   10 | 2012-09-05 |       36 |
|   11 | 2012-09-06 |       36 |
|   12 | 2012-09-07 |       36 |
|   13 | 2012-09-08 |       36 |
|   14 | 2012-09-09 |       37 |
|   15 | 2012-09-10 |       37 |
|   16 | 2012-09-11 |       37 |
|   17 | 2012-09-12 |       37 |
+------+------------+----------+
# 进行分组 并聚合
select  week(dt) as wk,sum(num) from jiaban group by wk;
+------+----------+
| wk   | sum(num) |
+------+----------+
|   35 |        6 |
|   36 |       70 |
|   37 |       62 |
+------+----------+
```

##### 加密函数

> 不可逆的、碰撞性低

```mysql
 select md5('123');
+----------------------------------+
| md5('123')                       |
+----------------------------------+
| 202cb962ac59075b964b07152d234b70 |
+----------------------------------+
```

##### 控制流函数

###### select case when then

```mysql
create table test15(
    name varchar(10),
    gender tinyint
)engine myisam charset utf8;
insert into test15 values
('张三',0),
('李四',1),
('春',2);

select * from test15;
+--------+--------+
| name   | gender |
+--------+--------+
| 张三   |      0 |
| 李四   |      1 |
| 春     |      2 |
+--------+--------+

# 要求判断 0女 1男 2春 
# 语法 select case when then 的使用
select name,
case gender
when 1
then '男'
when 0
then '女'
else '春'
end as 性别
from test15;

+--------+--------+
| name   | 性别   |
+--------+--------+
| 张三   | 女     |
| 李四   | 男     |
| 春     | 春     |
+--------+--------+

```

###### if 相当于三元表达式

```mysql
select name ,if(gender=0,'优先','等待') as 优先级 from test15;
```

###### ifnull

````mysql
select ifnull(2,1);
+-------------+
| ifnull(2,1) |  # 如果第一个不为null 那么返回第一个
+-------------+
|           2 |
+-------------+

select ifnull(null,1);
+----------------+
| ifnull(null,1) | # 如果第一个为null 那么返回后面的结果
+----------------+
|              1 |
+----------------+
````

##### 系统调试函数

###### user

 ```mysql
# 判断用户主机，判断自己的身份
select  user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
 ```

##### database

```mysql
#  正在操作的数据库
select database();
+------------+
| database() |
+------------+
| myGoods    |
+------------+

```

###### version

```mysql
# 查看数据库的版本
select version();
+-----------+
| version() |
+-----------+
| 5.7.28    |
+-----------+
```

#### 视图 view

###### 创建视图

> 在查询中我们经常把查询结果 当成临时表来看，view可以看出是一张虚拟的表。
>
> 可以将view看出一个虚拟表，来通过一系列运算得到的一个映射

```mysql
# 查询每个栏目下商品的平均价格，并取出 平均价前3高的栏目
 select cat_id, avg(shop_price) as av  from goods group by cat_id order by av desc limit 3;
+--------+-------------+
| cat_id | av          |
+--------+-------------+
|      5 | 3700.000000 |
|      4 | 2297.000000 |
|      3 | 1746.066667 |
+--------+-------------+
#创建一个视图
create view sta as 
select cat_id from goods
group by
cat_id;
# 查询这视图
select * from sta;
# 可以将该视图当成一个表 进行各种操作
```

###### 视图作用

> 1、简化查询
>
> 2、更精细的权限控制。
>
> 3、多表时候，分表使用

###### 视图的修改

> 如果视图的数据和表的关系是一一对应的，那么可以删除
>
> 如果视图的数据和表的关系不是一一对应的话，那么是不能删除的。

```mysql
# 取出最贵的前五条商品
create view guizhong as 
select goods_id,goods_name,shop_price
from goods 
order by shop_price desc 
limit 5;
# 这个时候是不能增删改查的。
```

###### algorithm

- merge

> 视图就是创建临时表。当简单的查询的时候，创建一个临时表（视图）开销有点儿大。这个时候我们可以指定`algorithm=merge`为true。这样不需要创建临时表，只是记录了条件。当使用这个视图的话，只需要合并条件就可以啦。

```mysql
create algorithm=merge view v2
as
select * from goods;
```

- TEMPTABLE

> 创建临时表

- UNDEFINED

> 不定义的，让系统自己来定

#### mysql 引擎

> 存储数据的不同方式

- myisam 

- innodb 
- 等

#### 事物

- 原子性

> 通俗称：要不都成功，要不都不成功。

- 一致性

> 数据库总是从一个一致性状态转换到另一个一致状态。我的理解就是 好几条语句，执行完了一个语句后，表数据不会变化，

- 隔离

> 通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。注意这里的“通常来说”，后面的事务隔离级级别会说到。

- 持久性

> 一旦事务提交，则其所做的修改就会永久保存到数据库中。此时即使系统崩溃，修改的数据也不会丢失。（持久性的安全性与刷新日志级别也存在一定关系，不同的级别对应不同的数据安全级别。）



```mysql
# 演示
# 先创建一个表
# engine 必须为 innodb 因为innodb支持事物，而myisam不支持事物 

#开始事物
START TRANSACTION;
# checking 表中 
SELECT balance FROM checking WHERE customer_id = 10233276;
# 给账号id为 10233276 的用户-200块
UPDATE checking SET balance = balance - 200.00 WHERE customer_id = 10233276;
# 给 savings 表 账号id为 10233276 的用户+200块
UPDATE savings SET balance = balance + 200.00 WHERE customer_id = 10233276;
# 事物提交 
COMMIT;
```

上面的`sql`很简单，给一个账号加200 再给另外一个账号-200

- 原子性

> 要么完全提交（10233276的checking余额减少200，savings 的余额增加200），要么完全回滚（两个表的余额都不发生变化）

- 一致性

> 这个例子的一致性体现在 200元不会因为数据库系统运行到第3行之后，第4行之前时崩溃而不翼而飞，因为事务还没有提交。

- 隔离性

> 允许在一个事务中的操作语句会与其他事务的语句隔离开，比如事务A运行到第3行之后，第4行之前，此时事务B去查询checking余额时，它仍然能够看到在事务A中被减去的200元（账户钱不变），因为事务A和B是彼此隔离的。在事务A提交之前，事务B观察不到数据的改变。











































