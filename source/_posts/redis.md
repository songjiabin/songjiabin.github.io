---
title: redis
date: 2018-08-13 14:38:35
tags: 
- python
- redis 
- go 
copyright: true
categories: python
categories: go
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

requirepass 123456（我的密码是123456）

maxmemory 1073741824（内存是1G）~ 1024*1024*1024

#### 开启服务

开启终端定位到此目录下。运行。开启了服务
```
redis-server.exe redis.windows.conf   

```
![效果](https://upload-images.jianshu.io/upload_images/2953304-f5c1943ce822c8da.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
出现此图表示服务已经启动


#### 基本操作


##### 定位到刚才的目录下。打开客户端
```
redis-cli.exe 
```
##### 输入密码 
`auth 123456`


##### 存入数据 键值对的方式存储的。 
`set name songjiabin    `



##### 设置数据并设置气过期的时间
`setex wife 10 bingbing    设置wife bingbing 过期时间为10秒钟`





##### ttl 获取key的有效时间    
`ttl wife` 


##### mset 一次加入多个键值对
`mset wife bingbing  wife2 yaya `


##### mget 一次获取多个
`mget wife name wife2 `


##### incr 加1 将原来的数值进行加1
`incr age` 

##### decr  加1 将原来的数值进行减1
`decr  age` 



#####  incrby 将一个数值增加到具体的数字
`incrby age 20 ` 将age数值增加20 

##### decrby 将一个数值减少到具体的数字
`decrby age 20 `将age数值减少20 


##### append 追加
`append name study`



##### strlen  获取长度 
`strlen name`


--- 
#### key 操作



##### 查看所有的key
`keys *`

##### 查看所有已n开头的
`keys n*`


##### 查看此key是否存在  
`exists  name `


##### type 查看啥类型
`tpye name`


##### 删除key
`del name`

 
删除多个
`del name age `


##### expire  设置过期时间
`expire name 20`


##### persist 取消过期时间
`persist names`


##### 重命名
`rename name names`


##### 重命名 在确保新的名字 在库中不存在
`renamenx name names` //如果库中没有names那么才会修改成功



#### 哈希操作

##### 设置哈希person name 为songjiabin
`hset person name songjiabin`

##### 得到person name 值
``


##### 哈希设置多个值
`hmset person rmb 333333333 wife ruhua wife2 ciyu`

##### 哈希得到多个值
`hmget person rmb wife wife2`

##### 哈希得到所有的字段和值
`hgetall person`

##### 获取全部字段
`hkeys person`


##### 获取全部字段的值
`hvals person`



##### 获取里面的字段的数量
`hlen person`


##### 查看哈希里面是否存在某个键
`hexists person name` 查看person里面是否存在name这个属性



##### 删除哈希里面的键-值
`hdel person name `


#### list操作

##### 向左侧插入值
`lpush list 1`


##### 向右侧插入值
`rpush list 2`


##### 得到所有的值
`lrange list 0 -1` 从第0位到最后














 
#### http://redis.cn/commands.html 查看更多的api  