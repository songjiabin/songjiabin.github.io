---
title: beego datetime 时间少八个小时问题
date: 
tags: 
- go 
- beego
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![å¤§ä¼æ±½è½¦, å¤§ä¼æ¬ºè´, å¤§ä¼æ±½è½¦å¬å¸, åé©, æµ·, èå, æ¯è§, æ·å¤](/images/vw-bus-1845719__340.jpg)

<!-- more -->

##### 问题

> 在使用`beego orm `的时候，插入`datetime`的时候发现插入的时间比正常时间少八个小时

#####  解决方案

> 原因是是使用了错误的时区导致的。

![1568713482221](/images/1568713482221.png)

**在连接数据库的时候声明时区就好了**

```golang
time.Time `orm:"type(datetime);auto_now_add;null"` //创建时间 orm时候映射的字段
orm.RegisterDataBase("default", "mysql", "root:123root@A@tcp(xxx:3306)/mydb?charset=utf8&loc=Asia%2FShanghai")
&loc=Asia%2FShanghai 为定义的时区
```

