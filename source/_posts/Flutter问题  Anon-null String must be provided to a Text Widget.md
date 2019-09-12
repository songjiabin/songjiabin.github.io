---
title: Flutter问题  Anon-null String must be provided to a Text Widget
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">相信你做得到，你一定会做到。不断告诉自己某一件事，即使不是真的，最后也会让自己相信。</blockquote>

<!-- more -->
> 自定义了一个Text类，构造方法中传入了Title。当调用的时候总是报这个错误。意思是传过来的事null。

代码如下:
```Java
import 'package:flutter/material.dart';

/**
 * 每个Widget
 */
class EachWidget extends StatefulWidget {

  String _title;

  @override
  EachWidgetWidgetState createState() => new EachWidgetWidgetState();

  EachWidget(@required String _title);

}

class EachWidgetWidgetState extends State<EachWidget> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(this.widget._title,
          textDirection: TextDirection.ltr,),
      ),
      body: new Center(
        child: new Text(this.widget._title,
          textDirection: TextDirection.ltr,),
      ),
    );
  }


  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    print(widget._title);
  }
}
```


 经过返回查找原来：
 ```Java
 //构造方法中加上 this 
  EachWidget(@required String  this._title);
 ```
 低级错误...
 
 
 问题解决 