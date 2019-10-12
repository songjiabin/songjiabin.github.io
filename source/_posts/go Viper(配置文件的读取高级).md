---
title: go Viper(配置文件的读取高级)
tags: 
- Viper
- go 
copyright: true
categories: go 
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![蘑菇, 鹅膏菌, 红色, 森林, 季节, 在秋天, 性质, 有毒](/images/mushroom-4529778__340.jpg)



<!-- more -->

#### Viper 是什么?

> viper 是以个完善的go语言配置包.开发它的目的是来处理各种格式的配置文件信息.

viper 支持:

- 设置默认配置
- 支持读取JSON TOML YAML HCL 和Java属性配置文件
- (可选)监听配置文件变化,实时读取读取配置文件内容
- 读取环境变量值
- 读取远程配置系统(etcd Consul)和监控配置变化
- 读取命令哈Flag值
- 读取buffer值
- 读取确切值

#### 为什么要使用 Viper?

> 当你创建app的时候需要关注怎么创建完美的app,而不需要关注怎么写配置文件。
>
> 使用任何编程语言开发工程化的项目都缺少不了配置，我们可能要存储一些数据库信息、邮件配置、其他的第三方服务密钥等，而配置文件的类型又有很多种，比如 `XML`、`JSON`、`YML`、`INI` 等，配置文件又可能分为不同的环境，如 `dev`、`test`、`prod`。

viper 能够帮你做这些事情

- 找到和反序列化JSON TOML YAML HCL JAVA配置文件
- 提供一个配置文件默认值和可选值的机制
- 提供重写配置值和Flag的可选值
- 提供系统的参数别名,解决对以有代码的侵入
- 轻松的辨别出用户输入值还是配置文件值

**viper 配置项key是大写不敏感的**

#### 不适用Viper的时候

我们假设现在有一份 `json` 格式的配置文件 `config.json`

```golang
{
  "port": 10666,
  "mysql": {
    "url": "(127.0.0.1:3306)/biezhi",
    "username": "root",
    "password": "123456"
  }
}
```

这个配置的结构有两个层级，第一层只有一个端口号，第二层是 MySQL 数据库的信息。在 Go 代码中可以这样读取

```golang
import (
    "encoding/json"
    "fmt"
    "io/ioutil"
)

// 定义配置文件解析后的结构
type MySQLConfig struct {
    URL      string
    Username string
    Password string
}

type Config struct {
    Port  int
    MySQL MySQLConfig
}

func main() {
    var config Config

    // ReadFile 函数会读取文件的全部内容，返回一个字节数组
    data, err := ioutil.ReadFile("./config/config.json")
    if err != nil {
        panic(err)
    }

    //读取的数据为json格式，需要进行解码
    err = json.Unmarshal(data, &config)
    if err != nil {
        panic(err)
    }
    fmt.Println(config)
}

// output
» {10666 {(127.0.0.1:3306)/biezhi root 123456}}
```

#### 未完待续

#### 参考链接

<https://blog.biezhi.me/2018/10/load-config-with-viper.html>

<https://mojotv.cn/2018/12/26/how-to-use-viper-configuration-in-golang>

