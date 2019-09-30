---
title: go 持久化数据到redis
tags: 
- go 
- redis
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![月亮, 全, 天空, 蓝色, 晚上, 天文学, 空间, 月球, 性质, 望月](/images/moon-4496622__340.jpg)

<!-- more -->

> 在`golang`中使用`redis`操作数据的时候，打算将从数据库查询的实体类保存到`redis`中。这个时候就用到了数据的持久化了 。

##### 初始化`redis`

```golang
func connReidis() {
	RedisClient = &redis.Pool{
		MaxIdle:     16,                //最大的连接数量
		MaxActive:   0,                 //接池最大连接数量,不确定可以用0（0表示自动定义），按需分配
		IdleTimeout: 300 * time.Second, //超时时间 连接关闭时间 300秒 （300秒不使用自动关闭）
		Dial: func() (conn redis.Conn, e error) {
			con, err := redis.Dial(config.RedisConfig["type"], "xxx:6379")
			if err != nil {
				return nil, err
			}

			if _, errAuth := con.Do("auth", config.RedisConfig["auth"]); err != nil {
				return nil, errAuth
			}
			return con, nil
		},
	}
}

```

##### 将数据插入到`redis`

```golang
//定义存储数据的容器
var buffer bytes.Buffer
encoder := gob.NewEncoder(&buffer) //进行编码
encoder.Encode(articleTypes)//将article编码成字节数据
//将字节数据存储到redis 中
_, err = rc.Do("set", "article_type", buffer.Bytes()) 
if err != nil {
	logs.Info("redis数据库操作错误", err)
	return
}
```

##### 获取`redis`中的数据

```golang
//从redis中读取数据  并转出成字节数组
byte, err := redis.Bytes(rc.Do("get", "article_type"))
decoder := gob.NewDecoder(bytes.NewReader(byte)) //解码器
articleTypes := []models.ArticleType{}
decoder.Decode(&articleTypes) //将数据赋到定义的对象中
```



