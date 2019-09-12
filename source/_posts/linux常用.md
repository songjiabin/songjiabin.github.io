---
title: linux常用
date: 
tags: 
- linux
copyright: true
categories: linux
---



<blockquote class="blockquote-center">我花开后百花杀</blockquote>

<!-- more -->




##### 查看是否开启了远程端口，从而可以进行远程连接
> 要开始远程登录的话，需要开启sshd

###### 查看是否开启远程服务

```
ps -e |grep ssh
```
如果成功了，会出现：


![TIM截图20190812140124.png](https://upload-images.jianshu.io/upload_images/2953304-669e8b3861d09178.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果没有出现，那么进行安装：

```Java
sudo apt-get install openssh-server
```
再次运行第一次命令



##### 帮助命令
```Java
 ls -a 
```


##### 常用命令
- 目录的操作
```Java 
ll -h  -->查看并标出大小
ls     --->列出目录或者文件
ls -l  --->以列表的方式
ls -a  --->包括隐藏文件全部显示
mkdir A -->在当前目录下创建新文件夹
pwd    --->得到当前所在的目录
su - use1 --->从高权限切换到低权限的用户
exit  -->当在一个低权限的用户的时候，立马回到高权限中去
cd ~  或者    cd     --->回到家目录
mkdir new  -->当前目录下创建new
mkdir -p new/new2/new3 -->在当前目录下创建多个目录
rmdir  /root/new  --->删除一个目录(必须删除的事空目录)
rm -rf  /root/new   -->强制删除一个文件或者一个文件夹  
cat /etc/passwd     --->查看用户列表
cat /etc/group      --->查看用户组列表
每当安装了一个软件后，默认会放到/usr/local下面的目录下，比如安装了mysql,在usr/local 下面会有/usr/local/mysql 的目录 
chown -R mysql:mysql /usr/local/mysql ---->将该目录下的所有文件的所属人和分组都改为mysql 
```
- 文件的操作
```Java
touch a.txt  --->创建一个空文件
toucch a.txt b.txt --> 一次可以创建多个文件
cp  a.txt /root/new/ -->拷贝当前目录下的a.txt到 /root/new/目录下
cp  -r go/ /roo/new/  -->靠谱当前目录下的go文件到 /root/new目录下。
\cp -r go/ /roo/new/ -->强制覆盖内容 拷贝目录到指定地址去 
rm   a.txt     --->删除一个文件
rm -f /root/a.txt -->不给提示，删除一个文件（只能删除一个文件）
rm -rf /root/go  -->删除一个文件夹或者一个文件（强制） -f(不想让他提示)  
mv  a.txt b.txt  -->将a.txt 重命名为b.txt (在一个文件目录下操作)
mv  /root/a.txt  /home/b.txt  -->将/root/a.txt 文件移动到 /home/b.txt下，并改名为b.txt
cat a.txt   --->查看问文件a.txt(只读的方式)
cat -n a.txt --->显示行号的只读查看 
cat -n /etc/profile | more  --->带上行号查看并且分页查看 [more] 按空格 是换页 
more /etc/profile  -->使用more查看(空格下一页，回车下一行，ctr+b上一页,ctr+f下一页)
less /etc/profile 查看大文件的时候使用 
```



- \>指令      >>指令
```Java
>       --->追加覆盖
>>      --->向内容后面添加 
ls > a.txt  --->将ls显示的内容覆盖写入到a.txt中（a.txt如果没有则创建）
ls >> a.txt   --->将显示的内容追加到a.txt中去
```



- cat、echo 、tail 
```Java
cat /etc/profile > /root/go/1.txt   --->将一个文件的内容加入到指定的文件中
echo "hello world!" > /etc/go/a.txt  --->将数据写入到指定的文件里面 
echo $PATH   --->输出当前环境变量路径
head /etc/profile    --->查看文件（/etc/profile）的头部(默认是10行)
head  -n5 /etc/profile   --->查看文件的默认前5行   
tail  /etc/profile   --->查看文件的后10行 
tail -n 5 /etc/profile    -->查看文件的后5行
tail -f  /etc/profile   --->实时追踪该文档的所有更新的内容(当该内容更新的时候，会实时的显示)
```

- 软连接 
```Java
ln -s /home goToHome   --->在当前目录下创建链接goToHome，并指向/home目录。当cd goToHome的时候 展示的是/home下的文件 

rm -rf goToHome     --->删除软连接    
```


- 查看历史命令
```Java
histroy   -->查看以前的命令
history 10  --->查看最近的10条命令
当显示了历史命令后，如果想执行那么!行号 例如：！22 执行你执行过的第22个命令

```






- cal 日历
```Java
cal 当前日历的信息 
cal 2019  显示2019年所有的日期信息
```

-  date 
```Java
date    ---->得到日期数据
date "+%Y-%m-%d"    ---> 2019-08-15
date "+%Y年%m月%d日 %H时%M分%S秒"  --->获取年月日，时分秒   2019年08月15日 22时31分39秒 
date -s "2019-08-15 15:10:00"  ---->设置系统的时间为指定日期
date  "+%Y"   ---->获取年的信息    2019 
```




- vim 快捷键
```Java
vim hello.java //创建并写入java
i ->写
esc -> 退出写的模式
wq ->  写并且保存退出
q！-> 强制退出，不保存修改的内容
q -> 退出
5yy ->复制从当前开始的5行
p ->粘贴
dd ->删除当前行
2dd ->当当前行开始删除下面的2行
/hello ->查询hello这个单词
n ->当查询某个单词时候，n表示查询到的下一个
set nu ->显示行号
set nonu ->取消显示行号
G ->定位到最下面
gg ->定位到最下面
u  ->撤销
5 shit+g ->快速定位到某行（第五行）中去
```


##### 搜索 查找相关指令 
- find 
```Java
find /root/  -name images.png   -->在/root目录下，查找名字为 image.png的文件
find /root   -name *.png        -->在/root下，查找名字后缀为.png的文件
find /root   -user root         -->查找指定目录下，所有属于用户root的目录和文件
find /   -size +20M         --->查看系统中所有大于20M的文件 
find /root  -sze   20M          --->查找指定目录下等于20m的文件
```

- locate 
> 可以快速定位大文件路径。locate指令利用事先建立的系统所有文件及其路径的locate数据库实现快速定位给定的文件。locate指令无需遍历整个文件系统，查询速度较快，为了保证查询结果的准确度，管理员必须定期更新locate 

**因为locate指令基于数据库进行查询，所以第一次运行前必须使用updatedb指令创建locate数据库**
- [x] 首先执行`updatedb` 
- [x] 进行查询`locate abc.txt`  




- grep  过滤
```Java
cat /roo/go/a.txt  | grep  abc  --> 查询目标路径中 字段为 abc 的信息

cat /roo/go/a.txt  | grep  abc -n  --> 查询目标路径中 字段为 abc 的信息 ,并显示行号


cat /roo/go/a.txt  | grep  abc -n -i --> 查询目标路径中 字段为 abc(忽略大小写) 的信息 ,并显示行号

```
 
- 压缩 解压缩 
###### gizp、gunzip 
```Java
gzip    --->用于压缩文件（只能压缩为*.gz文件）压缩后 此前文件没有
gunzip  --->解压缩文件 
gzip /home/go/abc.txt   --->将指定文件压缩成.gz文件
```
######   zip 、unzip
```Java
zip -r myPackage.zip /home      ----->将目录/home压缩成myPackage.zip 文件
unzip  myPackage.zip     --->将压缩包进行解压 解压到当前目录下
upzip -d /root/new myPackage.zip   -->将当前目录下的 myPackage.zip解压到 /root/new 下面  
```


###### tar 既可以压缩也可以解压
-  -c 产生.tar 打包文件
-  -v 显示详细信息
-  -f 指定压缩后的文件名
-  -z 打包同时压缩
-  -x 解压.tar文件


```Java
 tar -zcvf a.tar.gz a.txt b.txt  --> 将改目录下的 a.txt b.txt 压缩成a.tar.gz 文件（打包同时压缩，产生.tar打包文件，显示详细信息,指定压缩后的名字要）
 
 
 tar -zcvf  myHome.tar.gz /home/new  ---> 将此目录 /home/new 压缩到当前目录来，名字为myHome.tar.gz 
 
 
 tar -zxvf myHome.tar.gz   ---->将此压缩文件解压到当前目录
 
 
 tar -zxvf myHome.tar.gz -C /home/ --->将此文件解压到/home/目录下面
```



 
 
 


##### ip地址查询
`ifconfig`

##### 重启系统

```Java
reboot
shutdown -h now   -> 立即关机
shutdown -h 1     -> 一分钟后关机
shutdown -r now     -> 立即重启 

halt   --->关机 
reboot --->重启系统
sync   -->将内存数据保存到磁盘上
```



##### 从xshell 注销
> logout --> 图形界面使用此命令无效
 


 

#####   用户管理
- 家目录
> /home/ 在该目录下有各自创建的用户对于的家目录，当用户登录时，会自动的加入自己的家目录中
- 用户组
> root 属于root组，用户1属于用户1组，....
- 每个用户至少属于一个组
- 添加用户
```Java
useradd use1  --> 添加一个用户(use1) 分组是use1
useradd -d /home/other/  use2 ->创建use2用户 在特定的目录下（目录别先创建）
passwd use1  -->给use1设置密码
passwd use2  -->给use2设置密码
```
- 删除用户
```Java
userdel use1 删除用户  但是保留其家目录
useedel -r use1 删除用户 和其家目录
```



- 查询用户信息
```Java
id use1  --->查询use1的信息 
用户的id号      所属于的组id    所属于的组的名字
uid=0(root)     gid=0(root)        组=0(root)


whoami --->查看当前的用户信息
```


- 对用户分组
```Java
groupadd group1   --->添加分组 group1
```
- 删除用户组
```Java
groupdel group1   --->删除用户组gruop1 
```


- 创建用户并指定到具体的组
```Java
useradd -g wudang zwj  ---> 创建zwj 并分组为 wudang
```

- 将用户移动到指定的组
```Java
usermod -g shaolin zwj   --->将zwj移动到shaolin这个组去
```


- 用户配置文件
```Java
/etc/passwd   -->用户的配置文件（用户信息）
/etc/group    -->组配置文件
/etc/shadow   -->口令配置文件（密码和登录信息，加密的）
```


- 解释配置文件 （/etc/passwd）
![TIM截图20190814103633.png](https://upload-images.jianshu.io/upload_images/2953304-255f3801d01766fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 运行级别
![TIM截图20190814113340.png](https://upload-images.jianshu.io/upload_images/2953304-eed0bbee69aaf674.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 切换到指定运行级别的指令
> init[012356]
```Java
init 0 -->关机
init 5 -->图形界面
...
```


##### 当root密码丢失怎么办？
> init 1 进入单用户模式（root不需要密码）




