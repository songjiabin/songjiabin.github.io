---
title: go gin 操作
tags: 
- gin
- go 
- android 
copyright: true
categories: go 
toc: true
typora-copy-images-to: ..\images
typora-root-url: ..
---

![草地Hogweed, 弗罗斯特, 冻结, 冰, 冷, 严寒, 冰镇, 寒冬](/images/meadow-hogweed-4530921__340.jpg)

<!-- more -->

> [Gin](https://github.com/gin-gonic/gin) 是用 Go 编写的一个 Web 应用框架，对比其它主流的同类框架，他有更好的性能和更快的路由。由于其本身只是在官方 net/http 包的基础上做的完善，所以理解和上手很平滑。如果你现在开始做一套新的Api，我十分推荐你使用它。

#### Gin 的使用

##### 安装和更新

首次安装，使用 `go get`命令获取即可。

```golang
 go get github.com/gin-gonic/gin
```

更新就是常规的 `go get -u`

```golang
go get -u github.com/gin-gonic/gin
```

##### 快速运行

在你的 main 包中，引入 gin 包并初始化。

```golang
import (
	"github.com/gin-gonic/gin"
	"net/http"
)
func main() {
	// 初始化引擎
	engine := gin.Default()
	//注册一个路由和处理函数
	engine.Any("/", func(context *gin.Context) {
		context.String(http.StatusOK, "hello_world")
	})
	//get方法
	engine.GET("/name", func(context *gin.Context) {
		context.String(http.StatusOK, "get请求哦")
	})

	engine.Any("/any/good", hanlderGet)
	engine.Run(":9217")

}
func hanlderGet(context *gin.Context) {
	context.String(http.StatusOK, "请求成功200")
}

```

`go run main.go`运行

```golang
Listening and serving HTTP on :9217  访问此端口 
```

#### 路由(Router)

##### Restful Api

> 你可以注册路由方法有 GET, POST, PUT, PATCH, DELETE 和 OPTIONS.

调用：

```golang
func main() {
	// 初始化引擎
	router := gin.Default()

	// get
	router.GET("/name", func(context *gin.Context) {
		context.String(http.StatusOK, "get请求哦")
	})

	// post
	router.POST("/post", func(context *gin.Context) {
		context.String(http.StatusOK, "post请求哦")
	})

	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)

	//默认绑定的事8080
	router.Run()
}
```

##### 动态路由（参数路由）

> 有时候我们需要动态的路由，如 `/user/:id`，通过调用的 url 来传入不同的 id .在 Gin 中很容易处理这种路由：

- 常规的动态路由

```golang
func main() {
	engine := gin.Default()
	//注册一个动态路由
	
	// 可以匹配 /user/joy
	// 不能匹配 /user 和 /user/
	engine.GET("/user/:name", func(context *gin.Context) {
		//使用context.getParam(key) 获取url参数
		name := context.Param("name")
		context.String(http.StatusOK, "hello %s", name)
	})
  engine.Run()
}
```

- 高级的动态路由

```golang
func main() {
	engine := gin.Default()
	// 注册一个高级的动态路由

	// 该路由会匹配 /user/john/ 和 /user/john/send
	// 如果没有任何路由匹配到 /user/john, 那么他就会重定向到 /user/john/，从而被该方法匹配到
	engine.GET("/user/:name/*action", func(context *gin.Context) {
		name := context.Param("name")
		action := context.Param("action")
		message := name + " is " + action
		context.String(http.StatusOK, message)
	})
	engine.Run()
}
```

##### 路由组

> 一些情况下，我们会有统一前缀的 url 的需求，典型的如 Api 接口版本号 `/v1/something`。Gin 可以使用 Group 方法统一归类到路由组中：

```golang
func main() {
	engine := gin.Default()
	// 定义一个组前缀
	//  (/v1/login)  就会匹配到这个组
	v1 := engine.Group("/v1")

	{
		v1.GET("/login", func(context *gin.Context) {
			context.String(http.StatusOK, "login")
		})
		v1.GET("/submit", func(context *gin.Context) {
			context.String(http.StatusOK, "submit")
		})
		v1.GET("/read", func(context *gin.Context) {
			context.String(http.StatusOK, "read")
		})
	}
	engine.Run()
}

```

请求的时候：`http://localhost:8080/v1/login`

##### 中间件（Middleware）

> 现代化的 Web 编程，中间件已经是必不可少的了。我们可以通过中间件的方式，验证 Auth 和身份鉴别，集中处理返回的数据等等。Gin 提供了 Middleware 的功能，并与路由紧紧相连。

###### 单个路由中间件

> 单个路由使用中间件，只需要在注册路由的时候指定要执行的中间件即可。

```golang
engine := gin.Default()
// 注册一个路由，使用了 middleware1，middleware2 两个中间件
engine.GET("/someGet", middleware1, middleware2, func(context *gin.Context) {
	log.Print("处理请求")
	context.String(http.StatusOK, "中间件")
})
engine.Run()
- - - 
//中间件1
func middleware1(context *gin.Context) {
	log.Println("中间件1")
	context.Next()
	log.Println("中间件1--next后")
}

//中间件2
func middleware2(context *gin.Context) {
	log.Println("中间件2")
	context.Next()
	log.Println("中间件2--next后")
}
```

日志输入：

```golang
中间件1
中间件2
处理请求
中间件2--next后
中间件1--next后
```

当中间件中遇到了next()会去调到流程下的一个方法进行执行，当把这个方法中执行完后，在回来继续执行next()后的代码。

###### 路由组使用中间件

> 路由组使用中间件和单个路由类似，只不过是要把中间件放到 `Group` 上

```golang
func main() {
	engine := gin.Default()
	//路由组 连接中间件
	groupV2 := engine.Group("/v2", middleware1, middleware2)
	groupV2.GET("/login", func(context *gin.Context) {
		context.String(http.StatusOK, "路由组中间件")
	})
	engine.Run()
}
- - - 
//中间件1
func middleware1(context *gin.Context) {
	log.Println("中间件1")
	context.Next()
	log.Println("中间件1--next后")
}

//中间件2
func middleware2(context *gin.Context) {
	log.Println("中间件2")
	context.Next()
	log.Println("中间件2--next后")
}
```

日志输入：

```golang
中间件1
中间件2
处理请求
中间件2--next后
中间件1--next后
```

#### 参数

##### Url 查询参数

> 假定一个 url 为 `/welcome?firstname=Jane&lastname=Doe`，我们想获取参数 `firstname` 的内容，可以使用`c.Query`方法。该方法始终返回一个 `string` 类型的数据。

```golang
func main() {

	engine := gin.Default()
	// 注册路由和Handler
	// url为 /welcome?firstname=Jane&lastname=Doe
	engine.GET("/welcome", func(context *gin.Context) {
		// 获取参数内容
		// 获取的所有参数内容的类型都是 string
		// 如果不存在，使用第二个当做默认内容
		firstname := context.DefaultQuery("firstname", "默认的名字哦")
		// 获取参数内容，没有则返回空字符串
		lastname := context.Query("lastname")
		context.String(http.StatusOK, "Hello %s %s", firstname, lastname)
	})
	engine.Run()
}

```

#### 表单和Body参数（Multipart/Urlencoded Form）

> 典型的如 `POST` 提交的数据，无论是 `multipart/form-data`格式还是`application/x-www-form-urlencoded`格式，都可以使用 `c.PostForm`获取到参数。该方法始终返回一个 `string` 类型的数据。

```golang
func main() {
	engine := gin.Default()
	engine.POST("/android_post", func(context *gin.Context) {
		// 获取post过来的message内容
		// 获取的所有参数内容的类型都是 string
		message := context.PostForm("message")
		name := context.DefaultPostForm("name", "基本密码")
		types := context.DefaultPostForm("type", "type")
		context.JSON(http.StatusOK, gin.H{
			"status":  0,
			"message": message,
			"name":    name,
			"type":    types,
		})
	})
	engine.Run()
}
```

另外附上`android`端请求的代码

```android
   OkHttpClient okHttpClient = new OkHttpClient();
        RequestBody formBody = new FormBody.Builder()
                .add("message", "1")
                .add("name", "2")
                .add("type", "video")
                .build();
        Request request = new Request.Builder()
                .url("http:192.168.2.133:8080/android_post")
                .post(formBody)
                .build();
        okHttpClient.newCall(request).enqueue(new Callback() {
            @Override
            public void onFailure(Call call, IOException e) {
                Log.i(">>>>>>", "onFailure"+e.getMessage());
            }

            @Override
            public void onResponse(Call call, Response response) throws IOException {
                Log.i(">>>>>>", response.body().string());
            }
        });
//别忘记添加引用
 compile 'com.squareup.okhttp3:okhttp:3.12.1'
```

返回的结果是：`{"message":"1","name":"2","status":0,"type":"video"}`

#### 上传文件

> Gin 对接受用户上传的文件做了友好的处理，在 Handler 中可以很简单的实现文件的接收。
>
>

**要注意的是，上传文件的大小有限制，通常是 `<32MB`，你可以使用 `router.MaxMultipartMemory`更改它。**

- 处理一个文件

```golang
func main() {
	engine := gin.Default()
	// 设置文件上传大小 router.MaxMultipartMemory = 8 << 20  // 8 MiB
	// 处理单一的文件上传
	engine.MaxMultipartMemory = 8 << 20 // 8 MiB
	engine.POST("/upload", func(context *gin.Context) {
		//拿到这个文件
		file, _ := context.FormFile("file")
		log.Println(file.Filename)
		context.String(http.StatusOK, fmt.Sprintf("'%s' uploaded!", file.Filename))
	})
}
```

- 处理一个文件并接受它保存到本地

```golang
func main() {
	engine := gin.Default()
	engine.POST("/uploads", func(context *gin.Context) {
		//获取请求中文件的名字为 file
		file, header, err := context.Request.FormFile("file")
		name := context.DefaultPostForm("name", "基本密码")
		log.Print(name)
		if err != nil {
			context.String(http.StatusBadRequest, "Bad request")
			return
		}
		filename := header.Filename

		fmt.Println(file, err, filename)
		// 将传递的文件保存到本地
		out, err := os.Create("C:/Users/songjiabin1/Desktop/" + filename)
		defer out.Close()
		io.Copy(out, file)
		context.String(http.StatusOK, "upload successful")

	})
	engine.Run()
}
```

- android端的代码

> 使用到了文件选择器

```golang
compile 'ru.bartwell:exfilepicker:2.1'
```

点击文件得到其文件名字。

```golang
  private final int EX_FILE_PICKER_RESULT = 0xfa01;
  private String startDirectory = null;// 记忆上一次访问的文件目录路径
 // 打开系统的文件选择器
    public void pickFile(View view) {
        ExFilePicker exFilePicker = new ExFilePicker();
        exFilePicker.setCanChooseOnlyOneItem(true);// 单选
        exFilePicker.setQuitButtonEnabled(true);

        if (TextUtils.isEmpty(startDirectory)) {
   exFilePicker.setStartDirectory(Environment.getExternalStorageDirectory().getPath());
        } else {
            exFilePicker.setStartDirectory(startDirectory);
        }
        exFilePicker.setChoiceType(ExFilePicker.ChoiceType.FILES);
        exFilePicker.start(this, EX_FILE_PICKER_RESULT);
    }
    
    - - -
    
   // 获取文件的真实路径
    @Override
    protected void onActivityResult(final int requestCode, final int resultCode, @Nullable final Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == EX_FILE_PICKER_RESULT) {
            ExFilePickerResult result = ExFilePickerResult.getFromIntent(data);
            if (result != null && result.getCount() > 0) {
                String path = result.getPath();

                List<String> names = result.getNames();
                for (int i = 0; i < names.size(); i++) {
                    File f = new File(path, names.get(i));
                    try {
                        Uri uri = Uri.fromFile(f); //这里获取了真实可用的文件资源
                        Toast.makeText(Demo.this, "选择文件:" + uri.getPath(), Toast.LENGTH_SHORT)
                                .show();

                        uploadFile(new File(uri.getPath()));
                        startDirectory = path;
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    
    - - -
    
    //处理并上传到服务器
    private void uploadFile(final File file) {
        new Thread() {
            @Override
            public void run() {
                super.run();
                //进行自己的操作
                MultipartBody.Builder bodyBuilder = new MultipartBody.Builder()
                        .setType(MultipartBody.FORM);
                MediaType mediaType = MediaType.parse("text/plain");
                bodyBuilder.addFormDataPart("name", "sjb");
                bodyBuilder.addFormDataPart("file", file.getName(),
                        RequestBody.create(mediaType, file));
                String url = "http:192.168.2.133:8080/uploads";
                Request request = new Request.Builder()
                        .url(url)
                        .post(bodyBuilder.build())
                        .build();
                OkHttpClient httpClient = new OkHttpClient();

                try {
                    Response response = httpClient.newCall(request).execute();
                    if (response.isSuccessful()) {
                        Log.i(">>>>>>>", response.body().string());
                    } else {
                        throw new java.lang.IllegalStateException("Not [200..300) range code response [" + response.toString() + "]");
                    }
                } catch (IOException e) {
                    Log.e(">>>>>>>", e.getMessage().toString());
                }
            }
        }.start();
    }
```



处理多个文件

```golang
func main() {
	engine := gin.Default()
	engine.POST("/uploads", func(context *gin.Context) {
		//获取请求中文件的名字为 file
		form, _ := context.MultipartForm()
		//获取传递过来的参数
		name := context.DefaultPostForm("name", "基本密码")
		log.Println(name)
		// 拿到集合
		files := form.File["files"]
		for _, file := range files {
			log.Println(file.Filename)
		}
		context.String(http.StatusOK, fmt.Sprintf("%d files uploaded!", len(files)))

	})
	engine.Run()
}
而对应Android双传多个文件的时候，只要添加多个 
 bodyBuilder.addFormDataPart("file", file.getName(),
      RequestBody.create(mediaType, file));
即可！
```

#### 其他格式的数据

> 一些复杂的场景下，如用户直接 `POST`一段 `json`字符串到应用中，我们需要获取原始数据，这时需要用 `c.GetRawData`来获取原始字节。

```golang
func main() {
	engine := gin.Default()
	engine.POST("/jsonDataToMe", func(context *gin.Context) {
	    //接受json数据
		data, err := context.GetRawData()
		if err != nil {
			log.Fatalln(err)
			return
		}
		log.Println("data--->", string(data))
		context.String(http.StatusOK, "ok")
	})
	engine.Run()
}
```

下面是`android`的代码

```android
private void jsonDataToGo() {
        String json = "{\n" +
                "    \"name\": \"张三 \"\"age\": \"23 \"\n" +
                "}";
                //定义上传的类型数据格式
        MediaType JSON = MediaType.parse("application/json; charset=utf-8");
        String url = "http:192.168.2.133:8080/jsonDataToMe";
        OkHttpClient client = new OkHttpClient();//创建okhttp实例
        RequestBody body = RequestBody.create(JSON, json);
        Request request = new Request.Builder()
                .url(url)
                .post(body)
                .build();
        Call call = client.newCall(request);
        call.enqueue(new Callback() {
            //请求失败时调用
            @Override
            public void onFailure(Call call, IOException e) {
                Log.i(">>>>>>>", "onFailure: " + e);
            }

            //请求成功时调用
            @Override
            public void onResponse(Call call, Response response) throws IOException {
                if (response.isSuccessful()) {
                    Log.i(">>>>>>>", "onResponse: " + response.body().string());
                }
            }
        });
    }
```

#### 数据绑定(请求到数据绑定到实体)

> Gin 提供了非常方便的数据绑定功能，可以将用户传来的参数自动跟我们定义的结构体绑定在一起。

##### 绑定 Url 查询参数（Only Bind Query String）

使用 `c.ShouldBindQuery`方法，可以自动绑定 Url 查询参数到 `struct`.

```golang
type Person struct {
	Name string `form:"name"`
	Age  int    `form:"age"`
}

func main() {
	engine := gin.Default()
	engine.GET("/getname", func(context *gin.Context) {
		//name := context.DefaultQuery("name", "基本密码")
		//age := context.DefaultQuery("age", "22")
		var person Person
		if context.ShouldBindQuery(&person) == nil {
			log.Println(person.Name)
			log.Println(person.Age)
		}
		context.String(http.StatusOK, "Success")
	})
	engine.Run()
}

```

##### 绑定url查询参数和POST参数

使用 `c.ShouldBind`方法，可以将参数自动绑定到 `struct`.该方法是会检查 Url 查询字符串和 POST 的数据，而且会根据 `content-type`类型，优先匹配`JSON`或者 `XML`,之后才是 `Form`. 有关详情查阅 [这里](https://github.com/gin-gonic/gin/blob/master/binding/binding.go#L48)

```golang
package main

import "log"
import "github.com/gin-gonic/gin"
import "time"

// 定义一个 Person 结构体，用来绑定数据
type Person struct {
    Name     string    `form:"name"`
    Address  string    `form:"address"`
    Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
}

func main() {
    route := gin.Default()
    route.GET("/testing", startPage)
    route.Run(":8085")
}

func startPage(c *gin.Context) {
    var person Person
    // 绑定到 person
    if c.ShouldBind(&person) == nil {
        log.Println(person.Name)
        log.Println(person.Address)
        log.Println(person.Birthday)
    }

    c.String(200, "Success")
}
```

#### 其他数据绑定

> Gin 提供的数据绑定功能很强大，建议你仔细查阅官方文档，了解 `gin.Context`的 `Bind*`系列方法。这里就不再一一详述。

#### 数据验证

> 永远不要相信用户任何的输入。对于获取的外来数据，我们要做的第一步就是校验和转换。对于这类基础并且常用的功能， Gin 很贴心的帮我们提供了数据校验的方法，省去我们重复判断的烦恼。
>
> Gin 的数据验证是和数据绑定结合在一起的。只需要在数据绑定的结构体成员变量的标签添加`binding`规则即可。详细的规则查阅 [这里](https://godoc.org/gopkg.in/go-playground/validator.v8#hdr-Baked_In_Validators_and_Tags)。

登录数据简单验证。`form`和`json`数据

```golang
type Login struct {
	User     string `form:"user" json:"user" binding:"required"`         //required 意思是必须要传过来 不然bind直接报error
	Password string `form:"password" json:"password" binding:"required"` //required 意思是必须要传过来 不然bind直接报error
}

func main() {
	engine := gin.Default()

	engine.POST("/loginJSON", func(context *gin.Context) {
		//1.请求的参数格式为：{"user": "manu", "password": "123"}
		// 验证数据并绑定
		var loginJson Login
		if err := context.ShouldBindJSON(&loginJson); err == nil {
			if loginJson.User == "基本密码" && loginJson.Password == "123456" {
				context.JSON(http.StatusOK, gin.H{
					"code": http.StatusOK,
					"msg":  "成功",
				})
			} else {
				context.JSON(http.StatusUnauthorized, gin.H{
					"code": http.StatusUnauthorized,
					"msg":  "未经授权",
				})
			}
		} else {
			context.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
		}
	})

	engine.POST("/loginForm", func(context *gin.Context) {
		//2.请求的格式为form表单提交 表单 (user=manu&password=123)
		var loginFrom Login
		if error := context.ShouldBind(&loginFrom); error == nil {
			if loginFrom.User == "基本密码" && loginFrom.Password == "123456" {
				context.JSON(http.StatusOK, gin.H{
					"code": http.StatusOK,
					"msg":  "成功",
				})
			} else {
				context.JSON(http.StatusUnauthorized, gin.H{
					"code": http.StatusUnauthorized,
					"msg":  "未经授权",
				})
			}
		}
	})
	engine.Run()
}
```

除了绑定验证之外，你还可以注册自定义的验证器。

```golang
package main

import (
    "net/http"
    "reflect"
    "time"
    "github.com/gin-gonic/gin"
    "github.com/gin-gonic/gin/binding"
    "gopkg.in/go-playground/validator.v8"
)

// 定义的 Booking 结构体
// 注意成员变量CheckIn的标签 binding:"required,bookabledate"
// bookabledate 即下面自定义的验证函数
// 成员变量CheckOut的标签 binding:"required,gtfield=CheckIn"
// gtfield 是一个默认规则，意思是要大于某个字段的值
type Booking struct {
    CheckIn  time.Time `form:"check_in" binding:"required,bookabledate" time_format:"2006-01-02"`
    CheckOut time.Time `form:"check_out" binding:"required,gtfield=CheckIn" time_format:"2006-01-02"`
}

// 定义一个验证方法，用来验证时间是否合法
// 验证方法返回值应该是个布尔值
func bookableDate(
    v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value,
    field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string,
) bool {
    if date, ok := field.Interface().(time.Time); ok {
        today := time.Now()
        if today.Year() > date.Year() || today.YearDay() > date.YearDay() {
            return false
        }
    }
    return true
}

func main() {
    route := gin.Default()
    // 注册一个自定义验证方法 bookabledate
    binding.Validator.RegisterValidation("bookabledate", bookableDate)
    route.GET("/bookable", getBookable)
    route.Run(":8085")
}

func getBookable(c *gin.Context) {
    var b Booking
    // 验证数据并绑定
    if err := c.ShouldBindWith(&b, binding.Query); err == nil {
        c.JSON(http.StatusOK, gin.H{"message": "Booking dates are valid!"})
    } else {
        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
    }
}
```

Gin 提供的验证灰常灰常强大，可以帮我们很好的处理数据验证和省掉各种 `if else`的判断，建议各位使用 Gin 的道友一定要把这个东东吃透。

#### 输出响应

> Web 应用的目标之一就是输出响应。Gin 为我们提供了多种常见格式的输出，包括 `HTML`, `String`，`JSON`， `XML`， `YAML`。

##### string

```golang
// 省略的代码 ...

func Handler(c *gin.Context) {
    // 使用 String 方法即可
    c.String(200, "Success")
}

// 省略的代码 ...
```

##### JSON、 XML、 YAML

Gin 输出这三种格式非常方便，直接使用对用方法并赋值一个结构体给它就行了。

你还可以使用`gin.H`。`gin.H` 是一个很巧妙的设计，你可以像`javascript`定义`json`一样，直接一层层写键值对，只需要在每一层加上 `gin.H`即可。看代码：

```golang
// 省略的代码 ...

func main() {
    r := gin.Default()

    // gin.H 本质是 map[string]interface{}
    r.GET("/someJSON", func(c *gin.Context) {
        // 会输出头格式为 application/json; charset=UTF-8 的 json 字符串
        c.JSON(http.StatusOK, gin.H{"message": "hey", "status": http.StatusOK})
    })

    r.GET("/moreJSON", func(c *gin.Context) {
        // 直接使用结构体定义
        var msg struct {
            Name    string `json:"user"`
            Message string
            Number  int
        }
        msg.Name = "Lena"
        msg.Message = "hey"
        msg.Number = 123
        // 会输出  {"user": "Lena", "Message": "hey", "Number": 123}
        c.JSON(http.StatusOK, msg)
    })

    r.GET("/someXML", func(c *gin.Context) {
        // 会输出头格式为 text/xml; charset=UTF-8 的 xml 字符串
        c.XML(http.StatusOK, gin.H{"message": "hey", "status": http.StatusOK})
    })

    r.GET("/someYAML", func(c *gin.Context) {
        // 会输出头格式为 text/yaml; charset=UTF-8 的 yaml 字符串
        c.YAML(http.StatusOK, gin.H{"message": "hey", "status": http.StatusOK})
    })

    r.Run(":8080")
}

// 省略的代码 ...
```

#### HTML

> Gin 使用 Html 模板就是官方标准包`html/template`, 模板的用法没什么太多要说明的。这里给大家说一下如何在 gin 中输出 `html`。
>
> 由于 Gin 并没有强制的文件夹命名规范，所以我们必须要告诉 Gin 我们的静态资源（如图片、Css、JS 脚本等）和我们的html 模板放在了哪里，看代码：

- 目录结构是单层的

```golang
type student struct {
	Name string
	Age  int8
}
func main() {
	engine := gin.Default()
	engine.LoadHTMLGlob("templates/*")  
	stu1 := &student{Name: "Geektutu", Age: 20}
	stu2 := &student{Name: "Jack", Age: 22}

	engine.GET("/arr", func(c *gin.Context) {
		// 访问的路径是： /templates/arr.html
		c.HTML(http.StatusOK, "arr.html", gin.H{
			"title":  "Gin",
			"stuArr": [2]*student{stu1, stu2},
		})
	})
	engine.Run()
}

- - - 

html文件的路径是: /templates/arr.html
html的代码为：
<!DOCTYPE html>
<html>
<body>
<p>hello, {{.title}}</p>
{{range $index, $ele := .stuArr }}
    <p>{{ $index }}: {{ $ele.Name }} is {{ $ele.Age }} years old</p>
{{ end }}
</body>
</html>
```

- 多层目录结构

```golang
func main() {
	engine := gin.Default()
	// 注册一个目录，gin 会把该目录当成一个静态的资源目录
	// 该目录下的资源看可以按照路径访问
	// 如 static 目录下有图片路径 index/logo.png , 你可以通过 GET /static/index/logo.png 访问到
	engine.Static("/static", "./static")
	// 注册一个路径，gin 加载模板的时候会从该目录查找
	// 参数是一个匹配字符，如 views/*/* 的意思是 模板目录有两层
	// gin 在启动时会自动把该目录的文件编译一次缓存，不用担心效率问题
	engine.LoadHTMLGlob("views/*/*")  

	engine.GET("/getHtml", func(context *gin.Context) {
		// 输出 html
		// 三个参数，200 是http状态码
		// "shop/search" 要加载的模板路径，对应目录路径 views/shop/search.html
		// gin.H{"keywords":"macbook pro"} 是模板变量
		context.HTML(200, "shop/search.html", gin.H{"keywords": "macbook pro"})
	})
	engine.Run()
}

- - - 
这里注意：html的代码：
需要添加此 {{define "shop/search.html"}} //将该模板作为嵌套模板  
路径是：/views/shop/search.html


{{define "shop/search.html"}}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<h1>模板路径</h1>


<h1>模板路径2</h1>


<h1>模板路径3</h1>

<a>{{.keywords}}</a>

</body>
</html>
{{end}}
```

#### 开发热更新

> 我们在开发代码时希望能够边改代码边运行看到结果，类似于 `PHP`脚本语言那样，但受限于 Go 的编译运行，自身无法实现，所以要借助一些第三方工具来解决这个问题。
>
> 我使用 Go 开发了一个文件更新通知的软件 `fileboy`，可以很好的处理这类问题。该软件已开源，有兴趣的朋友可以在 [fileboy github ](https://github.com/dengsgo/fileboy)下载使用。



#### 参考

<https://www.itfanr.cc/2017/07/30/golang-web-framework-gin/>

https://www.yoytang.com/go-gin-doc.html#Gin%20的介绍



