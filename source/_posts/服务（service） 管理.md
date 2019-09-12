---
title: 服务（service） 管理
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">独倚危楼，不信人间别有愁。</blockquote>

<!-- more -->



> 服务本质是进程，但是运行在后台的。通常会监听某个端口，等待其他程序的请求。因此我们称之为守护进程。



##### service服务
 - 查看防火墙状态
 ```Java
 service iptables status 
 ```
 - 停止防火墙
 ```Java
 service iptables stop 
 ```
 - 开启防火墙 **这种开启方式只会暂时开启，当系统重启后失效**
 ```Java
 service iptables start 
 ```
 
 

**可以试试用telnet ip 端口 查看端口是否可用**



##### 查看所有的服务名
- `setup` 进入到系统服务中就可以看到
- 查看 /etc/init.d/


##### chkconfig 给每个服务的各个运行运行级别设置自启动/关闭 （**永久生效**）

- 查看所有的服务在各个运行级别中是否自启动
```
chkconfig --list | grep  ssh
如下图
```


![TIM截图20190820113432.png](https://upload-images.jianshu.io/upload_images/2953304-88f798871961fabc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 查看具体的某个服务在各个运行级别中是否自启动
```Java
查看防火墙在各个运行级别中的状态
chkconfig iptables --list 
```

- 修改某个服务在某个状态下是否自启动 off/on 
```Java
修改 ssdh 在运行级别 5 上，自启动为：关闭状态
chkconfig --level 5 sshd off
当开启了以后，可以重启，或者执行service sshd start 进行开启
```



##### 案例
>在所有运行级别下关闭防火墙
```Java  
chkconfig iptables off   不加level 就可以   
```