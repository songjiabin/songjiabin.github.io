---
title: flutter android  混合开发之android调用flutter方法（先前已有android项目，在此基础上再次集成flutter）
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">远远围墙，隐隐茅堂。飏青旗、流水桥旁。</blockquote>

<!-- more -->
- android 定义变量 为了和flutter更好的通信（变量一致）
```Java
private static final String CHARGING_CHANNEL = "MyFlutterApp.flutter/begainGetBattery";
```
- 注册
>  创建 EventChannel 并通过 StreamHandler 的 EventSink 发送内容给 Flutter
```Java
new EventChannel((FlutterView) flutterView, CHARGING_CHANNEL).setStreamHandler(new EventChannel.StreamHandler() {
            @Override
            public void onListen(Object o, EventChannel.EventSink eventSink) {
                // 调用flutter成功的方法
                MainActivity.this.eventSink = eventSink;
            }

            @Override
            public void onCancel(Object o) {

            }
        });
```

- 当需要给Flutter发送请求的时候
```Java
  //android 7.0 后需要动态注册
    private void initResisgerNetWork() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            //实例化IntentFilter对象
            IntentFilter filter = new IntentFilter();
            filter.addAction("android.net.conn.CONNECTIVITY_CHANGE");
            NetBroadcastReceiver netBroadcastReceiver = new NetBroadcastReceiver();
            //注册广播接收
            registerReceiver(netBroadcastReceiver, filter);
        }
    }


    //网络监听回调
    public class NetBroadcastReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            ConnectivityManager connectivityManager = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo networkInfo = connectivityManager.getActiveNetworkInfo();
            if (networkInfo != null && networkInfo.isAvailable()) {
                //当前网络可以用 
                //发送给flutter 
                if (eventSink != null)
                    MainActivity.this.eventSink.success("当前网络可用");
            } else {
                //当前网络不可用
                if (eventSink != null)
                    MainActivity.this.eventSink.error("当前网络不可用", "网络断开", null);
            }
        }
    }
```
- flutter端定义 通信的常量
```Java
//一开始就得到android发送过来的信息
  static const EventChannel _eventChannel = EventChannel(
      'MyFlutterApp.flutter/begainGetBattery');
```
- 注册接收器

_eventSuccessForAndroid:成功后返回的数据

onError:_eventErrorForAndroid：错误时返回的数据

onDone：_eventDoneForAndroid：完成时返回的数据
```Java
 @override
  void initState() {
    super.initState();
    //类似于接受广播类似的 在这里注册
    _eventChannel.receiveBroadcastStream().listen(
        _eventSuccessForAndroid, onError: _eventErrorForAndroid,
        onDone: _eventDoneForAndroid);
  }
```

参考链接：https://www.jianshu.com/p/717b792fea9e

代码地址：https://github.com/songjiabin/android_flutter_example

