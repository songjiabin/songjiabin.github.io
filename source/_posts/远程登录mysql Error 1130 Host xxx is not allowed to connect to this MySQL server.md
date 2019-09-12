---
title: 远程登录mysql Error 1130 Host xxx is not allowed to connect to this MySQL server
tags: 
- linux
- mysql
copyright: true
categories: mysql
typora-copy-images-to: ..\images
typora-root-url: ..
---

![ééé¦, é²è±, çº¢è², ç½, æ¥å¤©, ç¾å­¦, èè², é¢è², æ¤ç©ç¾¤, èæ¯](/images/tulips-65036__340.jpg)

<!-- more -->

> 在使用golang的连接腾讯云远程数据库的时候，报错：`Error 1130: Host xxx is not allowed to connect to this MySQL server`  原因是没有权限登录这个数据库。解决的方案是，通过root用户登录系统，进入数据库。更改更改`mysql`中的`user`表里的`host`项。



##### 选择mysql库

```go
use mysql;  
```



##### 查看mysql库中的user表的host值（即可进行连接访问的主机/IP名称）

```go
select 'host' from user where user='root';  
```



#####  修改host值（以通配符%的内容增加主机/IP地址），当然也可以直接增加IP地址

```go
update user set host = '%' where user ='root';  
```



#####  刷新MySQL的系统权限相关表

```go
flush privileges;  
```



##### 再重新查看user表时，有修改。。

```go
select 'host'   from user where user='root'; 
```



##### 重启mysql服务即可完成 