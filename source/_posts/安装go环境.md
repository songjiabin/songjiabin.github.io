---
title: 安装go环境
tags: 
- linux
copyright: true
categories: linux
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![æ , èç¸, äº, è, æ¯è§, æ·å¤, é£æ¯, å¹³é, å®é](/images/tree-736888__340.jpg)

<!-- more -->

##### 下载文件

`<https://golang.org/dl/>`

##### 解压

将文件传输到`/opt`下，解压`tar -zxvf go1.13.linux-amd64.tar.gz ` 到当前文件下。

##### 创建go文件目录

`mkdir /home/go`

##### 配置环境变量如下

![1568181827834](/images/1568181827834.png)

##### 检测是否安装成功

`go version` 

结果为：`go version go1.13 linux/amd64`