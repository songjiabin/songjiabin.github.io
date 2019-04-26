---
title: Flutter 学习
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">最伤人的，不适你爱的人已经不在了，而是他的世界已没有你的驻足之地。</blockquote>

<!-- more -->




###### 程序的入口
> lib -> main.dart 

```Java
import 'package:flutter/material.dart';

void main() {
  runApp(new Center(
    child: new Text(
      'Flutter',
      textDirection: TextDirection.ltr,
    ),
  ));
}
```
- runApp 
> 该runApp函数接受给定的Widget并使其成为widget树的根。用到了Center和Text两个weight。**框架强制根widget覆盖整个屏幕**
- textDirection
> 文本指定的方向

##### widget 
> 创建新的widget，这些widget是无状态的StatelessWidget或者是有状态的StatefulWidget， 具体的选择取决于您的widget是否需要管理一些状态。widget的主要工作是实现一个build函数，用以构建自身。一个widget通常由一些较低级别widget组成。Flutter框架将依次构建这些widget，直到构建到最底层的子widget时，这些最低层的widget通常为RenderObject，它会计算并描述widget的几何形状。

###### 基础 Widget
- Text 
> 该 widget 可让创建一个带格式的文本。








- Row、 Column：
> 这些具有弹性空间的布局类Widget可让您在水平（Row）和垂直（Column）方向上创建灵活的布局。其设计是基于web开发中的Flexbox布局模型。
- Stack ：
> 取代线性布局 (译者语：和Android中的LinearLayout相似)，Stack允许子 widget 堆叠， 你可以使用 Positioned 来定位他们相对于Stack的上下左右四条边的位置。Stacks是基于Web开发中的绝度定位（absolute positioning )布局模型设计的。
- Container
> Container 可让您创建矩形视觉元素。container 可以装饰为一个BoxDecoration, 如 background、一个边框、或者一个阴影。 Container 也可以具有边距（margins）、填充(padding)和应用于其大小的约束(constraints)。另外， Container可以使用矩阵在三维空间中对其进行变换。










