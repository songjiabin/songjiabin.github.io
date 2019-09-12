---
title: go相关信息
date: 
tags: 
- go 
copyright: true
categories: go
---



<blockquote class="blockquote-center">习闲成懒，习懒成病。</blockquote>

<!-- more -->


`go env` 查看go相关的环境路径


一个包下面有不同的`.go`文件。包名都是一样的，运行'go run main.go'会出现错误
- 这个时候要使用 git 终端。使用`go run *.go`可以解决问题。
- 还可以使用window下，`go build .`,在执行想应的exe文件就可以了



使用 go install 
- 设置GOPATH 我设置的事：`E:\gocode\goproject\goproject\src\go_code_book\12(工程管理、不同目录)\src`
- 设置GOBIN 我的设置是：`E:\gocode\goproject\goproject\src\go_code_book\12(工程管理、不同目录)\bin`
- 找到main目录中执行`go install`生成bin文件 



tip/ip

TCP/IP|简述
---|---
应用层|比较具体的协议了（http...)
传输层|进程到进程（端口号之间）
网络层|主机到主机（ip到ip之间）
链路层|设备到设备，网卡（mac地址）之间



gopath
> 设置好gopath后，可以再任何地方创建go文件，根据gopath的路径进行任意的引用文件就OK



##### int string 之前转换
```Java 
string转int：    int, err := strconv.Atoi(string)
string转int64：  int64, err := strconv.ParseInt(string, 10, 64)
int转string：    string := strconv.Itoa(int)
int64转string：  string := strconv.FormatInt(int64, 10)
```



##### go中的引用类型
> 指针属于引用类型，其它的引用类型还包括 slices(切片)，maps(map)和 channel(管道)。

- - -
>Go中的数组是数值，因此当你向函数中传递数组时，函数会得到原始数组数据的一份复制。如果你打算更新数组的数据，可以考虑使用数组指针类型。
```
import "fmt"

func main() {
	x := [3]int{1, 2, 3}
	func(arr *[3]int) {
		(*arr)[0] = 0
		fmt.Println(arr)
	}(&x)

	fmt.Println(x)
}
```


##### map判断key是否存在
```Java
x := make(map[string]string)
	x["two"] = "1"
	if _, ok := x["two"]; !ok {
		fmt.Println("no entry")
	}else {
		fmt.Println("存在哦")
	}
```



##### 错误类型
- errors.New
> 任何时候当你需要一个新的错误类型，都可以用 errors（必须先 import）包的 errors.New 函数接收合适的错误信息来创建。

```Java
func Sqrt(f float64) (float64, error) {
	if f < 0 {
		return 0, errors.New("math - square root of negative number")
	} else {
		return 0, nil
	}
}
```

- fmt.Printf()
```Java
if f < 0 {
    return 0, fmt.Errorf("square root of negative number %g", f)
}
```


###### 并发
```Java
runtime.NumCPU()       // 返回当前CPU内核数
runtime.GOMAXPROCS(2)   // 设置运行时最大可执行CPU数
runtime.NumGoroutine()  // 当前正在运行的gorouti  
```
##### channel 
```Java
import (
	"fmt"
	"time"
)

func main() {
	//不适用缓冲区
	c := make(chan int)
	go send(c)
	go recv(c)
	time.Sleep(3*time.Second)
	close(c)
}

//只能向chan里send数据
func send(c chan<- int) {
	for i := 0; i < 10; i++ {
		c <- i
	}
}

//接收chan数据
func recv(c <-chan int) {
	for i := range c {
		fmt.Println(i)
	}
}

```
> 往channel 发送数据后，这个数据如果没有取走，channel是阻塞的，也就是不能继续向channel 里面发送数据。因为上面代码中，我们没有指定channel 缓冲区的大小，默认是阻塞的。





##### WaitGroup(用于实现工作池)
> 用于等待一批 Go 协程执行结束。程序控制会一直阻塞，直到这些协程全部执行完毕。假设我们有 3 个并发执行的 Go 协程（由 Go 主协程生成）。Go 主协程需要等待这 3 个协程执行结束后，才会终止。这就可以用 WaitGroup 来实现

