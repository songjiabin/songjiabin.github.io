---
title: curl网络请求
tags: 
- linux
- curl
- go 
copyright: true
categories: curl
---

![摩天轮, 西雅图, 华盛顿, 市中心, 车轮, 西北, 天际线, 海滨, 摩天](../images/ferris-wheel-4476991__340.jpg)

<!-- more -->



> 准的 Linux 发行版都安装了 curl工具。curl可以很方便地完成对 REST API 的调用场景，比如：设置 Header，指定 HTTP 请求方法，指定 HTTP 消息体，指定权限认证信息等。通过 `-v` 选项也能输出 REST 请求的所有返回信息。curl功能很强大，有很多参数，这里列出 REST 测试常用的参数：

```golang
-X/--request [GET|POST|PUT|DELETE|…]  指定请求的 HTTP 方法
-H/--header                           指定请求的 HTTP Header
-d/--data                             指定请求的 HTTP 消息体（Body）
-v/--verbose                          输出详细的返回信息
-u/--user                             指定账号、密码
-b/--cookie    
```

##### 简单使用

典型的测试命令为：

```golang
$ curl -v -XPOST -H "Content-Type: application/json" http://127.0.0.1:8080/user -d'{"username":"admin","password":"admin1234"}'
-	-	-
curl   -XGET  http://127.0.0.1:8080
```

##### 高级使用

未完待续...