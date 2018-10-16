---
title: okhttp
date: 
tags: 
- android 
- okhttp
- 源码分析
copyright: true
categories: android
---


<blockquote class="blockquote-center">人若有志，万事可为</blockquote>

<!-- more -->

##### OkHttpClient 
> 所有的http请求客户端的一个类，在执行的时候只会创建一次。将其作为全局的实例进行保存。(其实就是单例的)

```
OkHttpClient okHttpClient = new OkHttpClient.Builder().readTimeout(10, TimeUnit.SECONDS).build();
```

##### Request 封装了请求的报文信息
> 封装了一些请求报文信息。比如请求的方法（GET、POST）、请求的地址、请求的参数等。

```
Request request = new Request.Builder().url("http://www.baidu.com").get().build();
```

##### Call 实际的请求，连接Request和Response的桥梁
> 实现类是RealCall 

```
 //连接 resquest  response 的桥梁
Call call = okHttpClient.newCall(request);

                try {
                    Response response = call.execute();
                    String okhttpString = response.body().toString();
                    Log.i("》》》", okhttpString);
                } catch (IOException e) {
                    e.printStackTrace();
                }
```


##### Dispatcher 分发器。
> 里面维护了线程池。执行我们的网络请求。包括同步和异步。进行同步异步请求的分发。 
维护请求的状态，并维护一个线程池，用于执行请求。
##### interceptors 拦截器
- -  - 


##### 基本流程：
1、创建一个Okhttpclient对象
2、构建一个Request对象，通过Okhttpclient和Request对象，构建Call对象。
3、执行call对象的execute（同步）或者enqueue（异步）方法


##### Dispatcher 大体流程
![HKO_JHZQZ(~2_T4BZN~YQ10.png](https://upload-images.jianshu.io/upload_images/2953304-fadae68c4ef6bb9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 右边的线程池用于进行网络请求。如果是同步的话，当满足条件后会将请求放到线程池中。通过线程池来满足任务。如果是异步的话，判断执行的异步队列有没有达到要求，如果达到要求的话，直接添加到执行的队列中去，否则的话添加到等待队列中去。