#####小实例
```Java
import 'package:flutter/material.dart';

void main() {
  runApp(new MaterialApp(
    title: '我的app',
    home: new MyScaffold(),
  ));
}


//home 板块
class MyScaffold extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    // Material类型的主题样式 
    // // Material 是UI呈现的“一张纸”
    return new Material(
    //其中包含 一个Column(列)
      child: new Column(
        children: <Widget>[
         //列里面包含了一个appbar
          new AppBar(
            //里面包含了title,title里面是Text 
            title: new Text('home模块',
              //主题
              style: Theme.of(context).primaryTextTheme.title,),
          )
        ],
      ),
    );
  }
}

```
样式为：
 ![TIM截图20190327164404.png](https://upload-images.jianshu.io/upload_images/2953304-86b5229e9df57e8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**请确保在pubspec.yaml文件中。将flutter的值设置为：uses-material-design: true**

```Java
name: my_app
flutter:
  uses-material-design: true
```




##### 写一个有状态的widghet
> 当点击按钮的时候 动态的改变文字 

```Java
class otherPressStatele extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      home: new otherPressFulWidght(),
    );
  }

}

class otherPressFulWidght extends StatefulWidget {

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _otherPressFulwWight();
  }

}


class _otherPressFulwWight extends State<otherPressFulWidght> {

  String name = "我是Title";

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(name),
      ),
      floatingActionButton: new FloatingActionButton(onPressed: _updateText),
    );
  }


  void _updateText(){
    setState(() {
      name='我是谁呢？';
    });
  }
}

```


##### padding widght 学习 
> 样式

```Java
class myPaddingWidght extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      title: "数据",
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text("标题"),
        ),
        body: new Center(
          child: new MaterialButton(
              onPressed: () {
                print('11111');
              },
              child: new Text("Hellow"),
              padding: EdgeInsets.only(left: 100, right: 50, top: 20)),
        ),
      ),
    );
  }
}
```

##### 删除或者是增加 Widget
```Java
void main() {
  runApp(new sampleApp());
}


class sampleApp extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      title: '',
      home: new homePage(),
    );
  }
}


class homePage extends StatefulWidget {

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _homePage();
  }
}

class _homePage extends State<homePage> {

  bool toggle = true;

  void _toggle() {
    setState(() {
      this.toggle = !toggle;
    });
  }

  _getToggleChild() {
    if (this.toggle) {
      return new Text(
        "Toggle True", style: TextStyle(color: Colors.red, fontSize: 20),);
    } else {
      return new Text(
        "Toggle Two", style: TextStyle(color: Colors.blue, fontSize: 20),);
    }
  }

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('标题'),
      ),
      body: new Center(
        child: _getToggleChild(),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _toggle, tooltip: 'press', child: Icon(Icons.public),),
    );
  }


}

```

##### 自定义的View

> 自定义了一个可以写入颜色和文字的按钮

```Java


void main() {
  runApp(new FadeAppTest());
}

class FadeAppTest extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
        title: 'title',
        theme: new ThemeData(
            primarySwatch: Colors.blue
        ),
        home: new customButton(title: '标题在这里呢！', color: Colors.blue,)
    );
  }
}


class customButton extends StatelessWidget {

  final String title;
  final Color color;

  customButton({@required this.title, @required this.color});

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new RaisedButton(onPressed: () {},
      child: new Text(this.title, style: TextStyle(color: this.color),),);
  }
}


```


##### 设置透明度
>  Opacity 属性

```Java
void main() {
  runApp(new FadeAppTest());
}

class FadeAppTest extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
        title: 'title',
        theme: new ThemeData(
            primarySwatch: Colors.blue
        ),
        home: new customButton(title: '标题在这里呢！', color: Colors.blue,)
    );
  }
}


class customButton extends StatelessWidget {

  final String title;
  final Color color;

  customButton({@required this.title, @required this.color});

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new Center(
      child: new RaisedButton(onPressed: null,
        child: new Opacity(opacity:1,
          child: new Text(this.title, style: TextStyle(color: this.color),),),),
    );
  }
}

```

##### Container 
> 控制一个布局的样式和属性。

```Java
void main() {
  runApp(new FadeAppTest());
}


class FadeAppTest extends StatelessWidget {


  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      home: new Center(
        child: new Column(
          children: <Widget>[
            new Container(
              color: Colors.red,
              width: 100,
              height: 100,
              child: new Text(
                '我的名字', style: TextStyle(fontSize: 20, color: Colors.orange),),
            ),
            new Container(
              color: Colors.blue,
              width: 100,
              height: 100,
              child: new RaisedButton(
                onPressed: () {}, child: new Text('我的名字'),),
            ),
            new Container(
              color: Colors.yellow,
              width: 100,
              height: 100,
            )
          ],
        ),
      ),
    );
  }
}
```

##### Stack 控件将其子项相对于其框的边缘定位。如果想重叠多个子窗口小部件，这个类很受用。

##### ListView
```Java
void main() {
  runApp(new listViewWidght());
}


class listViewWidght extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      title: 'Title',
      home: new listViewPage(),
    );
  }

}

class listViewPage extends StatefulWidget {


  listViewPage({Key key}) :super(key: key);

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _listViewPage();
  }

}

class _listViewPage extends State<listViewPage> {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Title'),
      ),
      body: new ListView(children: _getListData()),
    );
  }

  _getListData() {
    List<Widget> listWiget = [];
    for (int i = 0; i < 1000; i++) {
      listWiget.add(
          new Padding(padding: EdgeInsets.all(10), child: new Text('我的数据$i'),));
    }
    return listWiget;
  }

}

```


##### 改变listView状态的时候,动态改变数据的时候（比较低效的方法）
```Java
void main() {
  runApp(new listViewWidght());
}

class listViewWidght extends StatefulWidget {


  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _listViewWidght();
  }

  listViewWidght({Key key}) :super(key: key);

}


class _listViewWidght extends State<listViewWidght> {

  List<Widget> widgets = [];

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    for (int i = 0; i < 100; i++) {
      widgets.add(getRow(i));
    }
  }

  Widget getRow(int i) {
    return new GestureDetector(
      child: new Padding(
        padding: new EdgeInsets.all(10), child: new Text('Row$i'),),
      onTap: () {
        setState(() {
        //重新创建一个可变长度的数组
          this.widgets = List.from(widgets);
          widgets.add(getRow(this.widgets.length + 1));
          print('row$i');
        });
      },
    );
  }


  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
        home: new Scaffold(
          appBar: new AppBar(
            title: new Text('Title'),
          ),
          body: ListView(children: this.widgets),
        )
    );
  }

}

```

##### GestureDetector
> 在Android中所有View都可以设置OnClick事件，但是在Flutter中除开少数自带Press事件的widget，大部分控件都是不带事件的，如果需要添加事件，就可以用GestureDetector作为父widget包裹需要添加事件的widget

```Java

  Widget getWidget(int i) {
    return new GestureDetector(
      child: new Container(
        width: 200, height: 200, child: new Text('这个是数据哦--> $i',textDirection: TextDirection.ltr,),color: Colors.blue,),
      //点击事件
      onTap: () {
        setState(() {
          //重新创建一个可变长度的数组
          this.listWidget = List.from(this.listWidget);
          this.listWidget.add(getWidget(this.listWidget.length + 1));
        });
      },
      onLongPress: (){
        print('222');
      },
      onDoubleTap: (){
        print('双击');
      },
    );
  }
```

#####  使用ListView.builder方式
```Java
class ohterListView extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new MaterialApp(
      title: 'title',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: new sampleAppPage(),
    );
  }

}

class sampleAppPage extends StatefulWidget {


  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _sampleAppPage();
  }

  sampleAppPage({Key key}) :super(key: key);

}


class _sampleAppPage extends State<sampleAppPage> {

  List<Widget> listWidget = [];


  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    for (int i = 0; i < 1; i++) {
      this.listWidget.add(this.getWidget(i));
    }
  }


  Widget getWidget(int i) {
    return new GestureDetector(
      child: new Padding(padding: EdgeInsets.all(10), child: new Text('第$i列'),),
      onTap: () {
        setState(() {
//          this.listWidget = List.from(this.listWidget);
          this.listWidget.add(this.getWidget(this.listWidget.length + 1));
        });
      },
    );
  }


  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Title'),
      ),
      //使用ListView.builder()创建 
      body: ListView.builder(itemBuilder: (BuildContext context, int position) {
        return this.getWidget(position);
      }),
    );
  }
}

```

