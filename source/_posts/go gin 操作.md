---
title: go gin 操作
tags: 
- gin
- go 
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
		form, _ := context.MultipartForm()
		// 拿到集合
		files := form.File["upload[]"]
		for _, file := range files {
			log.Println(file.Filename)
		}
		context.String(http.StatusOK, fmt.Sprintf("%d files uploaded!", len(files)))
	})
}
```







