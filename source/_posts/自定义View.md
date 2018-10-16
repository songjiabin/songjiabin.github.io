---
title: 自定义View
date: 2017-06-02
tags: 
- android 
- 自定义view
copyright: true
categories: android
---


<blockquote class="blockquote-center">黑发不知勤学早，白首方悔读书迟</blockquote>

<!-- more -->

- - - 

### View 

#### 概叙
> 自定义View我们大部分时候只需重写两个函数：onMeasure()、onDraw()。onMeasure负责对当前View的尺寸进行测量，onDraw负责把当前这个View绘制出来。



```Java
 public MyView(Context context) {
        super(context);
    }

    public MyView(Context context, AttributeSet attrs) {
        super(context, attrs); 
    }
```

#### onMeasure 测量

> 当我们定义布局的时候。在xml中，会定义 wrap_content、match_parent。意思就是将尺寸设置成包裹的内容和填充瞒整父布局给子View的空间大小。这里并没有给出具体的大小。但是当真正的落实到屏幕上的时候，是必须给出具体的尺寸的。所以就需要我们出设置了，也就是测量宽高了，并设置宽高了。

 
onMeasure 方法中的测量模式的几种情况。

 


测量模式 | 意思| 用处
---|--- | ---
UNSPECIFIED | 父容器没有对当前View有任何限制，当前View可以任意取尺寸| 此模式用的不是很多
EXACTLY | 当前的尺寸就是当前View应该取的尺寸(就是这个尺寸，不用管了)|具体的尺寸、或者是给的match_parent
AT_MOST | 当前尺寸是当前View能取的最大尺寸(父容器给出的允许最大尺寸) | wrap_conent 

> **match_parent—>EXACTLY。怎么理解呢？match_parent就是要利用父View给我们提供的所有剩余空间，而父View剩余空间是确定的，也就是这个测量模式的整数里面存放的尺寸。**

> **wrap_content—>AT_MOST。怎么理解：就是我们想要将大小设置为包裹我们的view内容，那么尺寸大小就是父View给我们作为参考的尺寸，只要不超过这个尺寸就可以啦，具体尺寸就根据我们的需求去设定。**

> **固定尺寸（如100dp）—>EXACTLY。用户自己指定了尺寸大小，我们就不用再去干涉了，当然是以指定的大小为主啦**


自定义View以正方形的形式显示，即要宽高相等，并且默认的宽高值为100像素。
```Java
 private int getMySize(int defaultSize, int measureSpec) {
        int mySize = defaultSize;

        int mode = MeasureSpec.getMode(measureSpec);
        int size = MeasureSpec.getSize(measureSpec);

        switch (mode) {
            case MeasureSpec.UNSPECIFIED: {//如果没有指定大小，就设置为默认大小
                mySize = defaultSize;
                break;
            }
            case MeasureSpec.AT_MOST: {//如果测量模式是最大取值为size
                //我们将大小取最大值,你也可以取其他值
                //父类测量的最大尺寸是size。view最大的尺寸不能比这个大。
                mySize = size;
                break;
            }
            case MeasureSpec.EXACTLY: {//如果是固定的大小，那就不要去改变它
                mySize = size;
                break;
            }
        }
        return mySize;
}

@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int width = getMySize(100, widthMeasureSpec);
        int height = getMySize(100, heightMeasureSpec);

        if (width < height) {
            height = width;
        } else {
            width = height;
        }

        setMeasuredDimension(width, height);
}

 
```
ok 不管你的xml中如何定义 最终显示的都是正方形


#### onDraw  绘制
> 将我们想要的图形再次进行绘制


```Java   
@Override
    protected void onDraw(Canvas canvas) {
        //调用父View的onDraw函数，因为View这个类帮我们实现了一些
        // 基本的而绘制功能，比如绘制背景颜色、背景图片等
        super.onDraw(canvas);
        int r = getMeasuredWidth() / 2;//也可以是getMeasuredHeight()/2,本例中我们已经将宽高设置相等了
        //圆心的横坐标为当前的View的左边起始位置+半径
        int centerX = getLeft() + r;
        //圆心的纵坐标为当前的View的顶部起始位置+半径
        int centerY = getTop() + r;

        Paint paint = new Paint();
        paint.setColor(Color.GREEN);
        //开始绘制
        canvas.drawCircle(centerX, centerY, r, paint);
    }
 
```

