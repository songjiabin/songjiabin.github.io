---
title: 封装adapter
date: 
tags: 
- android 
- 工具类
copyright: true
categories: android
---


<blockquote class="blockquote-center">皑如山上雪，皎若云间月。</blockquote>

<!-- more -->

看以前的项目，发现还是用的ListView。所以想重新封装下。

##### 先看看普通的ListView常见写法


```Java
public class LastAdapter extends BaseAdapter {

    private Context context;
    private List<Bean> beanList;

    public LastAdapter(Context context, List<Bean> beanList) {
        this.context = context;
        this.beanList = beanList;
    }

    @Override
    public int getCount() {
        return beanList.size();
    }

    @Override
    public Object getItem(int position) {
        return beanList.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder viewHolder;
        if (convertView == null) {
            viewHolder = new ViewHolder();
            convertView = LayoutInflater.from(context).inflate(R.layout.item_content, parent,false);
            viewHolder.imageView = (ImageView) convertView.findViewById(R.id.iv_imageView);
            viewHolder.textView = (TextView) convertView.findViewById(R.id.tv_textView);
            convertView.setTag(viewHolder);
        } else {
            viewHolder = (ViewHolder) convertView.getTag();
        }

        viewHolder.textView.setText(beanList.get(position).getText());


        return convertView;
    }

    
    private class ViewHolder {
        private TextView textView;
        private ImageView imageView;
    }


}
```

##### 分析

分析普通的写法，主要针对getView(...)中的代码。是不是应该把ViewHolder封装一下？后续所有的adapter就不用ViewHolder了。
封装ViewHolder的时候，必须要存储里面View。就像`private TextView textView;`。我们可以用集合存起来，用到的时候取里面的View。于是想到了Map。而`SparseArray`比Map更高效，并且我们根据id，存入View。也符合SparseArray的特点。而根据ViewHolder的复用机制，我们将ViewHolder存入到布局的item中并用Tag标记。下次直接从布局item中取出ViewHolder。达到了复用的效果。

##### 下面是封装的ViewHolder的代码

```Java
public class CommViewHolder {


    private SparseArray<View> viewHolderArray = null;
    private int position;
    private View convertView;

    public CommViewHolder(Context context, int position, int layoutId, ViewGroup parent) {
        this.viewHolderArray = new SparseArray<>();
        this.position = position;
        //当第一次创建的时候，创建convertView
        convertView = LayoutInflater.from(context).inflate(layoutId, parent, false);
        //将此ViewHolder放入到convertView中
        convertView.setTag(this);
    }

    /**
     * 提供外界调用的静态方法
     *
     * @param context
     * @param convertView
     * @param layouId
     * @param parent
     * @param position
     * @return
     */
    public static CommViewHolder getCommViewHoler(Context context, View convertView, int layouId, ViewGroup parent, int position) {
        if (convertView == null) {
            //当convertView为空的时候，进行创建ViewHolder。
            return new CommViewHolder(context, position, layouId, parent);
        } else {
            //当不为空的时候，复用ViewHolder
            CommViewHolder viewHolder = (CommViewHolder) convertView.getTag();
            viewHolder.position = position;
            return viewHolder;
        }
    }

    /**
     * 通过 id  来获取 View
     *
     * @param viewId
     * @param <T>
     * @return
     */
    public <T extends View> T getView(int viewId) {
        View view = viewHolderArray.get(viewId);
        if (view == null) {
            view = convertView.findViewById(viewId);
            viewHolderArray.put(viewId, view);
        }
        return (T) view;
    }

    /**
     * 第一次创建的时候回创建convertView。 所以要返回这个convertView
     *
     * @return
     */
    public View getConvertView() {
        return convertView;
    }
}
```

看下面的图
![以前和现在的代码对比.png](https://upload-images.jianshu.io/upload_images/2953304-f6e6969e2c643b74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 封装adapter 
标准的adapter 需要继承BaseAdapter。重写很多方法。完全可以封装一下。

```Java
public abstract class CommAdapter<T> extends BaseAdapter {

    protected List<T> listBean;
    protected Context context;


    public CommAdapter(Context context, List<T> listBean) {
        this.listBean = listBean;
        this.context = context;

    }

    @Override
    public int getCount() {
        return listBean.size();
    }

    @Override
    public Object getItem(int position) {
        return listBean.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        //固定代码  找到指定的ViewHolder
        CommViewHolder commViewHolder = CommViewHolder.getCommViewHoler(context, convertView, getItemlayoutId(), parent, position);

        //逻辑代码 让 自类去实现
        convert(position, commViewHolder);

        //并返回comvertView
        return commViewHolder.getConvertView();
    }


    public abstract void convert(int position, CommViewHolder commViewHolder);


    //继承的时候只需要提供 布局id
    public abstract int getItemlayoutId();


}
```

具体的哪个adapter继承就好了。

到时候如果代码少的话，直接就可以使用匿名内部类了
```Java
 NowAdapter adapter=new NowAdapter(this,getListData()){
            @Override
            public void convert(int position, CommViewHolder commViewHolder) {
                commViewHolder.setText(R.id.tv_textView, listBean.get(position).getText()).setText(R.id.tv_time, System.currentTimeMillis() + "");
            }

            @Override
            public int getItemlayoutId() {
                return R.layout.item_content;
            }
        };
        listView.setAdapter(adapter);

``` 

代码地址：https://github.com/songjiabin/MyAdapter

参考：hyman 

