---
title: android爬虫-Fruit
date: 
tags: 
- android 
copyright: true
categories: android
---



<blockquote class="blockquote-center">在生命里，我们几乎每时每刻都在犯错</blockquote>

<!-- more -->
#### 效果图

网站效果：
![TIM截图20181113145717.png](https://upload-images.jianshu.io/upload_images/2953304-bf73a08d45109152.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


手机展示：
![aa.gif](https://upload-images.jianshu.io/upload_images/2953304-28686df0f0e99420.gif?imageMogr2/auto-orient/strip)



#### 概述
Fruit是一个解析html的框架。可以将html文件解析成本地实体文件。


Fruit地址：https://github.com/ghuiii/Fruit

解析的网址为：http://www.wyl.cc/tag/xinlingjitang/page/1

配合Rxjava、okhttp、retrofit使用
#### 导包
```Java
compile 'io.reactivex.rxjava2:rxjava:2.1.3'
compile 'com.squareup.retrofit2:retrofit:2.3.0'

compile 'com.squareup.retrofit2:adapter-rxjava2:2.3.0'
compile 'io.reactivex.rxjava2:rxandroid:2.0.1'
compile 'com.squareup.okhttp3:okhttp:3.9.0'
compile 'com.squareup.okhttp3:logging-interceptor:3.9.0'

/*解析html*/
compile 'me.ghui:fruit-converter-retrofit:1.0.5'
compile 'me.ghui:global-retrofit-converter:1.0.1'

/*converter-gson 用于将请求结果转换为实体类型*/
compile 'com.squareup.retrofit2:converter-gson:2.3.0'
/*converter-scalars 用于将请求结果转换为基本类型，一般为String*/
compile 'com.squareup.retrofit2:converter-scalars:2.3.0'
```
#### 注意
刚开始因为用了 converter:1.0.4导致一直报错：

```Java
Error:Execution failed for task ':app:transformClassesWithDexBuilderForDebug'.
```
应该是和后面导入的```converter-gson```或者是`converter-scalars`不兼容。当改成`fruit-converter-retrofit:1.0.5`后问题解决。

#### 初始化 OkHttp Retrofit Fruit
> 基本配置

```Java 
public class ApiStrategy {

    //读超时长，单位：毫秒
    public static final int READ_TIME_OUT = 7676;
    //连接时长，单位：毫秒
    public static final int CONNECT_TIME_OUT = 7676;


    /**
     * 设缓存有效期为两天
     */
    private static final long CACHE_STALE_SEC = 60 * 60 * 24 * 2;


    public static QuotaionsApis apiService;
    private Fruit mFruit;

    public static QuotaionsApis getApiService() {
        if (apiService == null) {
            synchronized (Api.class) {
                if (apiService == null) {
                    new ApiStrategy();
                }
            }
        }
        return apiService;
    }

    private ApiStrategy() {
        HttpLoggingInterceptor logInterceptor = new HttpLoggingInterceptor();
        logInterceptor.setLevel(HttpLoggingInterceptor.Level.BODY);
        //缓存
        File cacheFile = new File(MyApplication.getContextObject().getCacheDir(), "cache");
        Cache cache = new Cache(cacheFile, 1024 * 1024 * 100); //100Mb

        //增加头部信息
        Interceptor headerInterceptor = new Interceptor() {
            @Override
            public Response intercept(Chain chain) throws IOException {
                Request build = chain.request().newBuilder()
                        //.addHeader("Content-Type", "application/json")//设置允许请求json数据
                        .build();
                return chain.proceed(build);
            }
        };

        //创建一个OkHttpClient并设置超时时间
        OkHttpClient client = new OkHttpClient.Builder()
                .readTimeout(READ_TIME_OUT, TimeUnit.MILLISECONDS)
                .connectTimeout(CONNECT_TIME_OUT, TimeUnit.MILLISECONDS)
                .addInterceptor(mRewriteCacheControlInterceptor)
                .addNetworkInterceptor(mRewriteCacheControlInterceptor)
                .addInterceptor(headerInterceptor)
                .addInterceptor(logInterceptor)
                .cache(cache)
                .build();

        Retrofit retrofit = new Retrofit.Builder()
                .client(client)
                .baseUrl(Constant.BASE_URL)
//                .addConverterFactory(GsonConverterFactory.create())//请求的结果转为实体类
                //适配RxJava2.0,RxJava1.x则为RxJavaCallAdapterFactory.create()
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .addConverterFactory(GlobalConverterFactory.create().add(FruitConverterFactory.create(getFruit()), Html.class))
                .build();
        apiService = retrofit.create(QuotaionsApis.class);


    }


    /**
     * 云端响应头拦截器，用来配置缓存策略
     * Dangerous interceptor that rewrites the server's cache-control header.
     */
    private final Interceptor mRewriteCacheControlInterceptor = new Interceptor() {
        @Override
        public Response intercept(Chain chain) throws IOException {
            Request request = chain.request();
            String url = request.url().toString();

            Log.i("http", url);
            String cacheControl = request.cacheControl().toString();
            if (!NetworkUtil.isNetworkConnected(MyApplication.getContextObject())) {
                request = request.newBuilder()
                        .cacheControl(TextUtils.isEmpty(cacheControl) ? CacheControl
                                .FORCE_NETWORK : CacheControl.FORCE_CACHE)
                        .build();
            }
            Response originalResponse = chain.proceed(request);
            if (NetworkUtil.isNetworkConnected(MyApplication.getContextObject())) {
                return originalResponse.newBuilder()
                        .header("Cache-Control", cacheControl)
                        .removeHeader("Pragma")
                        .build();
            } else {
                return originalResponse.newBuilder()
                        .header("Cache-Control", "public, only-if-cached, max-stale=" +
                                CACHE_STALE_SEC)
                        .removeHeader("Pragma")
                        .build();
            }
        }
    };

    private Fruit getFruit() {
        if (mFruit == null) {
            mFruit = new Fruit();
        }
        return mFruit;
    }

}
```


#### 配置请求参数
> 和基本的retrofit参数类似

```Java

public interface QuotaionsApis {
    //使用动态 url 匹配
    @GET
    @Html
    Observable<QuotationsBean> getQuotationsBean(@Url String url);
}
```


#### 根据网页结构得到相应的字段
![TIM截图20181113145228.png](https://upload-images.jianshu.io/upload_images/2953304-4d7e62935f8b2f7f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


对应的实体如下：
```Java
public class QuotationsBean {


    @Pick(value = "article")
    private List<QuotationsBeanItem> quotationsBeanItemList;
    
    ...

    public static class QuotationsBeanItem {
        //图片
        @Pick(value = "img", attr = Attrs.SRC)
        private String img;
        @Pick(value = "h2.entry-title > a")
        private String title;
        @Pick(value = "div.archive-content")
        private String content;
        @Pick(value = "span.entry-meta > span")
        private String time;
        @Pick(value = "h2.entry-title > a", attr = Attrs.HREF)
        private String link;
        @Pick(value = "span.cat > a")
        private String tag; 
        ...
    }

```


根据视图结构便可以得到对应的数据。

#### 完成响应 进行请求

```Java
 ApiMethods.getQuotations(new MyObserver<QuotationsBean>(MainActivity.this, new ObserverOnNextListener<QuotationsBean>() {
            @Override
            public void onNext(QuotationsBean quotationsBean) {
                List<QuotationsBean.QuotationsBeanItem> listData = quotationsBean.getQuotationsBeanItemList();
                if (page == 1) {
                    mList.clear();
                    mList.addAll(listData);
                    refreshLayout.finishRefresh(1000);
                } else if (page > 0) {
                    mList.addAll(listData);
                    refreshLayout.finishLoadMore(1000/*,false*/);
                }
                mAdapter.notifyDataSetChanged();
            }
        }), Constant.QUOTATIONS_URL + String.valueOf(page));
```


[github传送门](https://github.com/songjiabin/FruitDemo)