在实际情况中遇到一个问题。就是当在xml布局中当你的父布局是 LinearLayout 的时候。此Demo是没问题的。但是当父布局View是 RelativeLayout 的时候。就不能达到，不管怎么写，都是正方形的要求了。

原来：**因为Relativelayout中，onLayout函数是直接通过RelativeLayout.LayoutParams来决定的，换句话说，不管你的setMeasuredDimension函数设置多大的尺寸，RelativeLayout还是以xml中的布局文件为主。当然了，setMeasuredDimension设置的值跟getMeasuredWidth()、getMeasuredHeight()是一致的。**



###  ViewGroup
#### 概述
> ViewGroup 就是装着很多ViewGroup 或者 view 的容器。所以自定义ViewGroup的话。要根据子view测量出自身的宽高。并将子View的放置到合适的位置。最后在摆放自己的位置。


#### 自己设计一个ViewGroup
- **首先，我们得知道各个子View的大小吧，只有先知道子View的大小，我们才知道当前的ViewGroup该设置为多大去容纳它们。**
- **根据子View的大小，以及我们的ViewGroup要实现的功能，决定出ViewGroup的大小**
- **ViewGroup和子View的大小算出来了之后，接下来就是去摆放了吧，具体怎么去摆放呢？这得根据你定制的需求去摆放了，比如，你想让子View按照垂直顺序一个挨着一个放，或者是按照先后顺序一个叠一个去放，这是你自己决定的。**

- **已经知道怎么去摆放还不行啊，决定了怎么摆放就是相当于把已有的空间”分割”成大大小小的空间，每个空间对应一个子View，我们接下来就是把子View对号入座了，把它们放进它们该放的地方去。**

**总结来说的话：测量子View的大小。从而决定此ViewGroup的大小。将每个子view对号入座，实现想要的效果**
 
 
 
#### 例子

> 将子View按从上到下垂直顺序一个挨着一个摆放，即模仿实现LinearLayout的垂直布局。
 

##### onMeasure()测量
 
根据子View来测量出ViewGroup的大小。
```Java
@Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        //将所有的子View进行测量，这会触发每个子View的onMeasure函数

        //注意要与measureChild区分，measureChild是对单个view进行测量
        measureChildren(widthMeasureSpec, heightMeasureSpec);


        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);


        int childCount = getChildCount();

        if (childCount == 0) {
            //说明没有子View 。那么当前ViewGroup没有存在的意思。不用占用
            setMeasuredDimension(0, 0);

        } else {

            if (widthMode == MeasureSpec.AT_MOST && heightMode == MeasureSpec.AT_MOST) {
                //如果 ViewGroup 宽高 都是 wrap_content

                //我们将高度设置为所有子View的高度相加，宽度设为子View中最大的宽度
                setMeasuredDimension(Math.min(getMaxWidth(), widthSize), Math.min(getAllViewsHeight(), heightSize));

            } else if (widthMode == MeasureSpec.AT_MOST) {
                //即：宽度是 wrap_content 高度是 固定的值


                // 设置  宽度的最大值 不能超过  withSize    高度是固定的  heightSize

                setMeasuredDimension(Math.min(getMaxWidth(), widthSize), heightSize);

            } else if (heightMode == MeasureSpec.AT_MOST) {
                //即：高度是 wrap_content 宽度是 固定的值


                // 设置  高度是 叠加的  宽度是 固定的

                setMeasuredDimension(widthSize, Math.min(getAllViewsHeight(), heightSize));
            }
        }
    }
    
    
    
    
     /**
     * 得到  子View 中 最大的宽度
     *
     * @return
     */
    private int getMaxWidth() {
        int maxWidth = 0;
        for (int i = 0; i < getChildCount(); i++) {
            View view = getChildAt(i);
            if (view.getMeasuredWidth() > maxWidth) {
                maxWidth = view.getMeasuredWidth();
            }
        }
        return maxWidth;
    }


    /**
     * 得到所有子View的高度
     *
     * @return
     */
    private int getAllViewsHeight() {
        int totalHeight = 0;
        for (int i = 0; i < getChildCount(); i++) {
            View view = getChildAt(i);
            totalHeight += view.getMeasuredHeight();
        }
        return totalHeight;
    }
```

