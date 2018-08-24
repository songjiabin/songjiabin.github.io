---
title: popularWindow封装、单选、多选
date: 2018-08-13 14:38:35
copyright: true
categories: android
---



<blockquote class="blockquote-center">雄起</blockquote>


<!-- more -->




> PopupWindow这个类用来实现一个弹出框，可以使用任意布局的View作为其内容，这个弹出框是悬浮在当前activity之上的。

#### 平常常见的创建方法是

```Java
    if (popupView != null && popupWindow.isShowing()) {
            popupWindow.dismiss();
            return;
        }


        //准备PopupWindow的布局View
        popupView = LayoutInflater.from(this).inflate(R.layout.popular_window, null);
        //初始化一个PopupWindow，width和height都是WRAP_CONTENT


        popupView.measure(0, 0);

        popupWindow = new PopupWindow(popupView, popupView.getMeasuredWidth(), popupView.getMeasuredHeight(), true);


//  不要这些创建 否则不能得到 popularWindow 的高度和宽度
//        popupWindow = new PopupWindow(
//                ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);


        //设置PopupWindow的视图内容
        popupWindow.setContentView(popupView);
        //点击空白区域PopupWindow消失，这里必须先设置setBackgroundDrawable，否则点击无反应
        popupWindow.setBackgroundDrawable(new ColorDrawable(0x00000000));
        popupWindow.setOutsideTouchable(true);
        //设置PopupWindow动画
        popupWindow.setAnimationStyle(R.style.Theme_AppCompat_DialogWhenLarge);
        //设置是否允许PopupWindow的范围超过屏幕范围
        popupWindow.setClippingEnabled(true);
        //注意必须设置焦点   不然 isShowing不执行
        popupWindow.setFocusable(true);
        //设置PopupWindow消失监听
        popupWindow.setOnDismissListener(new PopupWindow.OnDismissListener() {
            @Override
            public void onDismiss() {

            }
        });
```

