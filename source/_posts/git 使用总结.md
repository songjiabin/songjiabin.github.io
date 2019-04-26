---
title: git 使用总结
date: 
tags: 
- android 
copyright: true
categories: android
---

<blockquote class="blockquote-center">在生命里，我们几乎每时每刻都在犯错</blockquote>


##### Git Commit
> Git 仓库中的提交记录保存的是你的目录下所有文件的快照，就像是把整个目录复制，然后再粘贴一样，但比复制粘贴优雅许多！

> Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录。

> Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面都有父节点的原因 —— 我们会在图示中用箭头来表示这种关系。对于项目组的成员来说，维护提交历史对大家都有好处。

> 现在你可以把提交记录看作是项目的快照。提交记录非常轻量，可以快速地在这些提交记录之间切换！

```Java 
git commit -m "提交的内容"
```


##### Git Branch
> Git 的分支也非常轻量。它们只是简单地指向某个提交纪录

> 这是因为即使创建再多分的支也不会造成储存或内存上的开销，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单多了。

> 在将分支和提交记录结合起来后，我们会看到两者如何协作。**现在只要记住使用分支其实就相当于在说：“我想基于这个提交以及它所有的父提交进行新的工作。**”

```Java 
git  branch "分支名"  创建新的分支

git checkout "分支名" 切换到对应的分支上

git branch 查看当前的分支         
```

##### 分支与合并
###### Git merge 
> 第一种方法 —— git merge。在 Git 中合并两个分支时会产生一个特殊的提交记录，它有两个父节点。翻译成自然语言相当于：“我要把这两个父节点本身及它们所有的祖先都包含进来。”


![TIM截图20181126163357.png](https://upload-images.jianshu.io/upload_images/2953304-e43c7d3f86431de6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

c0、c1 是刚开始的commit,c2是在bugFix分支上提交的，c3是在master分支上提交的。
现在要将c2分支合并到master上去。
如下所示：

![TIM截图20181126164352.png](https://upload-images.jianshu.io/upload_images/2953304-42f187be3626a4af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```Java
//先切换到 master 
git checkout master 
git merge "需要合并的分支"
```


现在等于是主线上（master）上是最全的代码了，现在要把bugFix分支上指向最全的代码（也就是将maseter分支代码合并到bugFix上去）。
```Java
//先切换到 bugFix
git checkout bugFix 
git merge master 
```


![TIM截图20181126170921.png](https://upload-images.jianshu.io/upload_images/2953304-c37c286a93927aa3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ok！现在等于是master和bugFix分支都到了一起。合并完成。

**总结**

**创建新分支 bugFix**

**用 git checkout bugFix 命令切换到该分支**

**提交一次**

**用 git checkout master 切换回 master**

**再提交一次**

**用 git merge 把 bugFix 合并到 master**

 
 
###### Git Rebase

> 第二种合并分支的方法是 git rebase。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

- - - 

> 还是准备了两个分支；注意当前所在的分支是 bugFix（星号标识的是当前分支）

> 我们想要把 bugFix 分支里的工作直接移到 master 分支上。移动以后会使得两个分支的功能看起来像是按顺序开发，但实际上它们是并行开发的。

咱们这次用 git rebase 实现此目标
如图：
![TIM截图20181126173258.png](https://upload-images.jianshu.io/upload_images/2953304-ff30dbe6cd7e9a37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```Java
git checkout bugFix
git rebase master 
```
![TIM截图20181126174750.png](https://upload-images.jianshu.io/upload_images/2953304-a67de25de629597d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


将master分支内容合并到bufFix上。入上图所示。

现在 bugFix 分支上的工作在 master 的最顶端，同时我们也得到了一个更线性的提交序列。

注意，提交记录 C3 依然存在（树上那个半透明的节点），而 C3' 是我们 Rebase 到 master 分支上的 C3 的副本。

接下来在master上合并bugFix。
```Java
git checkout master 
git rebase bugFix
```
最后合并如下：
![TIM截图20181126175131.png](https://upload-images.jianshu.io/upload_images/2953304-f91a56bbee300328.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**总结**

**新建并切换到 bugFix 分支**

**提交一次**

**切换回 master 分支再提交一次**

**再次切换到 bugFix 分支，rebase 到 master 上**



