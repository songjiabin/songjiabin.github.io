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

##### 过滤路由 

######  注册路由过滤器 

- BeforeStatic     静态地址之前 
- BeforeRouter   寻找路由之前
- BeforeExec      找到路由之后，开始执行相应的Controller之前
- AfterExec         执行完Controller逻辑之后执行的过滤器
- FinishRouter    执行完逻辑之后执行的过滤器



```golang
//过滤router
beego.InsertFilter("/article/*", beego.BeforeRouter, filtFun)
 
 //当匹配的时候 router之前进行的操作
var filtFun = func(ctx *context.Context) {
	session := ctx.Input.Session("userName")
	if session == nil {
		url := ctx.Request.URL
		logs.Info("请求的url事：", url)
		ctx.Redirect(302, "/")
	}
}
```

######  Prepare 使用该方法 

> 定义`baseControllers` 使所有的`controllers`都继承它。在所有controllers都执行的方法里面，进行过滤

```golang
func (this *BaseController) Prepare() {
//所有的路由都会继承此方法
//首先执行的方法
this.IsLogin = false
	session := this.GetSession(SESSION_USER_KEY)
	logs.Info("session--->", session)
	if session != nil {
		//session是否为User对象
		if u, ok := session.(models.User); ok {
			this.User = u
			this.Data["user"] = u
			this.IsLogin = true
		}
	}
}
```







