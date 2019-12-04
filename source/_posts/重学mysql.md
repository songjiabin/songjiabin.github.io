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
|        gender         | char(1)  1代表一个字符。tinyint 一个字节也是可以的(0:女,1:男)。 |        |            |
|        weight         |                    tinyint unsigned(正数)                    |        |            |
|         birth         |                             date                             |        |            |
|    salary（工资）     |            decimal(8,2) 浮点型 最大值是999999.99             |        |            |
| lastlogin上次登录时间 |                           datetime                           |        |            |
|   info（跟人简介）    |                        varchar(1500)                         |        |            |

> 分析，上面的表。除了`username` 和`info`两个列其他都是定长的。当是定长的话，在查找的时候会提高查找的速度。这个时候我们可以将两个`varchar`类型的改为`char`类型，虽然可能会存储上浪费空间，但是提高了速度!具体看下

```mysql
varchar(20)--->char(20) 	浪费空间可以接受
varchar(1500)--->char(1500) 浪费的空间就有点儿多了
所以这个时候需要把 info 拿出来，单独成为一张表。用户经常操作的数据放到一张表，其他的放在表中去。
#
lastlogin 上次登录时间 使用datetime 不是很理想，在实际中不是
```



 



























