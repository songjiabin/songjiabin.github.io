---
title: 安装tomcat
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">十年磨一剑，霜刃未曾试。</blockquote>


<!-- more -->


##### 解压
```Java
cd /opt
tar -zxvf apache-tomcat-7.0.70.tar.gz 解压到当前目录
```

#####  进入到tomcat里面启动
```Java
cd apache-tomcat-7.0.70
cd /bin 
./startup.sh 开启  
在本地火狐查看：http:localhost:8080 
```

##### 在window 查看
> 在window开始

```Java
telnet  192.168.188.22  8080  尝试连接ip下的端口 发现失败 
原来，在centos中防火墙没有开发对应的端口号
```

##### 在centos上开发端口
```Java
进入 /etc/sysconfig/iptables 中进行编辑
vim /etc/sysconfig/iptables 
如下：
```
![TIM截图20190821134115.png](https://upload-images.jianshu.io/upload_images/2953304-b6b4e5a19f0f782b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 重新启动防火墙
```
service iptables restart 
```
##### 再用window连接
```Java
telnet ip 端口  
```