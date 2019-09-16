---
title: beego orm属性设置
date: 
tags: 
- beego 
- go 
copyright: true
categories: beego
toc: true

 
 
typora-copy-images-to: ..\images
typora-root-url: ..
---

![æ¬ä¹¦, è¯"å, æ¾æ¾, èå°, ä¹¦é¡µ, æè², å¾ä¹¦, å­¦ä¹ , æå­¦, éè](/images/book-2304389__340.jpg)

<!-- more -->



> `beego`属性的设置

 ###### 主键、自增

```mysql
Id  int `orm:"pk;auto"`  pk 主键   auto 自增 使用;进行分割  
```

###### 是否为空

```mysql
Title  string `orm:"null"` 默认情况下，不能为空。如果设置了null，那么就可以为空了
```

###### 大小

```mysql
Content string `orm:"size(200)"`  size(200) 大小是 200 
```

###### 时间类型

- datetime 日期时间类型

```mysql
Time  time.Time `orm:"type(datetime);auto_now_add"` 
type(datetime)日期时间类型。
auto_now_add:第一次保存时候的时间。  
auto_now:model保存时都会对时间自动更新      
```

- date 日期类型

```mysql
Time  time.Time `orm:"type(date);auto_now_add"` 设置Time类型为日期类型。第一次保存设置时间
type(datetime)时间类型。
auto_now_add第一次保存的时候的时间。  
auto_now:model保存时都会对时间自动更新     
```

###### 默认值

```mysql
Count  int  `orm:"default(0)"` 默认值为0   
```

###### 唯一的值

```mysql
UserName string `orm:"unique"`
```

###### 浮点数精度

```mysql
Num float64 `orm:"digits(12);decimals(4)"` //  浮点型 一共12位数，小数点后面4位
```