##### onLayout() 布局 
根据子View布局 

```Java 
 @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        int count = getChildCount();
        //记录当前的高度位置

        // 注意 curHeight 应为0开始。不能等于 t 。t是此 ViewGroup 相对于它父容器的距离。
        // 当 此ViewGroup 并不是父容器的第一个子View的时候。就会出现问题。所以 curHeight 应为 0 开始
        int curHeight = 0;


        //将子View逐个摆放
        for (int i = 0; i < count; i++) {
            View view = getChildAt(i);
            int height = view.getMeasuredHeight();
            int width = view.getMeasuredWidth();

            view.layout(l, curHeight, l + width, curHeight + height);
            curHeight += height;

        }

    }
```


#### 实现自己的Demo 

![TIM截图20180927182057.png](https://upload-images.jianshu.io/upload_images/2953304-d0ea13176ab5005c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实现上图中一行有两个View 并顺序拜访。中间没有间隔。
```Java 
/**
 * author : 宋佳
 * time   :  
 * desc   :  自定义ViewGroup 需求：我们定义一个ViewGroup，内部可以传入0到4个childView。第一、第二行分别两个 。       
 * version: 1.0.0
 */

public class OtherViewGroup extends ViewGroup {


    public OtherViewGroup(Context context) {
        super(context);
    }


    public OtherViewGroup(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public OtherViewGroup(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }


    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        //应该现在是明确的因为现在有四个  View

        //测量每个子View
        measureChildren(widthMeasureSpec, heightMeasureSpec);

        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int sizeWidth = MeasureSpec.getSize(widthMeasureSpec);
        int sizeHeight = MeasureSpec.getSize(heightMeasureSpec);


        //根据子View 来确定ViewGroup的大小

        //确定宽度


        int childCount = getChildCount();
        if (childCount == 0) {
            //说明没有子View 。那么当前ViewGroup没有存在的意思。不用占用
            setMeasuredDimension(0, 0);
        } else {
            if (widthMode == MeasureSpec.AT_MOST && heightMode == MeasureSpec.AT_MOST) {
                // 宽高给出的都是允许范围内的最大值
                int maxWithOfFirstAndTwo = getMaxWithOfFirstAndTwo();


                int maxWitdth = Math.min(sizeWidth, maxWithOfFirstAndTwo);


                int maxHeightOfFirstAndTwo = getMaxHeightOfFristAndTwo();

                int maxHeight = Math.min(sizeHeight, maxHeightOfFirstAndTwo);


                setMeasuredDimension(maxWitdth, maxHeight);
            } else if (widthMode == MeasureSpec.AT_MOST) {
                //宽度是  允许最大的值  高度是固定的值
                int maxWithOfFirstAndTwo = getMaxWithOfFirstAndTwo();


                int maxWitdth = Math.min(sizeWidth, maxWithOfFirstAndTwo);
                setMeasuredDimension(maxWitdth, sizeHeight);

            } else if (heightMode == MeasureSpec.AT_MOST) {
                int maxHeightOfFirstAndTwo = getMaxHeightOfFristAndTwo();

                int maxHeight = Math.min(sizeHeight, maxHeightOfFirstAndTwo);
                setMeasuredDimension(sizeWidth, maxHeight);
            }


        }


    }

    /**
     * 得到 第一行和第二行中最大的宽度
     *
     * @return
     */
    private int getMaxWithOfFirstAndTwo() {

        View viewLeftTop = getChildAt(0); //左上角
        View viewRightTop = getChildAt(1);//右上角
        View viewLeftBottom = getChildAt(2);//左下角
        View viewRightBottom = getChildAt(3);//右下角

        int firstLineWidth = viewLeftTop.getMeasuredWidth() + viewRightTop.getMeasuredWidth();
        int twoLineWidth = viewLeftBottom.getMeasuredWidth() + viewRightBottom.getMeasuredWidth();
        return Math.max(firstLineWidth, twoLineWidth);
    }

    /**
     * 得到 第一列 和第二列中最高的高度
     *
     * @return
     */
    private int getMaxHeightOfFristAndTwo() {


        View viewLeftTop = getChildAt(0); //左上角
        View viewRightTop = getChildAt(1);//右上角
        View viewLeftBottom = getChildAt(2);//左下角
        View viewRightBottom = getChildAt(3);//右下角

        //设置高度
        int firstColumnHeight = viewLeftTop.getMeasuredHeight() + viewLeftBottom.getMeasuredHeight();
        int twoColumnHeight = viewRightTop.getMeasuredHeight() + viewRightBottom.getMeasuredHeight();

        return Math.max(firstColumnHeight, twoColumnHeight);
    }


    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        int childCount = getChildCount();
        //记录当前的最大值
        int currentWidth = 0;
        int currentHeight = 0;
        for (int i = 0; i < childCount; i++) {
            View view = getChildAt(i);
            if (i == 0) {
                //第一个 View
                view.layout(0, 0, view.getMeasuredWidth(), view.getMeasuredHeight());
                currentWidth = view.getMeasuredWidth();
                currentHeight = view.getMeasuredHeight();
            } else if (i == 1) {
                //第二个View
                view.layout(currentWidth, 0, view.getMeasuredWidth() + currentWidth, view.getMeasuredHeight());
                currentWidth = 0;
                currentHeight = view.getMeasuredHeight() > currentHeight ? view.getMeasuredHeight() : currentHeight;
            } else if (i == 2) {
                view.layout(0, currentHeight, view.getMeasuredWidth(), currentHeight + view.getMeasuredHeight());
                currentWidth = view.getMeasuredWidth();
            } else if (i == 3) {
                view.layout(currentWidth, currentHeight, view.getMeasuredWidth() + currentWidth, currentHeight + view.getMeasuredHeight());
            }
        }
    }
}
```
[github](https://github.com/songjiabin/androidDemo/blob/master/app/src/main/java/com/example/mydemo/activity/MyViewGroupActivity.java)




#### 实现一个四个View在四个角上 支持边距  

![TIM截图20180928112512.png](https://upload-images.jianshu.io/upload_images/2953304-3a3550116fb1a0cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 思路大体一致，不过此ViewGroup要支持 margin 

首先ViewGroup要重新此方法

```Java 
   /**
     * 我们只需要ViewGroup能够支持margin即可，那么我们直接使用系统的MarginLayoutParams
     */
    @Override
    public ViewGroup.LayoutParams generateLayoutParams(AttributeSet attrs) {
        return new MarginLayoutParams(getContext(), attrs);
    }
```
下面是具体代码 
```Java 
/**
 * author : 宋佳
 * time   :  
 * desc   :  自定义ViewGroup 需求：我们定义一个ViewGroup，内部可以传入0到4个childView，分别依次显示在左上角，右上角，左下角，右下角。
 * version: 1.0.0
 */


public class OtherViewGroup_fourCorners extends ViewGroup {


    public OtherViewGroup_fourCorners(Context context) {
        super(context);
    }

    public OtherViewGroup_fourCorners(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public OtherViewGroup_fourCorners(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }


    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        //应该现在是明确的因为现在有四个  View

        //测量每个子View
        measureChildren(widthMeasureSpec, heightMeasureSpec);

        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);

        int sizeWidth = MeasureSpec.getSize(widthMeasureSpec);
        int sizeHeight = MeasureSpec.getSize(heightMeasureSpec);


        int childCount = getChildCount();
        if (childCount == 0) {
            //说明没有子View 。那么当前ViewGroup没有存在的意思。不用占用
            setMeasuredDimension(0, 0);
        } else {
            MarginLayoutParams cParams = null;
            int topWidth = 0;//上面第一行宽度
            int bottomWidth = 0;//下面一行的宽度

            int leftHeight = 0; //左边一列的高度
            int rightHeight = 0;//右边一列的高度

            int measureWidth = 0;
            int measureHeight = 0;

            for (int i = 0; i < getChildCount(); i++) {
                View view = getChildAt(i);
                cParams = (MarginLayoutParams) view.getLayoutParams();
                measureWidth = view.getMeasuredWidth();
                measureHeight = view.getMeasuredHeight();
                if (i == 0 || i == 1) {
                    topWidth += measureWidth + cParams.leftMargin + cParams.rightMargin;
                }
                if (i == 2 || i == 3) {
                    bottomWidth += measureWidth + cParams.leftMargin + cParams.rightMargin;
                }
                if (i == 0 || i == 2) {
                    leftHeight += measureHeight + cParams.topMargin + cParams.bottomMargin;
                }
                if (i == 1 || i == 3) {
                    rightHeight += measureHeight + cParams.topMargin + cParams.bottomMargin;
                }


            }


            int maxWidth = Math.min(Math.max(topWidth, bottomWidth), sizeWidth);
            int maxHeigth = Math.min(Math.max(leftHeight, rightHeight), sizeHeight);
            if (widthMode == MeasureSpec.AT_MOST && heightMode == MeasureSpec.AT_MOST) {
                // 最大的值是限定的
                setMeasuredDimension(maxWidth, maxHeigth);
            } else if (widthMode == MeasureSpec.AT_MOST) {
                setMeasuredDimension(maxWidth, sizeHeight);
            } else if (heightMode == MeasureSpec.AT_MOST) {
                setMeasuredDimension(sizeWidth, maxHeigth);
            }


        }


    }


    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        int viewGroupWidth = this.getMeasuredWidth();
        int viewGroupHeight = this.getMeasuredHeight();
        for (int i = 0; i < getChildCount(); i++) {
            View view = getChildAt(i);
            MarginLayoutParams cParams = (MarginLayoutParams) view.getLayoutParams();
            switch (i) {

                case 0:
                    view.layout(cParams.leftMargin, cParams.topMargin, view.getMeasuredWidth() + cParams.leftMargin, view.getMeasuredHeight() + cParams.topMargin);
                    break;
                case 1:
                    //右上角
                    view.layout(viewGroupWidth - cParams.rightMargin - view.getMeasuredWidth(), cParams.topMargin, viewGroupWidth - cParams.rightMargin, cParams.topMargin + view.getMeasuredHeight());
                    break;
                case 2:
                    //左下角
                    view.layout(cParams.leftMargin, viewGroupHeight - cParams.bottomMargin - view.getMeasuredHeight(), cParams.leftMargin + view.getMeasuredWidth(), viewGroupHeight - cParams.bottomMargin);
                    break;
                case 3:
                    //右下角
                    view.layout(viewGroupWidth - cParams.rightMargin - view.getMeasuredWidth(), viewGroupHeight - cParams.bottomMargin - view.getMeasuredHeight(), viewGroupWidth - cParams.rightMargin, viewGroupHeight - cParams.bottomMargin);
                    break;
                default:
                    break;
            }
        }


       /*//将 子View  layout
        int count = getChildCount();

        int cWidth = 0;
        int cHeight = 0;
        MarginLayoutParams cParams = null;

        for (int i = 0; i < count; i++) {
            View childView = getChildAt(i);
            cWidth = childView.getMeasuredWidth();
            cHeight = childView.getMeasuredHeight();
            cParams = (MarginLayoutParams) childView.getLayoutParams();

            int cleft = 0, ctop = 0, cright = 0, cbottom = 0;
            switch (i) {
                case 0:
                    cleft = cParams.leftMargin;
                    ctop = cParams.topMargin;
                    break;
                case 1:
                    cleft = getWidth() - cWidth - cParams.rightMargin;
                    ctop = cParams.topMargin;
                    break;
                case 2:
                    cleft = cParams.leftMargin;
                    ctop = getHeight() - cHeight - cParams.bottomMargin;
                    break;
                case 3:
                    cleft = getWidth() - cWidth - cParams.rightMargin;
                    ctop = getHeight() - cHeight - cParams.bottomMargin;
                    break;
                default:
                    break;


            }

            cright = cWidth + cleft;
            cbottom = cHeight + ctop;
            childView.layout(cleft, ctop, cright, cbottom);
        }
*/

    }


    /**
     * 我们只需要ViewGroup能够支持margin即可，那么我们直接使用系统的MarginLayoutParams
     */
    @Override
    public ViewGroup.LayoutParams generateLayoutParams(AttributeSet attrs) {
        return new MarginLayoutParams(getContext(), attrs);
    }
}

```

[github](https://github.com/songjiabin/androidDemo/blob/master/app/src/main/java/com/example/mydemo/viewgroup/OtherViewGroup_fourCorners.java)



[参考地址](https://blog.csdn.net/huachao1001/article/details/51577291)

