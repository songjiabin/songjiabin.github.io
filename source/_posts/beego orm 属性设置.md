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

 ##### 主键、自增

```mysql
Id  int `orm:"pk;auto"`  pk 主键   auto 自增 使用;进行分割  
```

##### 是否为空

```mysql
Title  string `orm:"null"` 默认情况下，不能为空。如果设置了null，那么就可以为空了
```

##### 大小

```mysql
Content string `orm:"size(200)"`  size(200) 大小是 200 
```

##### 时间类型

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

##### 默认值

```mysql
Count  int  `orm:"default(0)"` 默认值为0   
```

##### 唯一的值

```mysql
UserName string `orm:"unique"`
```

##### 浮点数精度

```mysql
Num float64 `orm:"digits(12);decimals(4)"` //  浮点型 一共12位数，小数点后面4位
```

##### 一对一设置

```go
rel(one)
reverse(one)
```

##### 一对多

```golang
rel(fk) //forign key 外键
reverse(mang) //多
例子：
ArticleType *ArticleType `orm:"rel(fk)"`  //外键  设置一对多的关系 -->表1
Article  []*Article `orm:"reverse(many)"` //一对多的反向关系       -->表2
```

##### 多对多

```golang
rel(m2m)
reverse(many) 
例子：
Article  []*Article `orm:"reverse(many)"` //多对多  -->表1
User        []*User      `orm:"rel(m2m)"` //多对多	 -->表2
```

#####  多表联合查询

```golang
查询Article表中中的外键ArticleType对应表的TypeName==se的所有类型
newOrm.QueryTable("Article").RelatedSel("ArticleType").Filter("ArticleType__TypeName", se).All(&articles)
RelatedSel 需要联合的表 
ArticleType__TypeName 根据联合表（ArticleType）中的（TypeName）字段进行查询

---

查询Article表中id为idInt的数据，并关联ArticleType表 
newOrm.QueryTable("Article").Filter("Id",idInt).RelatedSel("ArticleType").One(&article)

--- 
```

##### base

