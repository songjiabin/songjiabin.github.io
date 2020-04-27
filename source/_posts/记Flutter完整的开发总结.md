---
title: 记Flutter完整开发总结
tags: 
- Flutter
copyright: true
categories: Flutter
typora-copy-images-to: ..\images
typora-root-url: ..
---

![1577951789158](/images/1577951789158.png)

<!-- more -->

> 之前一直在学习`Flutter`,做一些小实例。没有真正的实战过。巧的是，这次有了一次锻炼的机会，公司要求写一个门店用来管理员工的`app`,我们正好使用了`Flutter`来进行写这个项目。下面我把遇到的问题，用到的相关知识点记录下。

##### 学习到的库

###### 状态管理

传统的想法使用`StatefulWidget`，在`init`中初始化数据，保存数据的状态。当数据量大，操作的数据多的时候，维护起来相当的困难。这个时候使用了`provider`，用来维护界面上的所有数据状态，并且完全和`view`分离，和`android`的`mvp`好像，不过`model`并没有持有`view`对象的引用。使用起来也非常方便，感觉就是一个局部的`eventBus`。

###### 全局消息总栈`event_bus`

和`android`的`eventBus`一样，订阅，接受消息。

###### mqtt_client 

因为登录的时候使用互踢、扫码登录功能，需要使用推送。所以就用到了这个库，

`permission_handler`权限管理库

`dio`不多说，封装起来使用（`json`-->`bean`）。

###### image_picker（照片选择）

###### photo_view (图片预览)

###### connectivity（查询网络状态）

###### flutter_easyrefresh （上拉加载下拉刷新）

###### 等等...

##### 使用到的插件

###### FlutterJsonBeanFactory

将`json`格式的数据直接转成`bean`的插件。

` flutter-img-sync`

图片导入后不需要在`pubspec.yaml`中进行注册，直接可以生产`r.dart`。使用的时候`R..`十分方便





