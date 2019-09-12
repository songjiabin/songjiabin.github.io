---
title: flutter android  混合开发之flutter调用Android方法（先前已有android项目，在此基础上再次集成flutter）
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">云想衣裳花想容，春风拂槛露华浓</blockquote>

<!-- more -->



> 当flutter想和android端进行通信的时候该怎么办呢？

##### 具体步骤
- 导入`import 'package:flutter/services.dart';` 
- 约定好沟通的标识（不可变的某个字段）
```Java
//Flutter
static const MethodChannel _methodChannel = MethodChannel('MyFlutterAPP.flutter.io/getBattery');
//android
public static final String BATTERY_CHANNEL = "MyFlutterAPP.flutter.io/getBattery";
```
- flutter端请求



```Java
//方法是异步的
//getBatteryLevel意思是我要调用android端这个方法。
//result 返回的数据
final String result = await _methodChannel.invokeMethod('getBatteryLevel');
```

- android端处理
```Java
//定义和flutter一样的变量
public static final String BATTERY_CHANNEL = "MyFlutterAPP.flutter.io/getBattery";

// 方法 和回调 
 new MethodChannel((FlutterView) flutterView, BATTERY_CHANNEL).setMethodCallHandler(new MethodChannel.MethodCallHandler() {
            @Override
            public void onMethodCall(MethodCall methodCall, MethodChannel.Result result) {
                //  methodCall 用来区分不同方法的调用
                if (methodCall.method.equals("getBatteryLevel")) {
                    //如果flutter调用的方法是：getBatteryLevel 那么执行
                    //返回成功的回调方法
                    if (true)
                        result.success("100%电量");
                    else {
                        result.error("错误", "", null);
                    }
                } else {
                    //其他方法的调用 表明没有对应实现。
                    result.notImplemented();
                }
            }
        });
```

**用flutter就可以访问android的代码片段了**

![flutter2anroid.gif](https://upload-images.jianshu.io/upload_images/2953304-d183b9ee929dad05.gif?imageMogr2/auto-orient/strip)


代码地址：https://github.com/songjiabin/android_flutter_example