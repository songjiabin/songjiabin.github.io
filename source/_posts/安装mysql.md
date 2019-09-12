---
title: 安装mysql
tags: 
- linux
- mysql 
copyright: true
categories: linux
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![ä¸éµ, å°åº¦, èªç¶, ç§ä"£å¡é£, æ , æ¯è§, æ¯åº, æ³°ç±³å°çº³å¾·é¦, é£æ¯, æé´, ç"¿è², æ·å¤, ä¸å](/images/hills-2836301_960_720.jpg)

<!-- more -->




##### 查询是否有mysql
```Java
rpm -qa | grep mysql 
```

##### 如果有那么进行删除
```Java
rpm -e mysql-libs
```
##### 强制删除，如果有依赖关系的话
```Java
rpm -e --nodeps  mysql-libs
```
##### 安装gcc包
- 因为安装的事源码包，所以需要gcc的环境，所以安装gcc包
```Java
yum -y install make gcc-c++ cmake bison-devel ncurses-devel
```
##### 安装mysql
```Java
cd /opt
tar -zxvf mysql-5.6.14.tar.gz  解压mysql 
cd mysql-5.6.14
因为是源码包，所以需要编译安装
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data *************/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci

(在家里的电脑上报错。用这个：------>
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/servers/mysql -DMYSQL_UNIX_ADDR=/usr/local/servers/mysql/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DMYSQL_DATADIR=/usr/data/mysqldata -DMYSQL_TCP_PORT=3306 -DENABLE_DOWNLOADS=1
)



编译并安装
make && make install 
为mysql添加一个用户,并创建一个用户组
groupadd mysql
useradd -g mysql mysql 创建一个用户mysql，用户组为mysql 
在 /usr/local 下面生成了 mysql （家里的电脑在/usr/local/service）
修改该mysql的权限  所有者、所在组为mysql:

chown -R mysql:mysql mysql/

初始化mysql :
scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql


```

##### 启动mysql
> 在启动mysql的时候，会按照一定的次序搜索my.cnf。先在/etc目录找，找不到则会搜索"$basedir/my.cnf"。而“$basedir/my.cnf”就是"/usr/local/mysql/my.cnf"。这个是mysql的配置文件的位置。

**注意：因为在centos6.8上，操作系统安装后，在/etc目录下会存在一个my.cnf。所以默认会去读取这个文件，干扰mysql正确配置。所以我们只需要将/etc/my.cnf改下名字就好了。
mv /etc/my.cnf /etc/my.cnf/bak**


```
cd /usr/local/mysql/   --->定位到该目录下
cp support-files/mysql.server  /etc/init.d/mysql  copy 文件到/etc/init.d/mysql
chkconfig mysql on     --->设置开机自启（不管在哪里运行级别）

```


##### 手动开启
```Java
service mysql start 

cd /usr/local/mysql/bin
./mysql -u root 
现在还没有密码 都是空 一路回车就进入了mysql 。
进入到mysql中设置新的密码：
set password = password('root');
```

##### 设置环境变量 从而在任何地方都可以进行mysql
```
vim /etc/profile
```


![img](/images/2953304-83f7cd4aa3d6a506.png)

**可以注销重新启动，也可以刷新下命令`source /etc/profile` 就可以进行到mysql了**



- - -
##### centos 7 安装
- 下载mysql源安装包
`wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm`
- 安装mysql源码
`yum localinstall mysql57-community-release-el7-8.noarch.rpm`
##### 安装mysql
` yum install mysql-community-server`
##### 启动mysql
`systemctl start mysqld`
##### 查看mysql启动状态
`service mysqld status`
> 显示 active(running)代表服务启动正常。
##### 设置开机启动
```Java
systemctl enable mysqld
systemctl daemon-reload
```
##### 修改root默认密码
```Java
安装完成后会分配一个原始的密码
查看
grep 'temporary password' /var/log/mysqld.log
登入修改密码
mysql -u root -p
set password = password('123root@A'); -注意：mysql5.7默认安装了密码安全检查插件，密码必须包含：大小写字母，数字和特殊符号，并且长度不小于8位。

```
##### 配置默认编码
修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置，如下：
```Java
character_set_server=utf8
init_connect='SET NAMES utf8'
```

