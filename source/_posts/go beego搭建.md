
---
title: go beego搭建
date: 
tags: 
- go 
- beego 
copyright: true
categories: go
---



<blockquote class="blockquote-center">我亦为君饮清酒，君心不肯向人倾。</blockquote>

<!-- more -->


##### get 安装beego  
`go get -u github.com/astaxie/beego`

`go get -u github.com/beego/bee`


> 这个时候再gopaht目录下 /bin/ 下会生成一个bee.exe文件。继续将这个文件的路径添加到环境变量的path下面。


##### 查看是否安装成功
打开doc命令，输入bee 
`bee`


![图片这个的](https://upload-images.jianshu.io/upload_images/13925206-72cdea0e13fa017b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/758/format/webp)

表明安装成功！


##### 新建beego项目

new命令是新建一个 Web 项目，在命令输入`bee new <项目名>`，比如我们输入命令`bee new myapp`


![360截图20190719094508886.jpg](https://upload-images.jianshu.io/upload_images/2953304-07935e817dad7c05.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


此时在`gopath/src`就生成了你的web项目

##### 运行项目
定位到你的项目路径下 比如 `gopath/src/myweb` ,运行 `bee run` 


![360截图20190719094508886.jpg](https://upload-images.jianshu.io/upload_images/2953304-4fccba910e318b7e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 浏览器查看
`http://localhost:8080/`

![TIM截图20190719095431.png](https://upload-images.jianshu.io/upload_images/2953304-c2fa9b3ae41aa44e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


