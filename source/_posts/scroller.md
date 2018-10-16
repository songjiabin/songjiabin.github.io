---
title: Scroller
date: 2018-08-13 14:38:35
tags: 
- 自定义view
- Scroller
copyright: true
categories: android
---



<blockquote class="blockquote-center">业余生活要有意义，不要越轨</blockquote>

<!-- more -->



#### Scroller

> **Scroller是一个专门用于处理滚动效果的工具类，可能在大多数情况下，我们直接使用Scroller的场景并不多，但是很多大家所熟知的控件在内部都是使用Scroller来实现的，如ViewPager、ListView等。而如果能够把Scroller的用法熟练掌握的话，我们自己也可以轻松实现出类似于ViewPager这样的功能。**

>**自己理解：我把scroller理解为滚动数据携带器，他只是一个记录滚动数据的工具，并不显示view的滚动效果，其实这点我觉得和安卓的属性动画挺像，他只提供数据。**


任何一个控件都是可以滚动的，因为在View类当中有scrollTo()和scrollBy()这两个方法。
这个两个方法操作的事mScrollX,mScrollY。两个属性用来偏移位置。而用了操纵这两个属性的方法是 scrollTo、scrollBy 两个方法。[这里](https://jjmima.top/2018/08/13/scrollTo%20%E4%BB%A5%E5%8F%8A%20%20scrollBy/#more)


当我们使用这个两个方法的时候。可以发现动作是瞬间完成，跳跃式的。这个时候就要用到了Scroller。
 
自定义一个ViewGroup。仿照ViewPager可以滑动。 
 
 #### 创建Scroller的实例 
 ```Java
 private void initView() {
        mScroller = new Scroller(getContext());
        //得到最小平移的距离 如果手指移动距离大于这个距离 就说明移动了
        mTouchSlop = ViewConfiguration.get(getContext()).getScaledPagingTouchSlop();
    }
 ```
其中需要测量里面每个view的大小。
```Java
@Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        for (int i = 0; i < getChildCount(); i++) {
            View view = getChildAt(i);
            //对每个View进行测量
            view.measure(widthMeasureSpec, heightMeasureSpec);
        }
    }
```
测量后对每个View进行布局。让其从左到右排列到屏幕上
```Java
 @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        if (changed) {
            for (int i = 0; i < getChildCount(); i++) {
                View view = getChildAt(i);
                view.layout(i * view.getMeasuredWidth(), 0, (i + 1) * view.getMeasuredWidth(), view.getMeasuredHeight());
            }
            //得到边界值
            zLeft = getChildAt(0).getLeft();
            zRight = getChildAt(getChildCount() - 1).getRight();
            Log.d("边界是：", zLeft + "");
        }
    }
```
#### 调用startScroll()方法来初始化滚动数据并刷新界面
其实就是定义好需要scroll的左标位置。
```Java
 前面的两个参数是起始坐标x,y,中间两个参数是对应的偏移量，最后一个参数是执行时间。
 mScroller.startScroll(getScrollX(), 0, dx, 0, 2000);
```
#### 重写computeScroll()方法，并在其内部完成平滑滚动辑
当调用了startScroll后，重写computeScroll()。在onDraw(方法中会一直调用此方法。在此方法中，再进行具体的坐标的操作
```Java 
@Override
    public void computeScroll() {
        // 第三步，重写computeScroll()方法，并在其内部完成平滑滚动的逻辑
        //当还没有平滑完成的时候 、
        // 判断是否完成滚动，如果没有滚动完成返回True(你要对其进行操作),如果返回false 表示不再滚动啦
        if (mScroller.computeScrollOffset()) {
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            invalidate();
        }
    }
```
 
最后在布局文件中调用
```Java
<com.example.sonbjiabin.myapplication.MyScrollerView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="300dp">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@android:color/holo_red_dark" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@android:color/holo_blue_bright" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="@android:color/holo_green_dark" />

</com.example.sonbjiabin.myapplication.MyScrollerView>
```
 
[原文链接](https://blog.csdn.net/guolin_blog/article/details/48719871) 
 
 
 
 
#### 另外一个demo
 
 > **ViewGroup中有个computeScroll方法，ontouch或invalidate(）或postInvalidate()都会导致这个方法的执行，
所以我们可以手动执行ViewGroup中以上方法，同时再computeScroll中执行postInvalidate()，这就会形成一个循环，我们在这个循环中调用ViewGroup的scrollTo方法更新位置信息，同时使用mScroller.computeScrollOffset()方法监听滚动是否完毕。**

 
 
 
 ```Java
 public class MyScrollerView extends RelativeLayout {


    private boolean s1 = true;
    Scroller mScroller = null;

    public MyScrollerView(Context context, AttributeSet attrs) {
        super(context, attrs);
        mScroller = new Scroller(context);
        // TODO Auto-generated constructor stub
    }

    @Override
    public void computeScroll() {
        if (mScroller.computeScrollOffset()) {
            if (mScroller.computeScrollOffset()) {
                Log.d("》》》", "11111111");
            }
            scrollTo(mScroller.getCurrX(), 0);
            postInvalidate();
        }
    }

    //需要手动执行这个方法
    public void beginScroll() {
        if (!s1) {
            mScroller.startScroll(0, 0, 0, 0, 10000);
            s1 = true;
        } else {
            mScroller.startScroll(0, 0, 500, 0, 10000);
            s1 = false;
        }
        invalidate();
    }

}
 ```
[此文原地址](https://www.jianshu.com/p/77210c03e0df)
 
 
 
 