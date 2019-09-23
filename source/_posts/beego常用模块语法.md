---
title: beego常用模块语法
date: 
tags: 
- beego 
- go
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![1568788572743](/images/1568788572743.png)

<!-- more -->

> 在html中使用数据的一些方法



**这里可以看到具体的文档 [传送门](<https://github.com/beego/beedoc/blob/master/zh-CN/mvc/view/tutorial.md)**



##### for 循环

- 不会得到index 

```html
{{range .articles}}  循环.articles数据
    <tr>
        <td>{{.Title}}</td>
        <td><a href="#">查看详情</a></td>
        <td> {{.Time}}</td>
        <td>{{.Count}}</td>
        <td><a href="#" class="dels">删除</a></td>
        <td><a href="#">编辑</a></td>
        <td>财经新闻</td>
    </tr>
{{end}}
```

- 得到index,value

```html
{{range $index ,$val := .articles}}
            <a href="#">{{$index}}</a>
            <a href="#">{{$val.Title}}</a>
{{end}}
```

##### 格式化时间

当从代码中取出时间后显示这样：`2019-09-17 17:37:01 +0800 CST`  很明显不是我想要的格式，需要进行格式化。

```html
<td> {{.Time.Format "2006-01-02 15:04:05"}}</td>  出来后的结果就是我们想要的格式了 
2019-09-17 17:37:01
```

#####  在分页中遇到的知识点

具体看 [这里](<https://github.com/songjiabin/goproject/blob/master/goproject/src/sjbproject/controllers/article.go>)

###### 使用视图代码（在html中进行调用go代码）

```golang
<li><a href="showArticle?pageIndex={{.pageIndex | showPrePage }}">上一页 </a></li>
<li><a href="showArticle?pageIndex={{.pageIndex | showNextPage }}">下一页</a></li>

其中上面 | 表示 赋值的操作 意思是 将.pageIndex 带入到名字为 showPrePage的方法中去。

//计算上一页
func HandlePrePage(pageIndex int) (result int) {
	result = pageIndex - 1
	if result <= 0 {
		result = 1
	}
	return
}
//计算下一页
func HandleNextPage(pageIndex int) (result int) {
	result = pageIndex + 1
	return
}
```

##### if else  gt(大于)       lt(小于)

```golang
 {{if gt .pageIndex 1}}  gt 大于的意思  如果.pageIndex > 1 才会显示链接
      <li><a href="showArticle?pageIndex={{.pageIndex | showPrePage }}">上一页 </a></li>
 {{else}}
      <li><a href="#">上一页 </a></li>
 {{end}}
 --- 
 {{if lt .pageIndex .totalPage }} lt 小于的意思 如果.pageIndex < totalPage 才会显示链接 
      <li><a href="showArticle?pageIndex={{.pageIndex | showNextPage }}">下一页</a></li>
 {{else}}
      <li><a href="#">下一页</a></li>
 {{end}}
```

##### gt(大于)  lt(小于)  gte(大于等于)    lte(小于等于)

```golang
{{if gt .pageIndex 1}}
```



##### 等于compare、eq 

```golang
 {{if  compare .pageIndex 1 }} 如果.pageIndex==1 
     <li><a href="/showArticle?pageIndex=1">首页</a></li>
 {{end}}
 
 ---
 
 {{if  eq .pageIndex 1 }}
      <li><a href="/showArticle?pageIndex=1">首页</a></li>
 {{end}}
```

##### 不等于

```golang
{{if  ne .pageIndex 1 }} ne 为不等于
     <li><a href="/showArticle?pageIndex=1">首页</a></li>
{{end}}
```

##### template

> 引用代码

```golang
{{template "comm/header.html" .}} 引用代码到当前的位置 .  表示传递当前参数到子模板
```

##### Layout 

> 这个是`template`类型 

```golang
func (this *ShowLayoutContent) ShowLayoutContent() {
	//需要渲染的目标布局
	this.Layout = "layout.html"
	//将当前的布局渲染到目标布局
	this.TplName="main.html"
}

---

目标目标为layout.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>后台管理页面</title>
    <link rel="stylesheet" type="text/css" href="/static/css/reset.css">
    <link rel="stylesheet" type="text/css" href="/static/css/main.css">
    <script type="text/javascript" src="/static/js/jquery-1.12.4.min.js"></script>

</head>
<body>

<div class="header">
    <a href="#" class="logo fl"><img src="/static/img/logo.png" alt="logo"></a>
    <a href="#" class="logout fr">退 出</a>
</div>

<div class="side_bar">
    <div class="user_info">
        <img src="/static/img/person.png" alt="张大山">
        <p>欢迎你 <em>李雷</em></p>
    </div>

    <div class="menu_con">
        <div class="first_menu active"><a href="javascript:;" class="icon02">文章管理</a></div>
        <ul class="sub_menu show">
            <li><a href="#" class="icon031">文章列表</a></li>
            <li><a href="#" class="icon032">添加文章</a></li>
            <li><a href="#" class="icon034">添加分类</a></li>
        </ul>
    </div>
</div>

{{.LayoutContent}}   注意这里就是需要将其他布局引入的地方。必须这样写

</body>
</html>

```

##### LayoutSections

> 当给`layout.html`中设置局部的信息的时候。比如每个界面都有自己的标题，这个时候需要从每个`controllers`中给`layout.html`发送特定的`title`。这个时候就用到了`LayoutSections`

```golang
1、layout.html 
<head>
   ...
    {{.title_text}} 这里就是每个界面要显示不同的信息
   ...
</head>

2、title_text.html 重新建立
<title>{{.title}}</title>

3、LayoutSections 局部中设定
func (this *ShowLayoutContent) ShowLayoutContent() {
	this.Layout = "layout.html"
	this.LayoutSections = map[string]string{} 
	this.LayoutSections["title_text"] = "title_text.html" //定义html
	this.Data["title"]="首页"
	this.TplName = "main.html"
}
```



##### $.注意的地方（在range中使用控制器中的变量时）

当在`range`的代码中使用`.`的时候，注意**如果想使用控制器里面的代码需要使用$**

```golang
 <form method="post" action="/showArticle" id="select_submit">
            <select name="select" id="select" class="sel_opt">
                {{range  .articleTypes}}
                    {{if compare .TypeName $.CurrentTypeName}}
                        <option selected="true">{{.TypeName}}</option>
                    {{else}}
                        <option>{{.TypeName}}</option>
                    {{end}}

                {{end}}
            </select>
            <input type="submit" hidden="hidden">
        </form>
```

上面代码中，在`range`中想使用控制其中的`CurrentTypeName`，这个时候需要用到`$.`来引入。

##### Cookie的注意点

> 将数据保存到浏览器，可以设置储存的时长，一般用于记录账号、是否保存等不敏感的信息。注意**保持的数据不能是中文**

```golang
//保持cookie
check := this.GetString("remember")
 if check == "on" {
 	//记住用户名
 	this.Ctx.SetCookie("userName", userName, time.Second*100, "/") 
 } else {
 	this.Ctx.SetCookie("userName", "", -1, "/")
 }
 
//获取cookie 	
 cookie := this.Ctx.GetCookie("userName")
 logs.Info("cookie---->", cookie)
 if cookie != "" {
 	this.Data["userName"] = cookie
 }
```

##### Session 和 路由过滤器

> 存储用户信息，beego默认把数据存在后台 内存中。临时存储数据，如果浏览器关闭，session数据失效。当在浏览器上