#### 展示popuwindow的定位展示
效果如图
![效果图](http://t1.aixinxi.net/o_1cktom2pb193t1r02gaa1o8l1d2ia.gif-w.jpg)

定位其实用到了 
> showAsDropDown(View anchor) : 显示在参照物的周围

>  showAsDropDown(View anchor, int xoff, int yoff)   xoff、yoff分别是X轴、Y轴的偏移量

> public void showAtLocation(View parent, int gravity, int x, int y)
 这个两个方法

代码有具体介绍：
```Java
 //向下弹框
    private void bottom(View view) {
        //showAsDropDown(View anchor) : 显示在参照物的周围
        //showAsDropDown(View anchor, int xoff, int yoff)   xoff、yoff分别是X轴、Y轴的偏移量
        //如果不设置  xoff、yoff  默认显示到 参照物的下面
        //不设置 xoff ,yoff 默认到 参照物下面

        //public void showAtLocation(View parent, int gravity, int x, int y)
        //设置在父控件的位置  example: Gravity.BOTTOM 表示在父控件的底部弹出   x ， y  分别是 x y 轴的偏移量

        popupWindow.showAsDropDown(view);
    }
    

 //向左弹框
    private void left(View view) {
        //public void showAtLocation(View parent, int gravity, int x, int y)
        //设置在父控件的位置  example: Gravity.BOTTOM 表示在父控件的底部弹出   x ， y  分别是 x y 轴的偏移量


        //得到点击的控件的坐标
        int[] positions = new int[2];
        view.getLocationOnScreen(positions);
        //其实 positions[0] 表示的横坐标  position[1]表示的事纵坐标

        int b = popupWindow.getWidth();


        //整个的content的位置  findViewById(android.R.id.content)
        popupWindow.showAtLocation(findViewById(android.R.id.content), Gravity.NO_GRAVITY, positions[0] - popupWindow.getWidth(), positions[1]);
    }
    
    
  //向上弹框
    private void top(View view) {
        //得到点击的控件的坐标
        int[] positions = new int[2];
        view.getLocationOnScreen(positions);
        //其实 positions[0] 表示的横坐标  position[1]表示的事纵坐标
        popupWindow.showAtLocation(view, Gravity.NO_GRAVITY, positions[0], positions[1] - popupWindow.getHeight());
    }
    
    //向右弹框
    private void right(View view) {
        //得到点击的控件的坐标
        int[] positions = new int[2];
        view.getLocationOnScreen(positions);

        //得到屏幕高度
        int screenHeight = ScreenUtils.getScreenHeight(this);

        int y =screenHeight - popupWindow.getHeight();

        //其实 positions[0] 表示的横坐标  position[1]表示的事纵坐标
        popupWindow.showAtLocation(view, Gravity.NO_GRAVITY, positions[0] + btn_nomar_popular_right.getMeasuredWidth(), y);
    }
```
 
#### 封装popularWindow 
使用建造中模式
```Java
public class CommonPopularWindow extends PopupWindow {


    private PopularController popularController;

    public CommonPopularWindow(Context context) {
        super(context);
        this.popularController = new PopularController(context, this);
    }


    /**
     * 当popular 关闭的时候
     */
    @Override
    public void dismiss() {
        super.dismiss();
        //将背景恢复
        this.popularController.setBackGroundLevel(1.0f);
    }


    @Override
    public int getWidth() {
        return popularController.contentView.getMeasuredWidth();
    }

    @Override
    public int getHeight() {
        return popularController.contentView.getMeasuredHeight();
    }


    public static class Builder {


        private PopularController.PopularParams params;
        private ViewInterface listener;



        public Builder(Context context) {
            params = new PopularController.PopularParams(context);
        }


        public Builder setView(int layoutResId) {
            params.mView = null;
            params.layoutResId = layoutResId;
            return this;
        }


        public Builder setWidthAndHeigth(int width, int height) {
            params.mWidth = width;
            params.mHeight = height;
            return this;
        }


        public Builder setBackGroundLevel(float level) {
            params.bg_level = level;
            return this;
        }

        public Builder setOutsideTouchable(boolean toubalbe) {
            params.isTouchable = toubalbe;
            return this;
        }

        public Builder setAnimationStyle(int animationStyle) {
            params.animationStyle = animationStyle;
            return this;
        }

        public Builder setViewOnclickListener(ViewInterface listener) {
            this.listener = listener;
            return this;
        }

        public CommonPopularWindow create() {
            CommonPopularWindow myCommonPopularWindow = new CommonPopularWindow(params.context);
            params.apply(myCommonPopularWindow.popularController);
            if (listener != null && params.layoutResId != 0) {
                listener.getChildView(myCommonPopularWindow.popularController.contentView, params.layoutResId);
            }
            CommonUtil.measureWidthAndHeight(myCommonPopularWindow.popularController.contentView);
            return myCommonPopularWindow;
        }

    }


    public interface ViewInterface {
        void getChildView(View view, int layoutResId);
    }


}
```

```Java
public class PopularController {


    private CommonPopularWindow popuWindow;
    private Context context;
    public View contentView;
    private Window mWindow;

    public PopularController(Context context, CommonPopularWindow commonPopularWindow) {
        this.context = context;
        this.popuWindow = commonPopularWindow;
    }


    /**
     * 创建 View
     */
    public void createView(int view) {
        this.contentView = LayoutInflater.from(context).inflate(view, null);
        this.popuWindow.setContentView(this.contentView);
    }

    /**
     * 创建 View
     */
    public void createView(View view) {
        this.contentView = null;
        this.contentView = view;
        this.popuWindow.setContentView(this.contentView);
    }


    /**
     * 设置popular的宽和高
     *
     * @param width
     * @param height
     */
    public void setPopuWindowWihthAndHeight(int width, int height) {
        if (width == 0 || height == 0) {
            this.popuWindow.setHeight(ViewGroup.LayoutParams.WRAP_CONTENT);
            this.popuWindow.setWidth(ViewGroup.LayoutParams.WRAP_CONTENT);
        } else {
            this.popuWindow.setWidth(width);
            this.popuWindow.setHeight(height);
        }
    }


    /**
     * 设置 popular的透明度
     * 当弹框的时候 改变背景颜色
     * 改变的是 window 透明度
     *
     * @param level
     */
    public void setBackGroundLevel(float level) {
        mWindow = ((Activity) context).getWindow();
        WindowManager.LayoutParams params = mWindow.getAttributes();
        params.alpha = level;
        mWindow.setAttributes(params);
    }


    /**
     * 设置popular动画
     *
     * @param animationStyle
     */
    public void setAnimationStyle(int animationStyle) {
        this.popuWindow.setAnimationStyle(animationStyle);
    }


    /**
     * 设置popular外部是否可以点击
     *
     * @param touchable
     */
    public void setOutSideTouchable(boolean touchable) {
        //设置是否可点击的时候必须先设置透明度
        this.popuWindow.setBackgroundDrawable(new ColorDrawable(0x00000000));//设置透明度
        this.popuWindow.setOutsideTouchable(touchable);
        this.popuWindow.setFocusable(touchable);
    }


    public static class PopularParams {

        public int layoutResId;//布局id
        public int mWidth, mHeight;//弹窗的宽和高
        public float bg_level;//屏幕背景灰色程度
        public int animationStyle;//动画Id
        public View mView;//布局View
        public boolean isTouchable = true;//是否可以点击


        public Context context;

        public PopularParams(Context context) {
            this.context = context;
        }

        public void apply(PopularController popularController) {

            if (mView != null) {
                popularController.createView(mView);
            } else if (layoutResId != 0) {
                popularController.createView(layoutResId);
            }

            popularController.setPopuWindowWihthAndHeight(mWidth, mWidth);

            popularController.setOutSideTouchable(isTouchable);

            popularController.setBackGroundLevel(bg_level);
            popularController.setAnimationStyle(animationStyle);

        }

    }
}
```
#### 使用封装的popuwindow
```Java
findViewById(R.id.btn_popular_all).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                CommonPopularWindow popu = new CommonPopularWindow.Builder(PopularWindowActivity.this)
                        .setView(R.layout.popular_window)
                        .setWidthAndHeigth(0, 0)
                        .setBackGroundLevel(0.5f).setViewOnclickListener(new CommonPopularWindow.ViewInterface() {
                            @Override
                            public void getChildView(View view, int layoutResId) {
                                //点击的具体idView
                                view.findViewById(R.id.tv_popuWindow1).setOnClickListener(new View.OnClickListener() {
                                    @Override
                                    public void onClick(View v) {
                                        Log.i("数据弹框","弹框了");
                                    }
                                });
                            }
                        }).create();
                popu.showAsDropDown(v);
            }
        });
```

#### 接下来封装popuwindow的单选、多选
效果图：
![单选](http://t1.aixinxi.net/o_1cl0ld597r8u1kaf1e3v1l571264a.gif-w.jpg)
![多选](https://upload-images.jianshu.io/upload_images/2953304-7562da839033fc01.gif?imageMogr2/auto-orient/strip)

- 定义类。
1、里面进行初始化PopularWindow及数据的初始化。
2、在popular中设置listview用于展示列表
3、自定义listview中item的布局。使用自定义的chebox和父布局
4、根据选择的模式（单选、多选)进行不同处理。

代码：
```Java
public class CustomerPopular implements View.OnClickListener, AdapterView.OnItemClickListener {


    private TextView tv_popular_cancel;
    private TextView tv_popular_ok;
    private ListView lv_popular_data;
    private LinearLayout ll_popular_buttons;
    private TextView tv_popular_title;
    private ImageView iv_popular_down;
    private final CommonPopularWindow.Builder budilder;
    private Activity context;
    private View view;
    private static int CHOICE_STATE = 0;
    private PopularAdapter adapter;
    private List<PopularWindowBean> datas;
    private CommonPopularWindow popupWindow;

    public CustomerPopular(Activity context) {
        this.context = context;
        budilder = new CommonPopularWindow.Builder(context);
        initView();
        initListener();
    }


    private void initView() {
        view = LayoutInflater.from(context).inflate(R.layout.popular_products, null);
        tv_popular_cancel = (TextView) view.findViewById(R.id.tv_popular_cancel);//取消
        tv_popular_ok = (TextView) view.findViewById(R.id.tv_popular_ok);//确定
        ll_popular_buttons = (LinearLayout) view.findViewById(R.id.ll_popular_buttons);//确定  取消的 父布局
        lv_popular_data = (ListView) view.findViewById(R.id.lv_popular_data);//listView
        tv_popular_title = (TextView) view.findViewById(R.id.tv_popular_title);//标题
        iv_popular_down = (ImageView) view.findViewById(R.id.iv_popular_down);
    }


    private void initListener() {
        tv_popular_cancel.setOnClickListener(this);
        tv_popular_ok.setOnClickListener(this);
        iv_popular_down.setOnClickListener(this);
        lv_popular_data.setOnItemClickListener(this);
    }


    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.tv_popular_ok:
                //确定按钮
                getSelectPostion();
                break;
            case R.id.tv_popular_cancel:
                //取消按钮的
                popupWindow.dismiss();
                break;
            default:
                break;
        }
    }

    private void getSelectPostion() {
        SparseBooleanArray checkedItems = lv_popular_data.getCheckedItemPositions();
        if (checkedItems == null || checkedItems.size() == 0) {
            return;
        }

        int arrays[] = new int[checkedItems.size()];
        for (int i = 0; i < checkedItems.size(); ++i) {
            final int position = checkedItems.keyAt(i);//点击的 position
            final boolean isChecked = checkedItems.valueAt(i);  //是否点击
            if (isChecked) {
                arrays[i] = position;
            }
        }
        popularItemOnclick.getOnItemPosition(arrays);
        popupWindow.dismiss();
    }


    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        //当选择的模式是单选的时候 则返回它当前的id
        if (CHOICE_STATE == ListView.CHOICE_MODE_SINGLE) {
            //当只有单选模式才是可以的
            popularItemOnclick.getOnItemPosition(new int[]{position});
            popupWindow.dismiss();
            return;
        }
    }


    /**
     * 刷新数据
     *
     * @param datas
     * @param parnent
     * @param type
     * @param indexs
     * @return
     */
    public CustomerPopular refreshListDatas(List<PopularWindowBean> datas, View parnent, int type, int[] indexs) {
        if (this.datas == null)
            this.datas = new ArrayList<>();
        this.datas.clear();
        this.datas.addAll(datas);
        setAdapter(parnent, type, indexs);
        return this;
    }


    private void setAdapter(View parent, int type, int[] index) {
        //items upcheck
        this.CHOICE_STATE = type;
        clearSelection();
        lv_popular_data.setChoiceMode(type);
        if (adapter == null) {
            lv_popular_data.setItemsCanFocus(false);
            adapter = new PopularAdapter(context, datas);
            lv_popular_data.setAdapter(adapter);
        }
        //liseview显示最上面
        for (int i = 0; index != null && i < index.length; i++) {
            lv_popular_data.setItemChecked(index[i], true);
        }
        lv_popular_data.setSelection(0);
        adapter.notifyDataSetChanged();

        showAsDropDown(parent);
    }


    //清除状态
    private void clearSelection() {
        final int itemCount = lv_popular_data.getCount();
        for (int i = 0; i < itemCount; ++i) {
            lv_popular_data.setItemChecked(i, false);
        }
    }


    private void showAsDropDown(View parent) {
        popupWindow = budilder.setView(view).setBackGroundLevel(0.5f)
                .setOutsideTouchable(true)
                .create();


        changePopularSize();

        if (Build.VERSION.SDK_INT >= 24) {
            int[] location = new int[2]; // 记录anchor在屏幕中的位置
            parent.getLocationOnScreen(location);
            int offsetY = location[1] + parent.getHeight();
            if (Build.VERSION.SDK_INT == 25) { // Android 7.1中，PopupWindow高度为 match_parent 时，会占据整个屏幕
                // 故而需要在 Android 7.1上再做特殊处理
                int screenHeight = ScreenUtils.getScreenHeight(context); // 获取屏幕高度
                popupWindow.setHeight(screenHeight - offsetY); // 重新设置 PopupWindow 的高度
            }
            popupWindow.showAtLocation(parent, Gravity.CENTER, 0, 0);

        } else {
            popupWindow.showAtLocation(parent, Gravity.CENTER, 0, 0);
        }

    }

    //改变popular的大小
    private void changePopularSize() {
        int height_default = (int) (ScreenUtils.getScreenHeight(context) * 0.7f);
        view.measure(0, 0);
        int measuredHeight = view.getMeasuredHeight();
        int count = 0;
        int size = adapter.getCount();
        for (int i = 0; i < size; i++) {
            View childAt = adapter.getView(i, null, lv_popular_data);
            if (childAt != null) {
                childAt.measure(0, 0);
                int measuredHeight2 = childAt.getMeasuredHeight();
                count += measuredHeight2;
            }
        }
        measuredHeight += count;
        if (measuredHeight < height_default) {
            popupWindow.setHeight(ViewGroup.LayoutParams.WRAP_CONTENT);
        } else {
            popupWindow.setHeight(height_default);
        }
    }


    /**
     * 接口 点击 popular
     */
    public interface OnProductPopularItemOnclick {
        /**
         * 得到点解的postion
         *
         * @param
         */

        void getOnItemPosition(int[] positionArray);
    }

    public OnProductPopularItemOnclick popularItemOnclick;

    public void setPopularItemOnclick(OnProductPopularItemOnclick popularItemOnclick) {
        this.popularItemOnclick = popularItemOnclick;
    }
}
```
[具体代码查看github](https://github.com/songjiabin/androidDemo)





