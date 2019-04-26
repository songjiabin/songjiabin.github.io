---
title: Android面试（二）
date: 
tags: 
- android 
- 面试
copyright: true
categories: android
---



<blockquote class="blockquote-center">长风破浪会有时，直挂云帆济沧海</blockquote>

<!-- more -->

 



###### Handler 
![360截图20181025233045532.jpg](https://upload-images.jianshu.io/upload_images/2953304-b35eeec404c8ac14.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 当创建Handler的时候会关联当前线程的looper。（每个线程都有一个looper进行循环处理消息队列中的信息）。通looper获取消息队列。Handler发送消息到消息队列中。looper循环去取出消息进行处理。再交给Handler来处理。

从源码分析
```Java

 public Handler(Callback callback, boolean async) {
        ...
        //创建looper对象。
        mLooper = Looper.myLooper();
        //如果looper为空的话报错，因为你没有执行Looper.prepare()方法。
        //看looper源码可以看出Looper.prepare()中是得到唯一的一个Looper对象。（一个线程中只能对应一个looper）
        if (mLooper == null) {
            throw new RuntimeException(
                "Can't create handler inside thread that has not called Looper.prepare()");
        }
        //得到Looper关联的队列。(looper循环队列，从里面得到消息数据。)
        mQueue = mLooper.mQueue;
        mCallback = callback;
        mAsynchronous = async;
    }
```

上面可以看到在初始化Handler的时候，初始化了looper对象。looper对象中对象又赋值给了Handler。所以handler是关联了looper又关联了队列。它和looper公共一个消息队列。

Handler的作用是
- 发送消息


在handler的所有发送消息后都会走到这个方法。
```Java

 private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
 //handler自己发送消息。message将自己内部的一个属性赋值给target属性。
        msg.target = this;
        if (mAsynchronous) {
            msg.setAsynchronous(true);
        }
    //将消息发送到消息队列中
        return queue.enqueueMessage(msg, uptimeMillis);
    }
```

- 处理消息
在looper.looper()方法中
```Java
//死循环迭代消息
.... 
for (;;) {
//从消息容器里面取出消息
            Message msg = queue.next(); // might block
            if (msg == null) {
                // No message indicates that the message queue is quitting.
                return;
            }
...

        try {
        //msg.target 就是刚才赋值的handler
        //调用hander的dispatchMessage()方法
            msg.target.dispatchMessage(msg);
            } finally {
                if (traceTag != 0) {
                    Trace.traceEnd(traceTag);
                }
            }

```
Hadnler中的dispatchMessage()查看

```Java
 public void dispatchMessage(Message msg) {
        // msg.CallBack  就是当Handler.post(run..);
        //就是那个run方法。
        //如果 run方法不为空那么处理run方法。
        if (msg.callback != null) {
            handleCallback(msg);
        } else {
        //当 run方法为空的话。并且 mCallBack不为空的话
        //mCallback 为 接口。里面实现了 handleMessage接口。也就是当我们需要处理的逻辑。
            if (mCallback != null) {
            //处理 handleMessage 方法
                if (mCallback.handleMessage(msg)) {
                    return;
                }
            }
            handleMessage(msg);
        }
    }
```
- 总结

Looper类主要是为了每个线程开启单独的消息循环

Handler可以看做是Looper的一个接口。用来向指定Looper中的messageQueue中发送消息

在非主线程中直接new Handler()是不可以的。因为Handler需要发送消息到messageQueu。而你所在的线程中没有MessageQeue。而MessageQueue是由Looper管理的。所以如果你想在非线程中使用，必须先创建出Looper。




