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

![Maślaki, 蘑菇, 帽子, 苔藓, 森林, 天空, 植被, 性质](/images/maslaki-4496903__340.jpg)

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

#####  多表联合查询（一对多）

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

##### 多对多插入操作

```golang
//多对多插入数据
//获取多对多的操作对象
m2m := newOrm.QueryM2M(&article, "User")  //article 为其中一个表。 User（字段名） 为其中的多对多字段
就是下面的实体类：
type Article struct {
	User        []*User      `orm:"rel(m2m)"`                         //多对多
}

//根据session获取用户
sessionUserName := this.GetSession("userName")
user := models.User{UserName: sessionUserName.(string)}
newOrm.Read(&user, "userName")
_, e = m2m.Add(&user)     //添加文章所对应的用户
if e != nil {
	logs.Info("添加多对多的时候失败", e)
	return
}

```

##### 多对多查询

- 第一种方法（`LoadRelated`）

```golang
//查询多对多
//这个文章都被哪些用户看过
newOrm.LoadRelated(&article, "User")   //查询对应该文章下所有的用户记录

newOrm.LoadRelated(&user, "Article")   //查询该用户下所有的文章记录
	for _, v := range user.Article {
		logs.Info("该用户下所有的文章", v.Id, v.Title)
	}
```



-  第二种方法（`QueryTable`）

```golang
//查询所有的用户 根据文章
users := []models.User{}
newOrm.QueryTable("User"). //指定查询的表
	Filter("Article__Article__Id", id). //字段名__表名__查询的字段
	Distinct(). //去重
	All(&users)
this.Data["article_users"] = users
logs.Info("用户--->", users)
```



