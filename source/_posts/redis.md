---
title: redis
date: 2018-08-13 14:38:35
tags: 
- python
- redis 
copyright: true
categories: python
---

<blockquote class="blockquote-center">最可怕的敌人，就是沒有坚强的信念</blockquote>
<!-- more -->


#### 基本操作步骤

#### 下载redis window 

https://github.com/ServiceStack/redis-windows

#### 找到响应的版本
../download/redis64-2.8.2101.zip
并解压到其他目录中
找打 redis.windows.conf 文件对其进行修改

在此文件中的 387 加入 requirepass 123456（我的密码是123456）

在此文件中的 455 加入 maxheap 1024000000（我的密码是123456）

#### 开启服务

开启终端定位到此目录下。运行。开启了服务
```
redis-server.exe redis.windows.conf 命令 

```
![效果](https://upload-images.jianshu.io/upload_images/2953304-f5c1943ce822c8da.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
出现此图表示服务已经启动


#### 连接数据库  

##### 定位到刚才的目录下。
```
redis-cli.exe 
```
##### 输入密码 
auth `123456`


##### 存入数据 键值对的方式存储的。 
set name songjiabin    





