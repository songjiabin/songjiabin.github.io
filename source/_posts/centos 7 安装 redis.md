---
title: centos 7 安装 redis
tags: 
- linux
- git 
copyright: true
categories: linux
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![img](/images/baltic-sea-4136488__340.jpg)



<!-- more -->



##### 下载 

`wget http://download.redis.io/releases/redis-4.0.1.tar.gz`

##### 移动并解压

`mv redis-4.0.1.tar.gz /usr/local/redis/ `  如果没有`redis`文件夹，先进行创建

`cd /usr/local/redis`

`tar -zxvf redis-4.0.1.tar.gz`

##### 安装c语言编辑器gcc

`yum install -y gcc`

##### 跳转到解压目录下进行编译安装

`make MALLOC=libc`

`cd src && make install`

##### 测试是否安装成功

`cd src`

`./redis-server`

服务端已经开启，成功如下：

![1569404999812](/images/1569404999812.png)

参考链接：[这里](<https://blog.csdn.net/NathanniuBee/article/details/83274960>)

##### 以后台进程方式启动redis

###### 修改redis.conf文件（我先先复制到window，编辑后再覆盖之前的）

redis.conf文件就在redis目录下

![img](/images/20181022165046848.png)

- 将**daemonize no**修改为**daemonize yes**

  ![img](/images/20181022165132408.png)

- 配置允许**所有ip都可以访问**redis，将bind 127.0.0.1注释掉: 

![img](/images/20181022165208476.png)

- 并且将protected-mode   改为no

  ![img](/images/20181022165230627.png)

- 配置访问密码：

  ![img](/images/20181022165247183.png)


###### 指定redis.conf文件启动

`./redis-server /usr/local/redis/redis-4.0.1/redis.conf`

###### 关闭redis进程

首先使用ps -aux | grep redis查看redis进程

`$ ps -aux | grep redis`

![img](/images/20181022165345576.png)

使用`kill`命令杀死进程，并检查是否成功关闭

```golang
$  kill -9 5545
```

![img](https://img-blog.csdn.net/20181022165422882?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L05hdGhhbm5pdUJlZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###### 检查是否开启了所有Ip访问：

```golang
$ ps -ef |grep redis
```

如果端口号前面显示的是*****则说明客户端可以访问了，如果是127.0.0.1，继续配吧骚年，另外第6步配置了服务形式开启自启动，拷贝了一个6379.conf配置文件，记得做同样的修改配置，至于不改会出现什么样的坑，这个坑还是留给你踩吧，我就省事儿起见了 - _ -

##### 设置`redis`开机自启动

###### 在`/etc`目录下新建`redis`目录

###### 将`/usr/local/redis/redis-5.0.1/redis.conf`文件复制一份到`/etc/redis`目录下，并命名为6379.conf　　

```golang
cp /usr/local/redis/redis-5.0.1/redis.conf /etc/redis/6379.conf
```

###### 将`redis`的启动脚本复制一份放到`/etc/init.d`目录下

```golang
$ cp /usr/local/redis/redis-5.0.1/utils/redis_init_script /etc/init.d/redisd
```

###### 设置redis开机自启动

先切换到/etc/init.d目录下，然后执行自启命令

```golang
$ chkconfig redisd on
```

**如果redisd不支持chkconfig**如下：

使用vim编辑redisd文件，在第一行加入如下两行注释，保存退出

```golang
# chkconfig:   2345 90 10
 
# description:  Redis is a persistent key-value database
```

注释的意思是，redis服务必须在运行级2，3，4，5下被启动或关闭，启动的优先级是90，关闭的优先级是10。

再次执行开机自启命令，成功

```golang
$ chkconfig redisd on
```

#####  现在可以直接以服务的形式启动和关闭redis了

##### 启动：

```golang
$ service redisd start　　
```

##### 关闭：

```golang
$ service redisd stop
```

##### 备注 

`service redisd start`

如果出现下面的错误 `/var/run/redis_6379.pid exists, **process is already running or crashed**　`

看这里 [连接](http://blog.csdn.net/luozhonghua2014/article/details/54649295)

