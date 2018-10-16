---
title: View 事件体系
date: 2017-06-02
tags: 
- android 
copyright: true
categories: android
---


<blockquote class="blockquote-center">事业无穷年</blockquote>

<!-- more -->



#### View事件的分发机制概要
> 一般会用到三个方法。即：

- **dispatchTouchEvent**   处理事件的分发  
- **onInterceptTouchEvent**   判断当前ViewGroup 是否拦截事件 (ViewGroup中有此方法，View中没有 ps:因为View下面没有子类了，不需要拦截了。)
- **onTouchEvent**  处理事件

#### 事件的分发是顺序执行的

看一个非常著名的图：

![20180710162702968.png](https://upload-images.jianshu.io/upload_images/2953304-33edd6bc1aefe039.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 当一个Touch事件(触摸事件为例)到达根节点，即Acitivty的ViewGroup时，它会依次下发，下发的过程是调用子View(ViewGroup)的dispatchTouchEvent方法实现的。简单来说，就是ViewGroup遍历它包含着的子View，调用每个View的dispatchTouchEvent方法，而当子View为ViewGroup时，又会通过调用ViwGroup的dispatchTouchEvent方法继续调用其内部的View的dispatchTouchEvent方法。上述例子中的消息下发顺序是这样的：①-②-⑤-⑥-⑦-③-④。dispatchTouchEvent方法只负责事件的分发，它拥有boolean类型的返回值，当返回为true时，顺序下发会中断。在上述例子中如果⑤的dispatchTouchEvent返回结果为true，那么⑥-⑦-③-④将都接收不到本次Touch事件。
---
**事件的分发是顺序执行的。当当前的View中的dispatchTouchEvent返回True。即拦截了事件。那么下面的所有的子View都无法接收到事件的传播。**

#### 事件传递大致流程

而触发时候的大致流程是这样的
> 一个触摸事件，如果事件坐标处于ViewGroup所“管辖范围”，首先调用的是该ViewGroup的dispatchTouchEvent函数，dispatchTouchEvent函数内部调用onInterceptTouchEvent函数，用于判断是否拦截该事件，如果拦截，则调用ViewGroup的onTouchEvent。

![360截图20180918235255639.jpg](https://upload-images.jianshu.io/upload_images/2953304-b69008746cf8649c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**上图的是事件从上到下的传递流程。没有子View的onTouchEvent的返回值。即没有子View向上回传的过程。**



#### 事件系列
> View的事件体系中，从down->move->……->move->up。这一个过程为同一个事件系列。
ViewGroup是否拦截事件，是通过onTnterceptTouchEvent返回值来确定，当返回true时，表示拦截该事件，那么该系列事件全部传递给ViewGroup的onTouchEvent，如果返回false，则表示不拦截该系列事件，该系列事件全部交给子View来处理。


#### 例子
定义ViewGroup

```Java
public class ViewGroupA extends LinearLayout {

    public ViewGroupA(Context context) {
        super(context);
        initContext();
    }

    public ViewGroupA(Context context, AttributeSet attrs) {
        super(context, attrs);
        initContext();
    }

    public ViewGroupA(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initContext();
    }


    private void initContext(){
        this.setOrientation(VERTICAL);
    }


    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        Log.i("info","ViewGroup - dispatchTouchEvent");
        return true;
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        Log.i("info","ViewGroup - onInterceptTouchEvent");
        return super.onInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.i("info","ViewGroup - onTouchEvent");
        return super.onTouchEvent(event);
    }

}
```
定义View
```Java
public class ViewA extends android.support.v7.widget.AppCompatTextView {

    public ViewA(Context context) {
        super(context);
    }

    public ViewA(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public ViewA(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    public boolean dispatchTouchEvent(MotionEvent event) {
        Log.i("info","ViewZi - dispatchTouchEvent");
        return super.dispatchTouchEvent(event);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.i("info","ViewZi - onTouchEvent");
        return super.onTouchEvent(event);
    }
}
```

布局代码
```XML
 <com.example.sonbjiabin.myapplication.event.ViewGroupA
        android:layout_width="match_parent"
        android:background="@android:color/black"
        android:layout_height="400dp">

        <com.example.sonbjiabin.myapplication.event.ViewA
            android:layout_width="match_parent"
            android:layout_height="80dp"
            android:background="@android:color/holo_green_dark" />
        <com.example.sonbjiabin.myapplication.event.ViewA
            android:layout_width="match_parent"
            android:layout_height="80dp"
            android:background="@android:color/holo_blue_dark"/>
        <com.example.sonbjiabin.myapplication.event.ViewA
            android:layout_width="match_parent"
            android:layout_height="80dp"
            android:background="@android:color/holo_red_dark" />
    </com.example.sonbjiabin.myapplication.event.ViewGroupA>
```
activity
```Java
public class MyEventActivity extends Activity implements View.OnClickListener ,View.OnTouchListener{


    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_view_event);
        ViewGroup ViewGroupA = (ViewGroup) findViewById(R.id.ViewGroupA);
        ViewGroupA.setOnClickListener(this);
        ViewGroupA.setOnTouchListener(this);
    }


    @Override
    public void onClick(View v) {
        Log.i("info", "Activity - onClick");
    }

    @Override
    public boolean onTouch(View v, MotionEvent event) {
        Log.i("info", "Activity - onTouch");
        return false;
    }
}
```



布局结构是这样的。
最外层是 ViewGroupA 。里面是ViewA1,ViewA2,ViewA3。

尝试：
- 分析

- [x] 点击 ViewGroupA 的时候。
打印如下：

```
com.example.sonbjiabin.myapplication I/info: ViewGroup - dispatchTouchEvent
com.example.sonbjiabin.myapplication I/info: ViewGroup - dispatchTouchEvent
```

因为 ViewGroupA中的 dispatchTouchEvent 的方法返回的是true。即：拦截事件。
当事件被拦截了。 
所以使其返回false。重新运行，打印的结果如下：
```Java 
com.example.sonbjiabin.myapplication I/info: ViewGroup - dispatchTouchEvent
com.example.sonbjiabin.myapplication I/info: Activity - dispatchTouchEvent
```
显然：打印结果并没不是我想要出现的结果。事件还是没有进行传递。

> **原因：当ViewGroup 的dispatchTouchEvent 方法 返回 false 时 事件分发为两种情况。一：如果ViewGroup获取的事件是从Activity传递过来的。则将会将事件返回到Activity中的onTouchEvent去执行。正如打印的结果是这样的。
二：如果此ViewGroup还有父类的话，那么此事件会传给父类来处理。会将事件传递给父类中的onTouchEvent方法。**


>> **注意：还有一种情况就是 dispatchTouchEvent 返回 super.dispatchTouchEvent(ev);当前事件会分发给当前的View的onInterceptTouchEvent方法。如果onInterceptTouchEvent不处理。即：返回的是false。那么事件会继续向下传递。 这种情况的话。**


当点击的是ViewGroupA的时候
```Java
com.example.sonbjiabin.myapplication I/info: ViewGroup - dispatchTouchEvent
ViewGroup - onInterceptTouchEvent
```

当点击的是 View1的时候 。
```Java 
ViewGroup - dispatchTouchEvent
ViewGroup - onInterceptTouchEvent
ViewZi - dispatchTouchEvent
ViewZi - onTouchEvent
```


事件是从上到下逐级传递的。

此时事件要最交给最后接收到消息的View处理OnTouchEvent。如果返回true,表示处理。不在向上传递。如果返回false，表示此事件不处理，把处理事件的权限交给父类的ouTouchEvent。如果最上层的ViewGroup中的ouTouchEvent也返回的是false。那么将会将事件传递给Activity.

打印：

```Android
ViewGroup - dispatchTouchEvent
ViewGroup - onInterceptTouchEvent
ViewZi - dispatchTouchEvent
ViewZi - onTouchEvent
```



关于回传的图片

 
![360截图20180920232308446.jpg](https://upload-images.jianshu.io/upload_images/2953304-e5595649227dbbd5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




#### 总结

 

- **如果View只消耗down事件，而不消耗其他事件，那么其他事件不会回传给ViewGroup，而是默默的消逝掉。我们知道，一旦消耗down时间，接下来的该系列所有的事件都会交给这个View，因此，如果不处理down以外的事件，这些事件就会被“遗弃”。** 

- **如果ViewGroup决定拦截（即：onInterceptTouchEvent返回的事true），那么这个系列事件都只能由它处理，并且onInterceptTouchEvent不会再被调用。**



- **某个View，在onTouchEvent中，如果针对最开始的down事件都返回false，那么接下来的事件系列都不会交给这个View。接下来的所有事件都会交给上面的父View中onTouchEvent()处理**

- **ViewGroup默认不拦截事件，即onInterceptTouchEvent默认返回false。** 

- **View的onTouchEvent默认返回false，即不消耗事件。**


- **View没有onInterceptTouchEvent方法。因为根本不需要拦截。因为根本不会向下传递了**


- **ViewGroup在传递触摸事件时，会遍历子View，判断触摸点是否在各个子View中。如果在，则触发调用相关函数。如果点击的位置没有子View，那么不管onIntercepTouchEvent返回的是什么，ViewGroup的onTouchEvent都会执行！**



#### 补充 
**onTouch、onClick、onTouchEvent 执行顺序**
> 执行的顺序是：onTouch->onTouchEvent->onClick。当onTouch返回false时，onTouchEvent才会执行，当onTouchEvent显式调用onClick时，onClick才会执行。

[参考地址](https://blog.csdn.net/huachao1001/article/details/51766225)