---
title: 熟悉使用ToolBar
date: 2018-08-13 14:38:35
tags:
copyright: true

---


<blockquote class="blockquote-center">优秀的人，不是不合群，而是他们合群的人里面没有你</blockquote>





> ToolBar的出现是为了替换之前的ActionBar的各种不灵活使用方式,相反,ToolBar的使用变得非常灵活,因为它可以让我们自由往里面添加子控件.低版本要使用的话,可以添加support-v7包.

要使用这个的话首先为了兼容低版本
```Java
compile 'com.android.support:appcompat-v7:25.3.1'
```
其次要隐藏默认的ActionBar 否则会报错
```Java
Caused by: java.lang.IllegalStateException: This Activity already has an action bar 
supplied by the window decor. Do not request Window.FEATURE_SUPPORT_ACTION_BAR and set 
windowActionBar to false in your theme to use a Toolbar instead.
```
要隐藏默认的ActonBar的话可以再style中进行设置
```Java
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!--状态栏颜色-->
        <item name="colorPrimaryDark">@color/Indigo_colorPrimaryDark</item>
        <!--Toolbar颜色-->
        <item name="colorPrimary">@color/Indigo_colorPrimary</item>
        <!--返回键样式-->
        <item name="drawerArrowStyle">@style/AppTheme.DrawerArrowToggle</item>

        <!--colorAccent 对应EditText编辑时、RadioButton选中、CheckBox等选中时的颜色。-->
        <item name="colorAccent">@color/colorYellow</item>


        <!--将ActionBar隐藏,这里使用ToolBar-->
        <item name="windowActionBar">false</item>
        <!-- 使用 API Level 22以上编译的话，要拿掉前綴字 -->
        <item name="windowNoTitle">true</item>
        <!--窗口的颜色-->
        <item name="android:windowBackground">@android:color/holo_blue_bright</item>

    </style>
```
从上面的style文件中,可以知道,手机状态栏的颜色和ToolBar的颜色也是可以在style中配置的.然后在清单文件的application节点下需要确认使用的style是android:theme=”@style/AppTheme”

开始创建xml布局文件
```Java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <!--
  ?attr/actionBarSize:表示根据屏幕的分辨率采用系统默认的高度
  如果低版本也要使用的话,则需要使用v7包的,否则只有api21上才能有效
  -->

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="?attr/colorPrimary">
        <!--添加Toolbar的子控件-->
        <Button
            android:id="@+id/btn_diy"
            android:layout_width="60dp"
            android:layout_height="wrap_content"
            android:layout_gravity="right"
            android:background="#80ffffff"
            android:text="自定义按钮"
            android:textColor="#000000"
            android:textSize="11sp"/>

        <TextView
            android:id="@+id/tv_title"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:gravity="center"
            android:text="首页"
            android:textColor="@android:color/black"
            android:textSize="20sp"/>

    </android.support.v7.widget.Toolbar>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:text="hello_world"
        android:textColor="@android:color/black"
        android:textSize="30sp"/>

</LinearLayout>
```