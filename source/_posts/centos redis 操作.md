---
title: centos redis 操作
- go
- redis
- linux
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



 ![日落, 风力发电机组, 天空, 性质, 景观, 风, 暮光之城, 能源, 生态](/images/sunset-4491838__340.jpg)

<!-- more -->

##### 进入客户端
```
cd /usr/local/redis/
cd redis-4.0.1/
cd src
./redis-cli
auth 123456 输入密码
```
##### 选择数据库
> 0到15个数据库，默认总共有16个
```
 select 0/1/.....15
```


##### 字符串的操作 
- 设置一个`key-value`
> 默认都是字符串格式的
```
set name sjb 
```
- 设置多个`key-value`
```
mset name sjb age 20
```
- 得到多个
```
mget name age 
```
- 设置`key-value`，多少时间失效
```
setex name 10 sjb //设置 name 为 sjb 。10秒钟内有效
```
- 追加操作
```
append name sjb  //在name后面追加 name
```
##### 键操作 

- `get name` 是中文的情况下，乱码问题

```
退出`exit`
./redis-cli -raw 重建进入
问题解决
```

- 查看所有的key

```
keys * 
```
- 通过正则查看 

```
keys *a  //查看a开头的key值 
```
- 查看key是否存在 

- 0:不存在 
- 1：存在
```
EXISTS name  //是否存在key name
```
- 删除

```
del a1  删除成功几个返回几 
```
- 给一个`key`设置有效的时间

```
EXPIRE name 10  给name添加有效时间为10秒
```
- 查看一个`key`还有多长时间

```
kkl name  //查看name 还有多少时间失效
```
- 查看`value`是什么类型的 

```
type name //查看name什么类型
```
##### 哈希类型操作

- 设置哈希类型值

```
hsetl person name sjb -- 设置 
hset person age 20
hget person age -- 获取
hget person name 
```
- 一次设置多个`key-value`

```
hmset person name sjb age 22 
hmget person name age --一次获取多个key-value
```
- 一次获取哈希的所有`key`

```
hkeys person  --获取哈希person中所有的key 
```
- 一次获取哈希的所有`value`

```
HVALS person  --获取哈希person所有的value  
```
- 删除哈希中的`key`

```
hdel person name     -- 删除哈希person中的name 
```

##### list操作数据

- lpush 从左向右插入

```golang
lpush l1 a b c d   --从左向右 向list l1 插入 a b c d 
```

- rpush 从右向左插入

```golang
rpush l2 a b c d  --从右向左插入数据  
```

- linset 从每个位置插入数据

```golang
LINSERT l1 before a a-1   -- 在l1中插入a-1。位置是在a之前 
```



- 遍历数据  

```golang
lrange l1  0 -1  --从左到又的读取  从第一个到最后一个
```

- 删除

```golang
lrem l1 1 a   -- 1（正数）代表从左到右的删除  删除一个 a
lrem l1 -2 b  -- -1(负数)代表从右到左的删除  删除两个 b
lrem l1 9 c   --  0(只要是c全部删除)
```

##### 集合操作

> 集合和`list`的区别就是。集合中没有重复的元素。 	 

- 添加元素

```golang
sadd s1 a1 a2 a3 a1 a2   --向集合中插入a1 a2 a3 不允许重复
```

- 获取集合元素

```golang
smembers s1      
```

- 删除其中的一个值

```golang
srem s1 a1     -- 删除集合 s1 中的 a1
```

- 有顺序的集合

```golang
zadd z1 1（权重值） a   2(权重值) b  -- 插入有权重的值 
```

- 查询

> 查询的时候根据权重排序 ，权重小的在前面

```golang
ZRANGE z1 0 -1   查询z1 。按照权重的大小排列 
zrangebyscore z1 10 30  按照权重查询 查询权重在 10~30之间
//根据权重查询
zscore z1  a      查询z1 中 a 的权重是多少
```

- 删除（集合删除）

```
zrem z1 1    删除z1中的1 
```

- 根据权重删除

```golang
 zremrangebyscore  z1 10 20   删除权重从 10 到 20 之间的数据
```

##### 备注下面几个图片便于理解，记忆：

![1569565170034](/images/1569565170034.png)



![1569565200192](/images/1569565200192.png)

![1569565414239](/images/1569565414239.png)

