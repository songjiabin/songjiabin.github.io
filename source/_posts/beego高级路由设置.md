---
title: beego高级路由设置
tags: 
- go
- beego
copyright: true
categories: go 
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![ä¸å¶è, æ¤ç©, ç"¿è², æ¤è¢", èæ¯, ç"¿å, èªç¶](/images/clover-1225988__340.jpg)

<!-- more -->

> 路由就是根据不同的请求，选择相应的控制器。

##### 普通的路由

```go
beego.Router("/", &controllers.MainController{},"get:GetFunc") //匹配get方法
beego.Router("/", &controllers.MainController{},"get,post:GetFunc")//匹配post方法
beego.Router("/", &controllers.MainController{},"get:GetFunc;post:PostFunc")//分别匹配get、post方法
```

##### 正则路由

######  ?  0个或者一个  当后面没有任何数据的时候也可以匹配

```go

beego.Router("/api/?:id",&controllers.ApiController{},"get:GetApi") 
获取`api`后面得到的数据用`id`来获取
s := c.GetString(":id")
```

###### 匹配数字 [0-9]+   

``` go
beego.Router("/num/?:id([0-9]+)",&controllers.NumController{},"get:GetNum") //只能匹配数字
```

######  匹配字母 [\\\\w]+ 

```go
beego.Router("/user/:id([\\w]+)", &controllers.UserNameController{}, "get:GetUserName") //只能匹配【0-9，a-z,A-Z】的组合
```

######  匹配    xxx.xxx   .前面的字符和后面的字符

```go
beego.Router("/download/*.*",&controllers.DownLoadController{},"get:DownLoadFile")
在后台接受的话是：
s := c.GetString(":path")   	 //接收匹配.前面  这里必须是path
	s2 := c.GetString(":ext") 	//接收匹配.后面   这里必须是ext
	beego.Info(s,s2)
	c.TplName = "1.html"
```

###### 全匹配的方式

```go
beego.Router("/all/*",&controllers.AllController{},"get:All") 
func (c *AllController) All()  {
	s := c.GetString(":splat")  //splat 固定 不能更改       
	beego.Info(s)            
	c.TplName = "1.html"
}
```



##### 复杂的路由

```go
beego.Router("/num/?:id(你复杂的路由)",&controllers.NumController{},"get:GetNum") 
```







