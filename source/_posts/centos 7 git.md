---
title: centos 7 git
tags: 
- linux
- git 
copyright: true
categories: linux
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![img](/images/the-color-run-4477874__340.jpg)



<!-- more -->

##### 安装

```git
yum install git 
```

##### 查看版本

```git
git version 
```

##### 配置信息

```git
root@VM_0_13_centos gitproject]# git config --global user.name "songjiabin"
[root@VM_0_13_centos gitproject]# git config --global user.email 1796254117@qq.com
```

##### 查看log

```git
git log --查看当前的log信息（当前版本之前的log）
git reflog --查看所有的版本信息

```

##### 回调版本

```git
git reset --hard HEAD^  回退到上次提交 ^表示上次
git reset --hard  471e57353762ed48d08c4a1990af98450f85da5e  回退到指定的版本
```

#####  查看当前的状态 git status 

```golang
git status  -- 可以根据里面的提示信息进行操作
```

##### add  文件后 回退操作

> 当我`add`文件后感觉又不想`add`了，可以使用`git reset HEAD 文件` 进行回退

##### 分支的一些命令

```git
git branch  		--查看分支
git branch dev  	--新建分支dev
git checkout dev 	--切换分支dev
git checkout -b newDev --创建并切换分支newDev
git checkout -d dev   --删除分支
```

##### 合并

> 当两个分支存在冲突的话，需要手动进行合并，提交

###### merge 合并

> 将master分支的位置移动到最新分支的位置上，git就会合并。这样的合并就是`fast-forward`（快进）合并

```git
git checkout master --切换到主分支
git merge dev    ---将dev分支的内容合并到主分支上   
git diff 查看合并冲突的地方
解决冲突
git add -A
git commit -m ""
gitk 查看合并的图文形式
```

##### 以树形查看版本记录

```git
git log --graph 
```

##### 暂存代码

> 当有急事任务，不允许提交版本的情况下。可以使用暂存代码。当解决完成后，再次回到暂存的状态

```git
git stash    --将当前的代码提交到缓存区（代码回退到上个commit的版本）
git checkout -b bug01 -- 新建分支并切换，用来解决bug
...进行就跟操作
git add .
git commit -m "提交修改的内容"
git checkout master --切换到主分支
git merge bug01 进行合并改成修改的内容
git stash list --查看缓存区的内容
git stash pop  --得到缓存区的数据，恢复到当前的版本
```

##### gitk(用图文查看历史记录)

![1569396115383](/images/1569396115383.png)