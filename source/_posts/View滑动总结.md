---
title: View滑动总结
date: 2018-08-13 14:38:35
tags:
copyright: true
categories: android
---



<blockquote class="blockquote-center">气不和时少说话，有言必失；心不顺时莫做事，做事必败。</blockquote>

<!-- more -->

> View的滑动是Android自定义控件的基础，在开发中我们难免会遇到View的滑动处理。
其实不管是哪种滑动方式，基本思想都是差不多的：
- 当点击事件传到View时，系统记下触摸点的坐标；
- 手指移动时系统记下移动后触摸的坐标并算出偏移量，并通过偏移量来修改View的坐标；

实现View滑动有很多种方法，这里主要讲下以下6种：
- layout()
- offsetLeftAndRight（）与offsetTopAndBottom（）
- LayoutParams
- 动画
- scollTo 与 scollBy
- Scroller

#### layout()方式
首先看一下layout()，在View进行绘制时，会调用OnLayout()方法来设置View显示的位置，同时可以修改View的left,top,right,bottom四个属性来控制View的坐标。这个坐标是View坐标系。

![2922217-b01724b799d440fa.png](https://upload-images.jianshu.io/upload_images/2953304-a3b5d3855f05adee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


代码如下
```Java
    @Override
    public boolean onTouchEvent(MotionEvent event) {

        int x = (int) event.getX();
        int y = (int) event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                lastX = x;
                lastY = y;
                break;
            case MotionEvent.ACTION_MOVE:
                //得到偏移量
                int offsetX = x - lastX;
                int offsetY = y - lastX;
                //四个参数，left,top,right,bottom
                layout(getLeft() + offsetX, getTop() + offsetY, getRight() + offsetX, getBottom() + offsetY);
                break;
            default:
                break;
        }
        return true;
    }

```

#### offsetLeftAndRight（）与offsetTopAndBottom（）方式
> 这种方式和layout()的方式基本是一样的,根据名字也知道这两个方法分别是设置左边和右边,上面和下面的偏离值
其实吧用法是上面的layout()的用法是一样。

```Java
@Override
    public boolean onTouchEvent(MotionEvent event) {

        int x = (int) event.getX();
        int y = (int) event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                lastX = x;
                lastY = y;
                break;
            case MotionEvent.ACTION_MOVE:
                //得到偏移量
                int offsetX = x - lastX;
                int offsetY = y - lastX;
                //四个参数，left,top,right,bottom
                //使用 layout
//                layout(getLeft() + offsetX, getTop() + offsetY, getRight() + offsetX, getBottom() + offsetY);


                // 使用 offsetLeftAndRight（）与offsetTopAndBottom（）方式
                offsetLeftAndRight(offsetX);
                offsetTopAndBottom(offsetY);

                break;
            default:
                break;
        }
        return true;
    }

```
#### LayoutParams的方式
> LayoutParams主要保存了一个View的布局参数，因此我们可以通过LayoutParams来改变View的布局参数从而达到改变View位置的效果
- **注意：这里如果viewgroup是relayoutlayout,如果设置了android:layout_centerInParent="true"是不起作用的;**


```Java
  @Override
    public boolean onTouchEvent(MotionEvent event) {

        int x = (int) event.getX();
        int y = (int) event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                lastX = x;
                lastY = y;
                break;
            case MotionEvent.ACTION_MOVE:
                //得到偏移量
                int offsetX = x - lastX;
                int offsetY = y - lastX;
                //四个参数，left,top,right,bottom
                //使用 layout
//                layout(getLeft() + offsetX, getTop() + offsetY, getRight() + offsetX, getBottom() + offsetY);

                // 使用 offsetLeftAndRight（）与offsetTopAndBottom（）方式
//                offsetLeftAndRight(offsetX);
//                offsetTopAndBottom(offsetY);


                //使用 LayoutParams 的方式
                LinearLayout.LayoutParams layoutParams = (LinearLayout.LayoutParams) this.getLayoutParams();
                layoutParams.leftMargin = getLeft() + offsetX;
                layoutParams.topMargin = getTop() + offsetY;
                layoutParams.rightMargin = getRight() + offsetX;
                layoutParams.bottomMargin = getBottom() + offsetY;
                setLayoutParams(layoutParams);
                break;
            default:
                break;
        }
        return true;
    }

```
#### 动画方式
> 采用动画的方式来进行View的滑动，主要就涉及到两个动画：属性动画和补间动画
补间动画的实现


##### 补间动画

- 在res目录新建anim文件夹并创建translate_remove_view.xml


```Java 
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"  
android:fillAfter="true"
android:duration="2000">
    <translate
        android:fromXDelta="0"
        android:toXDelta="300"/>
</set>

在代码中：
//次动画的移动是 补间动画
Animation animation = AnimationUtils.loadAnimation(RemoveViewActivity.thisR.anim.translate_remove_view);
removeView_animator.startAnimation(animation);

```

- 或者直接使用代码


```Java
TranslateAnimation animator_code = new TranslateAnimation(0, 300f, 0, 0);
animator_code.setDuration( 2000);//设置动画持续时间
animator_code.setFillAfter(true);//设置是否到指定的位置后不在移动
view.setAnimation(animator_code);
```


> **需要注意的是，View动画并不能改变View的位置参数。如果对一个Button进行如上的平移动画操作，
当Button平移300像素停留在当前位置时，我们点击这个Button并不会触发点击事件，但在我们点击这个它起始位置的时候会有反应的。这个就是补间动画的和属性动画的区别**



##### 属性动画
> 属性动画是Android3.0之后推出的，就是为了解决补间动画的焦点问题，我们经常使用到的类主要有AnimatorSet和ObjectAnimator；使用 ObjectAnimator 进行更精细化的控制，控制一个对象和一个属性值，而使用多个ObjectAnimator组合到AnimatorSet形成一个动画。属性动画通过调用属性get、set方法来真实地控制一个View的属性值，因此，强大的属性动画框架基本可以实现所有的动画效果。

- ObjectAnimator

ObjectAnimator 是属性动画最重要的类，创建一个 ObjectAnimator 只需通过其静态工厂类直接返还一个
ObjectAnimator对象。参数包括一个对象和对象的属性名字，但这个属性必须有get和set方法，其内部会通
过Java反射机制来调用set方法修改对象的属性值。下面看看平移动画是如何实现的


```Java
ObjectAnimator animation = ObjectAnimator.ofFloat(removeView_animator,"translationX", 300f,100f,900f);
animation.setDuration(2000);
animation.start();
```
**这和补间动画的加了fillAfter属性后动画效果是一样的，但是不一样的地方就是焦点已经平移到了新的位置。解决了焦点的问题。
需要注意的是，在使用ObjectAnimator的时候，要操作的属性必须要有get和set方法，不然
ObjectAnimator 就无法生效。如果一个属性没有get、set方法，也可以通过自定义一个属性类或包装类来间
接地给这个属性增加get和set方法。**

例子：


```Java
public class AnimatorView extends View {
    
    //给属性指定set get 方法 。以后的动画要用到的
    public int widths;


    public AnimatorView(Context context) {
        super(context);
    }

    public AnimatorView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public AnimatorView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }


    public int getWidths() {
        return widths;
    }

    public void setWidths(int width) {

        this.getLayoutParams().width = width;
        //刷新 控件
        this.requestLayout();
    }

}


```

调用


```Java
  //使用属性动画 并自定义 get set进行动画
  // 这里我们设置2秒增加view的宽度300个像素
  // widths 此属性要和上面定义的一致才可以   
  ObjectAnimator objectAnimator = ObjectAnimator.ofInt(animatorView, "widths", 300);
  objectAnimator.setDuration(2000);
  objectAnimator.start();
```
![效果](https://upload-images.jianshu.io/upload_images/2953304-089ae296663d14c7.gif?imageMogr2/auto-orient/strip)

- ValueAnimator
> ValueAnimator不提供任何动画效果，它更像一个数值发生器，用来产生有一定规律的数字，从而让调
用者控制动画的实现过程。通常情况下，在ValueAnimator的AnimatorUpdateListener中监听数值的变化，从
而完成动画的变换，代码如下所示：


```Java 
//从 0f 到 100f  产生规律的数字
 ValueAnimator valueAnimator = ValueAnimator.ofFloat(0f, 100f);
        valueAnimator.setTarget(tv_animator_method);
        valueAnimator.setDuration(2000);
        valueAnimator.start();
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                Log.d("animation", animation.getAnimatedValue() + "");
            }
        });
        
log信息：
 0.0
 0.01578927
 0.15413165
 0.26845932
 0.42434633
 0.61558187
 0.82773864
 1.0871828
 1.3815045
 1.6901821
 ... 
```

以后就是自定义进度条的思路之一。


#### scrollTo与scollBy

> scrollTo（x，y）表示移动到一个具体的坐标点，而scrollBy（dx，dy）则表示移动的增量为dx、dy。其中，scollBy最终也是要调用scollTo的。

> scollTo、scollBy移动的是View的内容，如果在ViewGroup中使用，则是移动其所有的子View。View则是移动的里面的content。

代码
```Java
@Override
    public boolean onTouchEvent(MotionEvent event) {
        int x = (int) event.getX();
        int y = (int) event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                lastX = x;
                lastY = y;
                break;
            case MotionEvent.ACTION_MOVE:
                int offsetX = x - lastX;
                int offsetY = y - lastX;
                ViewGroup viewGroup = (ViewGroup) ScrollByView.this.getParent();

                viewGroup.scrollBy(-offsetX, -offsetY);
                break;
            default:
                break;
        }

        return true;
    }
```


**这样就实现了和layout等一样的拖动效果
注：至于这里为什么是负数，其实是参照物不一样导致的，scrollby和scrollto滚动的时候其实是类似于窗口里面的View并没有移动，而是手机屏幕在移动，如果我设置为正数，其实类似手机屏幕往右移动了，view保持不动，这样间接的其实类似于View左移了**


#### Scroller
> 我们在用scollTo/scollBy方法进行滑动时，这个过程是瞬间完成的，所以用户体验不大好。这里我们可
以使用 Scroller 来实现有过渡效果的滑动，这个过程不是瞬间完成的，而是在一定的时间间隔内完成的。
Scroller本身是不能实现View的滑动的，它需要与View的computeScroll（）方法配合才能实现弹性滑动的效
果。

- computeScroll()

系统会在绘制View的时候在draw（）方法中调用computeScroll（）方法。在这个方法中，我们调用父类的scrollTo（）方法并通过Scroller来不断获取当前的滚动值，每滑动一小段距离我们就调用invalidate（）方法不断地进行重绘，重绘就会调用computeScroll（）方法，这样我们通过不断地移动一个小的距离并连贯起来就实现了平滑移动的效果。

写个demo我们将view平滑滚动带屏幕坐标（400,500）的位置，具体代码如下

```Java
  private void initView() {
        scroller = new Scroller(getContext());
    }

    @Override
    public void computeScroll() {
        super.computeScroll();
        //判断Scroller是否执行完毕
        if (scroller.computeScrollOffset()) {
            ViewGroup viewGroup = (ViewGroup) this.getParent();
            int x =scroller.getCurrX();
            int y =scroller.getCurrY();
            viewGroup.scrollTo(scroller.getCurrX(), scroller.getCurrY());
            invalidate();
        }
    }

    public void smoothScrollTo(int destX, int destY) {

        View viewGroup =  ((View) getParent());

        int scrollX = this.getScrollX();
        int scrollY = this.getScrollY();
        //偏移量
        int deltaX = destX - scrollX;
        int deltaY = destY - scrollY;
        scroller.startScroll(scrollX, scrollY, deltaX, deltaY, 2000);
    }
```

调用
```Java
 removeView_animator.smoothScrollTo(-400, -500);
```
![效果](https://upload-images.jianshu.io/upload_images/2922217-9220623cfa02fe66.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

原文：https://www.jianshu.com/p/61ad263c4a0e  如有侵权可马上删除！





