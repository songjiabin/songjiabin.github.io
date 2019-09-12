---
title: flutter - 查看天气
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">桃李春风一杯酒，江湖夜雨十年灯。</blockquote>

<!-- more -->

> 今天看到了一个小的开源项目，根据[和风天气](https://id.heweather.com/login?redirect=https://console.heweather.com/app/index)接口查看天气情况。于是跟着他demo进行学习，现在总结下学到的知识点！

![查看天气.gif](https://upload-images.jianshu.io/upload_images/2953304-f2e7e9706974083c.gif?imageMogr2/auto-orient/strip)


##### 学习到的第三方插件
- dio 网络请求库
> 使用dio进行网络请求，这个项目中用的到是get请求。
```Java 
 String url = rootUrl + '/now';
    try {
      Response response = await Dio().get(
          url,//请求url
          options: options,//超时时间设定
          //参数 
          queryParameters: {
            'location': location,
            'key': key});
     // 得到结果 
      RealTimeWeather realTimeWeather = RealTimeWeather.jsonToBean(
          response.data['HeWeather6'][0]);

      if (!realTimeWeather.status.contains('ok')) {
        return realTimeWeather;
      }
```
- shared_preferences sp数据存储库
> 用于存储项目中的需要保存的信息
```Java
 //使用
 if (SpClient.sp.getString('cid') != null) {
      cid = SpClient.sp.getString('cid');
    } else {
      cid = 'CN101010100';
    }
```
- intl 时间格式化工具
> 在此项目里面进行的是，将当前年月日换成成星期几
```Java
//将具体的年月日换成成周几 
 String date = DateFormat('EE', 'zh_cn').format(
      DateTime.parse(dailyForecast.date),
    );
```
- fluttertoast Toast库
> 实现类似android Toast的功能
```Java
Fluttertoast.showToast(msg: '更新成功');
```
##### 单例模式的使用
> 在使用 网络请求和share_presenter的时候，为了保持全局唯一的对象，使用了单例模式。

```Java
class DioClient {

  static DioClient _instance; //单例对象
  //通过工程模式来回去该实例
  factory DioClient() => getInstance();


  //得到单例情况下的对象
  static DioClient getInstance() {
    if (_instance == null) {
      return DioClient._internal();
    }
  }

  //构造方法
  DioClient._internal(){
    //初始化一些数据
  }
}  
```
使用：
```Java
DioClient.getInstance();//得到了 DioClient的对象 
```

##### 控件知识的学习
- Container 中背景渐变
>使用 Container中的decoration，也就是边框操作。具体操作如下：
```Java
    new Container(
            decoration: BoxDecoration(
              //边框 展示一个渐变的颜色
              gradient: LinearGradient(
                //从左下角开始变色
                begin: Alignment.bottomLeft,
                //结束点是右上角
                end: Alignment.topRight,
                colors: [Colors.blue, Colors.blue.withOpacity(0.3)],
              ),
            ),
            child: ...
            ....
```
 ![颜色渐变.png](https://upload-images.jianshu.io/upload_images/2953304-8394cda91508ce12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
 
 
 - 不同分辨率的图片放到不同的文件夹下
 
 ![TIM截图20190612174345.png](https://upload-images.jianshu.io/upload_images/2953304-5c97a03ce7e5105d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
会根据不同分辨率的手机加载不同的图片

- 当需要因为无数据不需要显示控件的时候，使用`Container()`,如：

```Java
this.widget._realTimeWeather == null ? Container() : Text(
                this.widget._realTimeWeather.now.cond_txt,
                style: TextStyle(fontSize: 20, color: Colors.white),
              )
```

- 下拉刷新的控件 `RefreshIndicator`
> 类似android 5.0 后材料设计中出的这个样式 使用：

``` Java
 //层叠布局刷新的按钮
          RefreshIndicator(
            child: 
            // 滑动布局中
            SingleChildScrollView(
              physics: BouncingScrollPhysics(
                  parent: AlwaysScrollableScrollPhysics()),
              child: 
              //当刷新的时候此控件被包裹的内容
              Center(child: SizedBox(height: 100.0)),
            ),
            //当刷新的时候触发
            onRefresh: _pullDownRefresh, color: Colors.black26,),
```
刷新的时候触发的代码
```Java
  //refresh 刷新
  Future _pullDownRefresh() async {
    bool result = await _updateWeather();
    if (result) {
      Fluttertoast.showToast(msg: '更新成功');
    } else {
      Fluttertoast.showToast(msg: '更新失败');
    }
  }
```

- showModalBottomSheet  弹出底部菜单
> 当要选择城市的时候，我们需要弹出一个底部菜单栏。
```Java
    showModalBottomSheet(
        context: context, builder: (BuildContext context) {
      return new Column(
        mainAxisSize: MainAxisSize.min,
        children: [
          ListTile(
            title: Text('切换城市'),
            onTap: () {
              //关闭弹出来的界面
              Navigator.pop(context);
              //跳转界面 右滑结束界面 
              Navigator.push(context, CupertinoPageRoute(builder: (_) {
                //跳转到搜索城市的界面
                return SearchPage();
              })).then((result) {
               //搜索城市的时候返会的数据
                if (result != null && this.mounted) {
                  _updateWeather();
                }
              });
            },
          ),
          Divider(height: 0.0),
          ListTile(
            title: Text('选择语言'),
            onTap: () {

            },
          ),
          Divider(height: 0.0,),
          ListTile(
            title: Text('关于'),
            onTap: () {

            },
          )
        ],
      );
    });
```

-  Future.wait 异步执行的时候等待操作
> 当有多个异步任务时，并且对几个任务的先后顺序没有要求的时候，可以使用` Future.wait`。当里面所有的异步任务都执行完成后，再进行下面的操作。
```Java
  //异步执行的列表，所有的异步方法执行顺序并不是很重要
  void getData() async {
    Future.wait(
        [SpClient.getInstance(), initializeDateFormatting("zh_CN", null)])
        .then((v) async {
      SpClient.sp.clear();
      if (SpClient.sp.getString('cid') == null) {
        //如果sp里面没有存数据 那么默认显示北京的
        SpClient.sp.setString('cid', 'CN101010100');
        await updateWeather(SpClient.sp.getString('cid'));
        setState(() {
            //刷新界面
        });
      }
    });
  }
```

- 得到上面状态栏的高度
```Java
   //上面状态栏的高度
double statusBarHeight = MediaQuery
        .of(context)
        .padding
        .top;
```
- 在脚手架中自定义appBar-> PreferredSize
> 自定义你appbar的样式
```Java
new Scaffold(
      appBar: 
      //自定义appbar 
      PreferredSize(
          child:
          new Container(
              color: Colors.blue.withOpacity(0.6),
              padding: EdgeInsets.only(top: statusBarHeight),
              child: _getSearchappBarWidget()
          ),
          //appbar的高度   
          preferredSize: Size.fromHeight(56.0)),
      body: _getSearchBody(),
    );
```

- 在widget中有个属性是mounted
> 意思是当widget树中还在关联的时候，常用于判断
```Java
  DioClient.getInstance().getSearchCity(v).then((v) {
                //当v不等于空 而且 次树正在关联着 执行下面
                  if (v != null && this.mounted) {
                    setState(() {
                      this.listData = v;
                    });
                  }
```


源码地址：https://github.com/songjiabin/flutter/tree/master/lib/weather