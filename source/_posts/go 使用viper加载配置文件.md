---
title: go 使用viper加载配置文件
tags: 
- Viper
- go 
copyright: true
categories: go 
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![火箭, 天空, 空间, 开始, 提升, 启动, 速度, 引力, 航天, 宇航员](/images/rockets-4528194__340.png)

<!-- more -->

> 今天使用了`viper`来加载配置文件信息。下面来总结下！在[这里](<https://jjmima.top/2019/10/12/go%20Viper(%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E8%AF%BB%E5%8F%96%E9%AB%98%E7%BA%A7)/>)我做了更加详细的记录。
>
> 需求很简单，加载配置文件（`yaml`）。如果没有指定那么则加载项目中默认的配置文件。

![1571017737396](/images/1571017737396.png)



##### 指定参数

如果指定的配置文件的话 需要读取命令行的参数。那么用到了`pflag` 。

```golang
var (
	//命令行参数
	cfg = pflag.StringP("config", "c", "", "apiserver config file path.")
)
```

##### main函数

下面是`main`函数

```golang
//初始化配置 读取配置文件
	//解析函数  将会在碰到第一个非flag命令行参数时停止
	pflag.Parse()
	// init config
	if err := config.Init(*cfg); err != nil {
		panic(err)
	}
```

##### config包的代码

`config`包下面的

```golang
package config

import (
	"github.com/fsnotify/fsnotify"
	"github.com/spf13/viper"
	"log"
	"strings"
)

type Config struct {
	Name string
}

//初始化 配置文件
func Init(cfg string) error {
	config := Config{Name: cfg}
	if err := config.initConfig(); err != nil {
		return err
	}
	//监控配置文件并热加载程序
	config.watchConfig()
	return nil
}

//初始化配置文件
func (c *Config) initConfig() error {
	if c.Name != "" {
		viper.SetConfigFile(c.Name) //如果制定了配置文件 ，那么就解析这个指定的
	} else {
		// ./conf/config.yaml
		viper.AddConfigPath("./conf") //添加需要解析配置文件的路径
		viper.SetConfigName("config")  //添加需要解析的文件
	}
	viper.SetConfigType("yaml") //设置配置文件的格式为： yaml

	// 通过指定配置文件可以很方便地连接不同的环境（开发环境、测试环境）并加载不同的配置，方便开发和测试。
	viper.AutomaticEnv()            //读取匹配的环境变量
	viper.SetEnvPrefix("APISERVER") // 读取环境变量的前缀为APISERVER
	replacer := strings.NewReplacer(".", "_")
	viper.SetEnvKeyReplacer(replacer)

	if err := viper.ReadInConfig(); err != nil { // viper解析配置文件
		log.Println("错误时", err)
		return err
	}
	return nil
}

// 监控配置文件变化并热加载程序
func (c *Config) watchConfig() {
	viper.WatchConfig()
	viper.OnConfigChange(func(e fsnotify.Event) {
		log.Printf("Config file changed: %s", e.Name)
	})
}

```

上面的注释很清楚，就是初始化了`viper`的参数。当设定了配置文件后，读取配置文件的信息，如果没有设定那么读取目录结构中`gopath/项目/conf/config.yaml`的配置信息。

##### 使用

- 指定配置文件

```
cd gopath/src/myspiservce 
go build -v
产生了mysqiserver.exe文件
mysqiserver.exe -c config.yaml
```

- 不指定配置文件

```golang
cd gopath/src/myspiservce 
go build -v
产生了mysqiserver.exe文件
直接运行就行了
```

##### 项目路径 

[项目地址](<https://github.com/songjiabin/goproject/tree/master/goproject/src/myapiserver>)



