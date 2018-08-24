---
title: scrollTo 以及  scrollBy
date: 2018-08-13 14:38:35
tags:
copyright: true
categories: android


---



<blockquote class="blockquote-center">不要给自己颓废的机会</blockquote>

<!-- more -->







想要知道scrollTo 和 scrollBy 需要先知道坐标系即：**视图坐标系**和**布局坐标系**
- 视图坐标系
> 我们必须明白在Android View视图是没有边界的，Canvas是没有边界的，只不过我们通过绘制特定的View时对Canvas对象进行了一定的操作。例如 :translate(平移)、clipRect(剪切)等，以便达到我们的对该Canvas对象绘制的要求。我们可以将这种无边界的视图称为“视图坐标”。它不受物理屏幕限制。
- 布局坐标系
> 通常我们所理解的一个Layout布局文件只是该视图的显示区域，超过了这个显示区域将不能显示到父视图的区域中 ，对应的，我们可以将这种有边界的视图称为“布局坐标”。
![TIM截图20180822162516.png](https://upload-images.jianshu.io/upload_images/2953304-78fe3fc40eb2dd33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以上中：

黑色框表示该子视图的**布局坐标。**

褐色框框表示该子视图的**视图坐标**--->该坐标是无限的，超过了父视图给子视图规定的区域后，不再显示该超出内容。

因为布局坐标是无限的。我们需要将布局是中内容准确的显示到布局坐标中心。这个时候就用到了 scrollTo和scrollBy。

其中有两个变量就是用来移动视图中内容坐标的。**mScrollX** ，**mScrollY**

```Java
/**
	 * The offset, in pixels, by which the content of this view is scrolled
	 * horizontally.
	 * {@hide}
	 */
	protected int mScrollX;   //该视图内容相当于视图起始坐标的偏移量   ， X轴 方向
	/**
	 * The offset, in pixels, by which the content of this view is scrolled
	 * vertically.
	 * {@hide}
	 */
	protected int mScrollY;   //该视图内容相当于视图起始坐标的偏移量   ， Y轴方向
 
	/**
     * Return the scrolled left position of this view. This is the left edge of
     * the displayed part of your view. You do not need to draw any pixels
     * farther left, since those are outside of the frame of your view on
     * screen.
     *
     * @return The left edge of the displayed part of your view, in pixels.
     */
    public final int getScrollX() {
        return mScrollX;
    }
 
    /**
     * Return the scrolled top position of this view. This is the top edge of
     * the displayed part of your view. You do not need to draw any pixels above
     * it, since those are outside of the frame of your view on screen.
     *
     * @return The top edge of the displayed part of your view, in pixels.
     */
    public final int getScrollY() {
        return mScrollY;
    }

```

方法：`public void scrollTo(int x, int y)；` 在当前视图内容偏移至(x , y)坐标处，即显示(可视)区域位于(x , y)坐标处。

```Java 
    /**
     * Set the scrolled position of your view. This will cause a call to
     * {@link #onScrollChanged(int, int, int, int)} and the view will be
     * invalidated.
     * @param x the x position to scroll to
     * @param y the y position to scroll to
     */
    public void scrollTo(int x, int y) {
    	//偏移位置发生了改变
        if (mScrollX != x || mScrollY != y) {
            int oldX = mScrollX;
            int oldY = mScrollY;
            mScrollX = x;  //赋新值，保存当前便宜量
            mScrollY = y;
            //回调onScrollChanged方法
            onScrollChanged(mScrollX, mScrollY, oldX, oldY);
            if (!awakenScrollBars()) {
                invalidate();  //一般都引起重绘
            }
        }
    }

```
方法 `public void scrollBy(int x, int y)` 在当前视图内容继续偏移(x , y)个单位，显示(可视)区域也跟着偏移(x,y)个单位。

```Java
  /**
     * Move the scrolled position of your view. This will cause a call to
     * {@link #onScrollChanged(int, int, int, int)} and the view will be
     * invalidated.
     * @param x the amount of pixels to scroll by horizontally
     * @param y the amount of pixels to scroll by vertically
     */
    // 看出原因了吧 。。 mScrollX 与 mScrollY 代表我们当前偏移的位置 ， 在当前位置继续偏移(x ,y)个单位
    //其实就是在以前偏移的基础上继续偏移多少个单位 
    public void scrollBy(int x, int y) {
        scrollTo(mScrollX + x, mScrollY + y);
    }

```

看例子:

![栗子](http://hi.csdn.net/attachment/201202/9/0_1328802540JzD6.gif)

> 自定义一个ViewGroup 其中包含三个LinearLayout。每个LinearLayout都占一屏。

```Java
// measure过程
	@Override
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {

		Log.i(TAG, "--- start onMeasure --");

		// 设置该ViewGroup的大小
		int width = MeasureSpec.getSize(widthMeasureSpec);
		int height = MeasureSpec.getSize(heightMeasureSpec);
		setMeasuredDimension(width, height);

		int childCount = getChildCount();
		Log.i(TAG, "--- onMeasure childCount is -->" + childCount);
		for (int i = 0; i < childCount; i++) {
			View child = getChildAt(i);
			// 设置每个子视图的大小 ， 即全屏
			child.measure(MultiScreenActivity.screenWidth, MultiScreenActivity.scrrenHeight);
		}
	}

```

> 设置三个LinearLayout的位置。从左到右排列。

```Java
// layout过程
	@Override
	protected void onLayout(boolean changed, int l, int t, int r, int b) {
		// TODO Auto-generated method stub
		Log.i(TAG, "--- start onLayout --");
		int startLeft = 0; // 每个子视图的起始布局坐标
		int startTop = 10; // 间距设置为10px 相当于 android：marginTop= "10px"
		int childCount = getChildCount();
		Log.i(TAG, "--- onLayout childCount is -->" + childCount);
		for (int i = 0; i < childCount; i++) {
			View child = getChildAt(i);
			child.layout(startLeft, startTop,
					startLeft + MultiScreenActivity.screenWidth,
					startTop + MultiScreenActivity.scrrenHeight);
			startLeft = startLeft + MultiScreenActivity.screenWidth ; //校准每个子View的起始布局位置
			//三个子视图的在屏幕中的分布如下 [0 , 320] / [320,640] / [640,960]
		}
	}
```

> 使用scrollBy scrollBo 将视图能移动到布局坐标中去。从而可以显示出来


```Java
@Override
    public void onClick(View v) {
        // TODO Auto-generated method stub

        switch (v.getId()) {
            case R.id.bt_scrollLeft:
                if (curscreen > 0) {  //防止屏幕越界
                    curscreen--;
                    Toast.makeText(MultiScreenActivity.this, "第" + (curscreen + 1) + "屏", Toast.LENGTH_SHORT).show();
                } else
                    Toast.makeText(MultiScreenActivity.this, "当前已是第一屏", Toast.LENGTH_SHORT).show();
                mulTiViewGroup.scrollTo(curscreen * screenWidth, 0);
                break;
            case R.id.bt_scrollRight:
                if (curscreen < 2) { //防止屏幕越界
                    curscreen++;
                    Toast.makeText(MultiScreenActivity.this, "第" + (curscreen + 1) + "屏", Toast.LENGTH_SHORT).show();
                } else
                    Toast.makeText(MultiScreenActivity.this, "当前已是最后一屏", Toast.LENGTH_SHORT).show();
                mulTiViewGroup.scrollTo(curscreen * screenWidth, 0);
                break;
        }
    }
```

**其中偏移量都是反着来的。相当于View是个恨到的白纸。中间有个洞，使我们的布局坐标。当我们向左移动纸的时候，视图相对于纸是向右移动的。**




