```
package main

import (
	"fmt"
	"sync"
	"time"
)

func process(i int, wg *sync.WaitGroup) {
	fmt.Println("started Goroutine ", i)
	time.Sleep(2 * time.Second)
	fmt.Printf("Goroutine %d ended\n", i)
	//wg -1 
	wg.Done()
}

func main() {
	no := 3
	var wg sync.WaitGroup
	for i := 0; i < no; i++ {
	//添加了三次
		wg.Add(1)
		//传递地址
		
		go process(i, &wg)
	}
	//当wg中数量是0的时候，停止堵塞 
	wg.Wait()
	fmt.Println("All go routines finished executing")
}
```
>  上述程序里，for 循环迭代了 3 次，我们在循环内调用了 wg.Add(1)（第 20 行）。因此计数器变为 3。for 循环同样创建了 3 个 process 协程，然后在第 23 行调用了 wg.Wait()，确保 Go 主协程等待计数器变为 0。在第 13 行，process 协程内调用了 wg.Done，可以让计数器递减。一旦 3 个子协程都执行完毕（即 wg.Done() 调用了 3 次），那么计数器就变为 0，于是主协程会解除阻塞。 



##### map的声明
```Java
// 声明但未初始化map，此时是map的零值状态
map1 := make(map[string]string, 5)

map2 := make(map[string]string)

// 创建了初始化了一个空的的map，这个时候没有任何元素
map3 := map[string]string{}

// map中有三个值
map4 := map[string]string{"a": "1", "b": "2", "c": "3"}
```


##### map range的时候想改变原有的值
> 如果你需要更新原有集合中的数据，使用索引操作符来获得数据。
```Java
data := []int{1, 2, 3}
	for i, _ := range data {
		data[i] *= 10
	}

	fmt.Println("data:", data) // 程序输出 data: [10 20 30]
```







##### 使用 Godoc
> 在程序中我们一般都会注释，如果我们按照一定规则，Godoc 工具会收集这些注释并产生一个技术文档。

`godoc -http=:6060 -goroot="."` 
然后在浏览器打开地址：http://localhost:6060


​    
##### len()
> Go语言默认使用UTF-8编码，对Unicode的支持非常好。但这也带来一个问题，也就是很多资料中提到的“获取字符串长度”的问题。内置的len()函数获取的是每个字符的UTF-8编码的长度和，而不是直接的字符数量。

```Java
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {

    s := "其实就是rune"
    fmt.Println(len(s))                    // "16"
    fmt.Println(utf8.RuneCountInString(s)) // "8"
}
```


##### 拼接字符串
- 直接使用运算符
```Java
str := "Beginning of the string " +
"second part of the string"
- - - - - - - - - - 
s := "hel" + "lo, "
s += "world!"
fmt.Println(s) // 输出 “hello, world!”
```
> 里面的字符串都是不可变的，每次运算都会产生一个新的字符串，所以会产生很多临时的无用的字符串，不仅没有用，还会给 gc 带来额外的负担，所以性能比较差。

- fmt.Sprintf()
```Java
fmt.Sprintf("%d:%s", 2018, "年")
```
> 内部使用 []byte 实现，不像直接运算符这种会产生很多临时的字符串，但是内部的逻辑比较复杂，有很多额外的判断，还用到了 interface，所以性能一般。

- strings.Join()
```Java
strings.Join([]string{"hello", "world"}, ", ")
```
> Join会先根据字符串数组的内容，计算出一个拼接之后的长度，然后申请对应大小的内存，一个一个字符串填入，在已有一个数组的情况下，这种效率会很高，但是本来没有，去构造这个数据的代价也不小。


- bytes.Buffer
```Java
var buffer bytes.Buffer
buffer.WriteString("hello")
buffer.WriteString(", ")
buffer.WriteString("world")

fmt.Print(buffer.String())
```
> 这个比较理想，可以当成可变字符使用，对内存的增长也有优化，如果能预估字符串的长度，还可以用 buffer.Grow() 接口来设置 capacity。


