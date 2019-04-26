---
title: Android材料设计之ToolBar+CardView
date: 
tags: 
- android 
- 材料设计
copyright: true
categories: android
---


<blockquote class="blockquote-center">皑如山上雪，皎若云间月。</blockquote>

<!-- more -->

##### ToolBar

##### 常用属性
```Java
背景------android:background="@color/yase"
阴影------android:elevation="@dimen/dp_4"
图标------app:logo="@drawable/icon_love"
返回图标--app:navigationIcon="@drawable/icon_a"
标题------app:title="捷特"
副标题----app:subtitle="天下无双"
```
###### 基本使用

```Java
<android.support.v7.widget.Toolbar
        android:id="@+id/id_toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/yase"
        android:elevation="@dimen/dp_4" //阴影
        app:logo="@android:drawable/ic_delete"
        app:navigationIcon="@android:drawable/ic_dialog_email"
        app:subtitle="副标题"
        app:title="标题">
    </android.support.v7.widget.Toolbar>
```
- 设置标题和导航键的颜色
```Java
<item name="android:textColorPrimary">@android:color/holo_orange_dark</item>
```
- 副标题的颜色
```Java
<item name="android:textColorSecondary">@android:color/white</item>
```
- 设置菜单
新建menu文件夹->新建.menu文件
```Java
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/test_menu1"
        android:icon="@android:drawable/ic_menu_more"
        android:title="测试1"
        app:showAsAction="never"/><!--有其他的几种情况....-->
    <item
        android:id="@+id/test_menu2"
        android:icon="@android:drawable/ic_menu_month"
        android:title="测试2"
        app:showAsAction="never"/>
    <item
        android:id="@+id/test_menu3"
        android:icon="@android:drawable/ic_menu_slideshow"
        android:title="测试3"
        app:showAsAction="never"/>
</menu>
```
**如果菜单中的文字要显示到ToolBar上，想让给这些文字设置字体颜色**
```Java
<item name="actionMenuTextColor">@android:color/holo_blue_bright</item>
```
- 给菜单设置点击事件、给navigation图标设置点击事件
```Java
    //引入布局
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_toobar, menu);
        return true;
    }
    //点击事件
     @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case android.R.id.home:
                Toast.makeText(MainActivity.this, "点击了navigation图标...", Toast.LENGTH_SHORT).show();
                break;
            case R.id.test_menu1:
                ToastUtils.showMessage(MainActivity.this, "Test1");
                break;
            case R.id.test_menu2:
                ToastUtils.showMessage(MainActivity.this, "Test2");
                break;
            case R.id.test_menu3:
                ToastUtils.showMessage(MainActivity.this, "Test3");
                break;
        }
        return super.onOptionsItemSelected(item);
    }

```
###### 设置溢出菜单图标:
```Java
id_toolbar.setOverflowIcon(getResources().getDrawable(R.mipmap.ic_launcher));
```
###### 修改溢出菜单文字及字体大小,同时也会修改自定义View字体大小,及Toolbar上ActionMenu文字的大小
```Java
<item name="android:textColor">@android:color/holo_purple</item>
    <item name="android:textSize">25dp</item>
```
###### 修改溢出菜单的背景,摆放位置,从系统Overflow样式知道其实就是修改对应属性的值即可

