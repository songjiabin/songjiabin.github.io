---
title: go基础知识巩固
date: 
tags: 
- go 
copyright: true
categories: go
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---



![img](/images/hot-air-balloons-4381674__340.jpg)

<!-- more -->

##### 占位符
```Java
%d 整型 
%f 浮点型
%t 布尔类型 
%s 字符串类型 
%c 字符类型的数据
%p 内存地址
%T 数据类型
%b 二进制
%o 八进制
%X %x 
%q 打印go语言格式的字符串

fmt.Printf();
fmt.Printf("%%")  -> 结果是：%
```



##### 数组作为函数的参数是值传递，不能修改以前的值 
##### 切片作为函数的参数是引用传递，会改变以前的值（再实际中最好使用切片）**注意，如果过程中使用了appen，会改变原有切片的地址，所以原来的值不会改变**
##### map作为函数的参数是引用传递，会改变以前的值
##### 所有的切片都属于地址传递,结构体数组作为函数参数是值传递
##### 用随机数制造双色球
> 这个小demo的难点就是红球不能重复。

```Java
 
rand.Seed(time.Now().UnixNano())

	var redball [6]int

	for i := 0; i < len(redball); i++ {
		//遍历之前存在的值和新随机数是否有重复
		//redball[i] = rand.Intn(33)+1//0-32
		temp := rand.Intn(33) + 1
		for j := 0; j < i; j++ {
			if temp == redball[j] {
				temp = rand.Intn(33) + 1
				j = -1
				continue
			}
		}

		redball[i] = temp
	}

	fmt.Println(redball, "+", rand.Intn(16)+1)
```

##### copy的时候必须要知道被拷贝的长度
```Java
olderInts := []int{
		1, 2, 3, 4, 5,
	}
	//必须知道newInts的长度
	newInts := make([]int, len(olderInts))
	copy(newInts, olderInts)

	fmt.Println(newInts)
```

##### 内存理解

![img](/images/2953304-d7ead28c283e33d0.png)


##### 为什么用切片
```Java
为什么用切片：

		1. 数组的容量固定，不能自动拓展。

		2. 值传递。 数组作为函数参数时，将整个数组值拷贝一份给形参。

		在Go语言当，我们几乎可以在所有的场景中，使用 切片替换数组使用。
```

##### 并行，并发
```Java
多核CPU 才能够并行
单核CPU 并发
runtime.Gosched()  出让当前go程所占用的cpu时间片。当再次获得cpu时，从出让位置继续回复执行。
goExit 退出当前go程序
n:=runtime.GOMAXPROCS(1)
fmt.Println("上次设置的cpu数量是：",n)
```