- strings.Builder
```Java
var b1 strings.Builder
b1.WriteString("ABC")
b1.WriteString("DEF")

fmt.Print(b1.String())
```
> strings.Builder 内部通过 slice 来保存和管理内容。slice 内部则是通过一个指针指向实际保存内容的数组。strings.Builder 同样也提供了 Grow() 来支持预定义容量。当我们可以预定义我们需要使用的容量时，strings.Builder 就能避免扩容而创建新的 slice 了。strings.Builder是非线程安全，性能上和 bytes.Buffer 相差无几。


#####  new([5]int) 和 ar arr2 [5]int 的区别
```Java
func main() {

    var arr1 = new([5]int)
    arr := arr1
    arr1[2] = 100
    fmt.Println(arr1[2], arr[2])

    var arr2 [5]int
    newarr := arr2
    arr2[2] = 100
    fmt.Println(arr2[2], newarr[2])
}
程序输出：
100 100
100 0
```
> new([5]int) 创建的是数组指针, arr其实和arr1指向同一地址。所以其中一个变化，都会变化。而newarr是由arr2值传递（拷贝），故而修改任何一个都不会改变另一个的值。



#####  切片重组(reslice)
> 通过改变切片长度得到新切片的过程称之为切片重组 reslicing，做法如下：slice1 = slice1[0:end]，其中 end 是新的末尾索引（即长度）。

```Java
func get() []byte {  
    raw := make([]byte, 10000)
    fmt.Println(len(raw), cap(raw), &raw[0]) // 显示: 10000 10000 数组首字节地址
    return raw[:3]  // 10000个字节实际只需要引用3个，其他空间浪费
}

func main() {  
    data := get()
    fmt.Println(len(data), cap(data), &data[0]) // 显示: 3 10000 数组首字节地址
}
```

>如：上面的这个例子，新的slice会继续引用原有slice的数组。在你的应用分配大量临时的slice用于创建新的slice来引用原有数据的一小部分时，会导致难以预期的内存使用。

**为了避免这个陷阱，我们需要从临时的slice中使用内置函数copy()，拷贝数据（而不是重新划分slice）到新切片。**



```Java
func get() []byte {
    raw := make([]byte, 10000)
    fmt.Println(len(raw), cap(raw), &raw[0]) // 显示: 10000 10000 数组首字节地址
    res := make([]byte, 3)
    copy(res, raw[:3]) // 利用copy 函数复制，raw 可被GC释放
    return res
}

func main() {
    data := get()
    fmt.Println(len(data), cap(data), &data[0]) // 显示: 3 3 数组首字节地址
}
```


##### append() 切片追加
>Append()函数将 0 个或多个具有相同类型 S 的元素追加到切片s后面并且返回新的切片；追加的元素必须和原切片的元素同类型。如果 s 的容量不足以存储新增元素，append 会分配新的切片来保证已有切片元素和新增元素的存储。



 


##### 下载mysql
`go get github.com/go-sql-driver/mysql`



###### 下载orm包

```go
go get github.com/astaxie/beego/orm
```






##### 下载 sqlx 
`go get github.com/jmoiron/sqlx`


##### go-redis 下载
`go get -u github.com/go-redis/redis`


##### redisgo 下载
`go get github.com/gomodule/redigo/redis`


##### 获取唯一的id
`go get github.com/satori/go.uuid`


##### 对html进行解析的库
`go get github.com/PuerkitoBio/goquery`

> 当下载此包的时候出错误：package golang.org/x/net/html: unrecognized import path "golang.org/x/net/html"。原因是没有此包，需要下载

下载 `package golang.org/x/net/html`此文件包 

`git clone  https://github.com/golang/net` 下载到本地。

在`gopath/src/golang.org/x/net` 将刚才下载的`net`下的数据复制这里，重新下载，解决




