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

为toolBar添加menu
在menu中注册布局文件
```Java
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/test_menu1"
        android:icon="@android:drawable/ic_menu_more"
        android:title="测试1"
        app:showAsAction="never"/>
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


在activity中进行重写onCreateOptionsMenu方法支持menu 
```Java
 @Override
    public boolean onCreateOptionsMenu(Menu menu) {

        getMenuInflater().inflate(R.menu.menu_toobar,menu);//加载menu布局
        return true;
    }
```


设置menu的点击事件
```Java
 //设置toolBar上的MenuItem点击事件
        mToolbar.setOnMenuItemClickListener(new Toolbar.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                switch (item.getItemId()) {
                    case R.id.test_menu1:
                        mToast.setText("click edit");
                        break;
                    case R.id.test_menu2:
                        mToast.setText("click share");
                        break;
                    case R.id.test_menu3:
                        //弹出自定义overflow
                        popUpMyOverflow();
                        return true;
                }
                mToast.show();
                return true;
            }
        });
```



设置主标题  副标题
```Java
  // 主标题,默认为app_label的名字  就是左边的那个标题
        mToolbar.setTitle("Title22222");
        mToolbar.setTitleTextColor(Color.YELLOW);

        // 副标题
        mToolbar.setSubtitle("Sub title");
        mToolbar.setSubtitleTextColor(Color.parseColor("#80ff0000"));
```

设置侧边栏的按钮 比如返回按钮的那个

```Java
//侧边栏的按钮  比如返回的那个   
        mToolbar.setNavigationIcon(android.R.drawable.checkbox_on_background);
        //取代原本的actionbar
        setSupportActionBar(mToolbar);
```

Toolbar中某个控件的点击事件可以使用toolBoar.findViewByid()使用 如：

```Java 
mToolbar.findViewById(R.id.tv_title).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mToast.setText("点击自定义标题");
                mToast.show();
            }
        });

```


点击nemu中的第三个的时候进行 弹框 popuwindow

```Java
 /**
     * 弹出自定义的popWindow
     */
    public void popUpMyOverflow() {
        //获取状态栏高度
        Rect frame = new Rect();
        getWindow().getDecorView().getWindowVisibleDisplayFrame(frame);
        //状态栏高度+toolbar的高度
        int yOffset = frame.top + mToolbar.getHeight();
        if (null == mPopupWindow) {
            //初始化PopupWindow的布局
            View popView = getLayoutInflater().inflate(R.layout.popular_window, null);
            //popView即popupWindow的布局，ture设置focusAble.
            mPopupWindow = new PopupWindow(popView,
                    ViewGroup.LayoutParams.WRAP_CONTENT,
                    ViewGroup.LayoutParams.WRAP_CONTENT, true);
            //必须设置BackgroundDrawable后setOutsideTouchable(true)才会有效
            mPopupWindow.setBackgroundDrawable(new ColorDrawable());
            //点击外部关闭。
            mPopupWindow.setOutsideTouchable(true);
            //设置一个动画。
            mPopupWindow.setAnimationStyle(android.R.style.Animation_Dialog);
            //设置Gravity，让它显示在右上角。
            mPopupWindow.showAtLocation(mToolbar, Gravity.RIGHT | Gravity.TOP, 0, yOffset);
            //设置item的点击监听

        } else {
            mPopupWindow.showAtLocation(mToolbar, Gravity.RIGHT | Gravity.TOP, 0, yOffset);
        }

    }

```

点击结束 popuwindow
```Java
 if (null != mPopupWindow && mPopupWindow.isShowing()) {
            mPopupWindow.dismiss();
        }
```

![demo](http://t1.aixinxi.net/o_1ckregrju1qr11rir1djs1ne6jifa.gif-w.jpg)



