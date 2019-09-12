---
title: flutter问题 RenderCustomMultiChildLayoutBox object was given an infinite size during layout.
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">追攀更觉相逢晚，谈笑难忘欲别前。</blockquote>

<!-- more -->

> 做一个ListView,引入ListView item布局，像这样：

```Java

  @override
  Widget build(BuildContext context) {
    var content = null;
    if (listData.isEmpty) {
      content = CircularProgressIndicator(
        backgroundColor: Colors.green,
      );
    } else {
      content =
      new Container(
        child: ListView.builder(
          itemCount: listData.length,
          //引入Item布局
          itemBuilder: (context, index) {
            return getItemWidget(index);
          },
        ),
      );
    }

    return new Scaffold(
      body: new Center(
        child: content,
      ),
    );
  }
```
当item布局这样写时候，
```Java
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Text('数据'),
    );
  }
```

这样的话  就会遇到这样的问题。问题是，你的子布局没有明确的大小。原来你的item布局不能使用脚手架于是改成这样
```Java
  @override
  Widget build(BuildContext context) {
    return new Container(
      child: Text('数据'),
    );
  }
```

**当item子布局中不用使用脚手架`Scaffold`**