上代码
```Java
    为ToolBar引入的主题
    <style name="AppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar">
        <item name="android:textColorPrimary">@android:color/holo_orange_dark</item>
        <item name="android:textColorSecondary">@android:color/white</item>
        <item name="actionMenuTextColor">@android:color/holo_red_dark</item>
        <item name="android:textColor">@android:color/holo_purple</item>
        <item name="android:textSize">25dp</item>
    </style>

    为ToolBar popular引入的主题
    <style name="AppTheme.PopupOverlay" parent="@style/Widget.AppCompat.Light.PopupMenu.Overflow">
        <item name="overlapAnchor">false</item>
        <item name="android:dropDownWidth">wrap_content</item>
        <item name="android:paddingRight">5dp</item>
        <!--<item name="android:popupBackground">@android:color/holo_orange_dark</item>-->
        <item name="android:colorBackground">@android:color/holo_orange_dark</item>
        <!-- 弹出层垂直方向上的偏移，即在竖直方向上距离Toolbar的距离，值为负则会盖住Toolbar -->
        <item name="android:dropDownVerticalOffset">5dip</item>
        <!-- 弹出层水平方向上的偏移，即距离屏幕左边的距离，负值会导致右边出现空隙 -->
        <item name="android:dropDownHorizontalOffset">0dip</item>
    </style>
```
具体的代码引入
```Java
 <android.support.v7.widget.Toolbar
        android:id="@+id/id_toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/yase"
        android:elevation="@dimen/dp_4"
        app:logo="@android:drawable/ic_delete"
        app:navigationIcon="@android:drawable/ic_dialog_email"
        app:popupTheme="@style/AppTheme.PopupOverlay"
        app:subtitle="副标题"
        app:theme="@style/AppTheme.AppBarOverlay"
        app:title="标题">
        <TextView
            android:id="@+id/tv"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="标题"/>
    </android.support.v7.widget.Toolbar>
```

##### CardView
###### 常用的属性

```Java
 app:cardBackgroundColor      背景颜色
  app:cardCornerRadius         圆角大小
  app:cardElevation            z轴阴影高度
  app:cardMaxElevation         z轴最大高度值

  app:contentPadding           内容与边距的间隔
  app:contentPaddingLeft       内容与左边的间隔
  app:contentPaddingTop        内容与顶部的间隔
  app:contentPaddingRight      内容与右边的间隔
  app:contentPaddingBottom     内容与底部的间隔    

  app:paddingStart             内容与边距的间隔起始
  app:paddingEnd               内容与边距的间隔终止

  app:cardUseCompatPadding     设置内边距，在API21及以上版本和之前的版本仍旧具有一样的计算方式
  app:cardPreventConrerOverlap 在API20及以下版本中添加内边距，这个属性为了防止内容和边角的重叠
  注意：CardView中使用android:background设置背景颜色无效。
```

###### 代码中使用
```Java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp">


    <android.support.v7.widget.CardView
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:clickable="true"
        android:foreground="?android:attr/selectableItemBackground"
        app:cardBackgroundColor="@color/yase"
        app:cardCornerRadius="@dimen/dp_8"
        app:cardElevation="@dimen/dp_4"
        app:cardPreventCornerOverlap="false"
        app:cardUseCompatPadding="true"
        app:contentPadding="10dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:text="基本密码基本密码基本密码基本密码基本密码基本密码"
            android:textSize="16dp"/>
    </android.support.v7.widget.CardView>
</LinearLayout>
```
###### 具体属性的解释

- app:cardUseCompatPadding

![图片](https://upload-images.jianshu.io/upload_images/5586232-37f417d21cc3fa69.png?imageMogr2/auto-orient/)

> 在5.0之前的版本中设置了 app:cardElevation=" "后 CardView 会自动留出空间供阴影显示，而5.0之后的版本中没有预留空间。

解决方法:
app:cardUseCompatPadding="true"  
让CardView在不同系统中使用相同的padding值，为阴影预留空间

- 圆角问题
![图片](https://upload-images.jianshu.io/upload_images/5586232-7b043f4fbc1c8abe.jpg?imageMogr2/auto-orient/)


> 在>=5.0（Lollipop API 21）的版本，CardView会直接裁剪内容元素满足圆角的需求.
在<5.0（Lollipop API 21）的版本，CardView为了使内容元素不会覆盖CardView的圆角，会添加一个padding，这样一来，如果CardView设置了背景颜色，就很难看了.
解决方法：给CardView设置该属性：

```app:cardPreventCornerOverlap="false"```

这条属性的意思是：是否阻止圆角被覆盖，默认为true
设为false后，padding效果就不存在了，同时圆角也被覆盖了
该属性对5.X设备没什么影响.


######  设置涟漪
```Java
android:clickable="true"
android:foreground="?android:attr/selectableItemBackground"
```

