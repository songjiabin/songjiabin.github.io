---
title: 安装mysql
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">江南可采莲，莲叶何田田，鱼戏莲叶间。</blockquote>


<!-- more -->
> 前几天设置了一个静态的ip，今天发现无法上网了。网上找了很久，原来是网关设置错误了。

```Java
点击vm->编辑->虚拟网络编辑->NAT设置->记录下 网关地址
```
![TIM截图20190821175143.png](https://upload-images.jianshu.io/upload_images/2953304-7dc58b5f21f7dd49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


修改虚拟的ip和网关
```Java
vim /etc/sysconfig/network-scripts/ifcfg-eth0 
```
![TIM截图20190821175855.png](https://upload-images.jianshu.io/upload_images/2953304-0ee255db9d616f8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


修改dns网关地址
```Java
vim /etc/resolv.conf  
如下图
```



![TIM截图20190821180230.png](https://upload-images.jianshu.io/upload_images/2953304-a566fefc0c87e0a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



重启服务
```Java
service network restart 
```


再次上网成功！