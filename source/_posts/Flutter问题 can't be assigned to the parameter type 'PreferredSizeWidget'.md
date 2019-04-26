---
title: Flutter问题 can't be assigned to the parameter type 'PreferredSizeWidget'
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">自己只会选择放弃的家伙们...却在嘲笑别人的战斗。</blockquote>

<!-- more -->



当我appbar抽出来的时候。如：
```
class getAppBarWidget extends StatelessWidget  {


  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new AppBar(
      title: new Text(this.name),
    );
  }

  final String name;

  getAppBarWidget({Key key, @required this.name}) :super(key: key);
}
```
引用的时候会出现问题
` 'getAppBarWidget' can't be assigned to the parameter type 'PreferredSizeWidget'.`
解决方法
```
class getAppBarWidget extends StatelessWidget implements PreferredSizeWidget {


  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new AppBar(
      title: new Text(this.name),
    );
  }

  final String name;

  getAppBarWidget({Key key, @required this.name}) :super(key: key);

  @override
  // TODO: implement preferredSize
  Size get preferredSize => getSize();


Size getSize() {
    return new Size(100.0, 100.0);
  }
}

```

Scaffold需要appbar作为实现PreferredSizeWidget的类