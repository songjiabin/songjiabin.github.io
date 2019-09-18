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



**这里可以看到具体的文档 [传送门](<https://github.com/beego/beedoc/blob/master/zh-CN/mvc/view/tutorial.md>)**



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



