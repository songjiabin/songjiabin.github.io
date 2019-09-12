---
title: rpm包的管理
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">会挽雕弓如满月，西北望，射天狼。</blockquote>


<!-- more -->


> 一种用户互联网下载包的打包机安装工具。


##### rpm基本操作
```Java 
rpm -qa  
rpm -qa  | grep firefox  查询火狐 
rpm -qi firefox 查看火狐并且查看版本
rpm -qi firefox 查询软件的信息
rpm -ql  firefox 查询火狐安装的位置信息
rpm -qf /etc/passwd 查询文件属于那个软件包
```




##### 卸载rpm安装的火狐
```Java
rpm -e firefox 
rpm -e --nodeps firefox 强制删除
```


##### rpm安装火狐
```Java
i = install
v = verbose 提示
h = hash 进度条

1、找到rpm的火狐安装包 
2、/cd/media  
3、cd centos...
4、cd Packages/
5、cp firefox-45...   /opt  复制火狐的rpm安装包到/opt下
6、cd /opt                  
7、rpm -ivh fire...         进行安装
```

