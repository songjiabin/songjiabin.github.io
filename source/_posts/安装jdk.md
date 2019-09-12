---
title: 安装jdk
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">古今多少事，都付笑谈中。</blockquote>


<!-- more -->

##### 下载
>将压缩文件放到/opt下面
##### 解压到当前目录
`tar -zxvf jdk-7u79-linux-x64.gz`

#####  配置环境变量 
> vim /etc/profile  如下图：

![TIM截图20190821095823.png](https://upload-images.jianshu.io/upload_images/2953304-e6ec5650e660936a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 注销用户 才能生效
- 如果运行级别是3：`logout`
- 如果运行级别是5： 界面上直接注销，重新登录 




##### 试试java功能

```Java
cd /home 
mkdir java
cd /java 
vim hello.java

public class hello{
    public static  void main(){
        System.out.println("hello");
    }
}


javac hello.java
java hello  


```



