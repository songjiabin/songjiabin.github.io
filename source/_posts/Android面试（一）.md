---
title: Android面试（一）
date: 
tags: 
- android 
- 面试
copyright: true
categories: android
---


<blockquote class="blockquote-center">我们唯一不会改正的缺点是软弱</blockquote>

<!-- more -->

##### Activity4中状态
- Active:Activity处于栈顶
- Paused:可见单不可交互（上面有一个透明的activity或者弹出了系统框）
- Stopped:不可见
- Killed:系统回收掉

当内存不足的话很有可能回收掉栈底的activity 

##### 异常情况下Activity生命周期

当activity发生异常终止的时候会调用
```Java

  //向里面写入数据
 @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
    }
 
// 从里面拿去异常保存的数剧
    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
    }
```


在onCreate中也有bundle的参数。他也是当出现异常的时候可以通过里面的bundle来恢复数据。

**当恢复数据的时候onCreate和onRestoreInstanceState的区别是：如果一旦调用onRestoreInstanceState此方法，那么里面的bundle绝对有数据。不用进行非空判断，而onCreate则不一定有数据。**

- 从一个Activity创建后启动另一个Activity并返回的完整log输出：

![20180723094015310.jpg](https://upload-images.jianshu.io/upload_images/2953304-84f139a1e5280a44.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 

**ActivityA --> ActivityB -->点击返回到-->ActivityA**

 


- ActivityA:onCreate-onStart-onResume-onPause
- ActivityB:onCreate-onStart-onResume
- ActivityA:onStop

然后点击ActivityB的返回键

- ActivityB:onPause
- ActivityA:onRestart-onResume
- ActivityB:onStop-onDestory

##### Activity通信方式
- intent 传递数据（bundle）
- 全局变量
- 类静态的变量



##### Activity向Fragment传递信息方式

- 使用Arguments、bundle
```Java
Activity端 使用bundle和arguments发送
  @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_my_view);

        fl_fragment = (FrameLayout) findViewById(R.id.fl_fragment);
        myFristFragment = new MyFristFragment();
        Bundle bundle = new Bundle();
        bundle.putString("key", "value");
        myFristFragment.setArguments(bundle);

        addFragment();
    }


    private void addFragment() {
        FragmentManager frgmentManager = getFragmentManager();
        FragmentTransaction transaction = frgmentManager.beginTransaction();
        transaction.replace(R.id.fl_fragment, myFristFragment);
        transaction.commit();
    }
```
- - - 
```Java
服务端使用getArguments接收
 @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.activity_view_demo, null);
        return view;
    }


    @Override
    public void onStart() {
        super.onStart();
        //Fragment是否已经依附于Activity上面
        if (isAdded()) {
            String value = this.getArguments().getString("key");
            Log.i(">>>", value);
        }
    }
```
- 在activity中定义方法。在Fragment中进行调用

```Java
Activity中定义方法
 public String getData() {
        return "这个是Activity返回的数据哦";
    }
```

- - -
```Java
Fragment中接受数据
// 当Fragment与Activity发生关联时调用。
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        //context强转成关联的Activity
        String data = ((MyViewActivity) context).getData();
        Log.i(">>>", data);
    }
```
##### Fragment向Activity发送数据

- 定义接口。让Activity实现。在Fragment中进行触发接口，activity中就可以得到从Fragment传来的数据。

```Java
定义接口
public interface AFInterface {
    void onPoress(String content);
}
```
- -  - 
```Java
Activity实现此接口
public class MyViewActivity extends FragmentActivity implements AFInterface {
    //接收从Fragment传来的数据
    @Override
    public void onPoress(String content) {
        Toast.makeText(this, content, Toast.LENGTH_SHORT).show();
    }
    
}
```

- - - 

```Java 

Fragment类进行触发接口回调
public class MyFristFragment extends Fragment {


    private AFInterface afInterface;
    public void setAfInterface(AFInterface afInterface) {
        this.afInterface = afInterface;
    }
    
     @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.activity_view_demo, null);
        TextView text_test = (TextView) view.findViewById(R.id.text_test);
        text_test.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                afInterface.onPoress("从Fragment到Activity");
            }
        });
        return view;
    }


    @Override
    public void onStart() {
        super.onStart();
        //Fragment是否已经依附于Activity上面
        if (isAdded()) {
            String value = this.getArguments().getString("key");
            Log.i(">>>", value);
        }
    }


    // 当Fragment与Activity发生关联时调用。
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        //如果已经实现此接口的话。
        if (context instanceof AFInterface) {
            afInterface = (AFInterface) context;
        } else {
            throw new IllegalArgumentException("类型错误...");
        }
    }
    
    //避免内存泄露。当Fragment与activity不关联的时候，制为null 
      @Override
    public void onDetach() {
        super.onDetach();
        afInterface=null;
    }
    
    
    
}
```



##### Activity和Service通信

- 绑定服务。利用ServiceConnection类

首先定义Service
```Java
public class MyService extends Service {

    private String content = "初始化";

    //绑定服务。将会返回给实现它关联他的activity
    @Override
    public IBinder onBind(Intent intent) {
        return new MyIbinder();
    }
    
    public class MyIbinder extends Binder {

        public void setData(String content) {
            MyService.this.content = content;
            Log.i(">>>", content);
        }
    }
}
```
--- 
定义Activity
```Java
//实现 ServiceConnection 接口
public class ServiceActivity extends AppCompatActivity implements ServiceConnection {

    private MyService.MyIbinder myBinder = null;
    private Intent intent_service;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.layout_service);
        intent_service = new Intent(this, MyService.class);

        bindService(intent_service, this, Context.BIND_AUTO_CREATE);


        findViewById(R.id.btn_start).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //开启服务
                //得到此类后变可以传递数据给Service 
                if (myBinder != null)
                    myBinder.setData("传递的数据");
            }
        });
        findViewById(R.id.btn_stop).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //停止服务
                stopService(intent_service);
            }
        });

    }

    //绑定成功
    @Override
    public void onServiceConnected(ComponentName name, IBinder service) {
        //得到Service中的两个MyIbinder类。
        myBinder = (MyService.MyIbinder) service;
    }

    //绑定断开的时候
    @Override
    public void onServiceDisconnected(ComponentName name) {
        myBinder=null;
    }
```








##### Acivity启动模式

- Standard 正常启动 先进后出
- SingleTop 如果启动的Activity位于栈顶，那么不在重新创建Activity实例。那么重新启动位于栈顶的此Activity实例。并调用此onNewIntent(Intent intent)方法。**(新闻推送，IM聊天界面经常用到此模式)**
- SingleTask 查看栈中是否有需要你重新启动的Activity。有的话清除掉此Activity以上的Activity让启动的Activity位于栈顶。并调用此onNewIntent(Intent intent)方法。**（主界面用到的比较多）**
- SingleInstace  以SingleInstace启动的Activity具有全局唯一性。以SingleInstace启动的Activity具有独占性（任务栈中只能有它一个实例）



##### Service 


###### Service和线程(Thread)的区别
- 都没有界面
- Service运行在主线程。Thread运行在工作线程
- Thread一旦启动它的Activity销毁，那么Thread将不受控。Service全局只有一个实例，只要是Activity就可以关闭它，并且自身内部还可以控制自己。


###### 管理Service生命周期

![2621116-8c19ef221d98115a.png](https://upload-images.jianshu.io/upload_images/2953304-1eb104ebae36ab70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



- onCreate 创建一个服务
- onStart 开始一个服务
- onDestroy 销毁一个服务
- onBind 绑定一个服务
- onUnBind 解绑一个服务





---
- startService启动服务
> **onCreate()只会被调用一次(不管调用多少次startService)**、**onStartCommand()会调用多次（根据startService调用的次数定）**

- stopService 停止服务
> 调用了stopService后，内部会自行调用onDestroy（）。**注意：如果一个Service调用了bindService。那么调用stopService是不管用的。必须先解绑**

- bindService 绑定服务 
> 当调用了此方法内部会调用**onCreate()->onBind()** 

- unBindService 解绑服务
> 手动调用了此后内部会调用**onUnbind()-onDestroy()**





###### Service和IntentService和区别
- Service运行在主线程
- 继承Service，包含了Service的所有特性
- 内部有一个工作线程HanderThread来处理耗时操作（onCreate()中）
- IntentService当耗时操作完成以后会自动停止
- 每次执行一个后台任务就必须启动一次IntentService
- IntentService内部通过消息的方式发送给HandlerThread的。然后由Handler中的Looper来处理消息     
- 执行任务是顺序的



> Service 是运行在主线程中的 所以不建议再里面编写耗时的操作。




###### 启动服务和绑定服务先后次序

> 在一个Service中有多种状态，可能是启动状态也可能是绑定状态。根据顺序的问题会出现多种情况。

**注意**

**绑定服务依托绑定它的Activity**

**启动服务依托于当前的Service**



- 先绑定服务后启动服务
> 如果当前的Service实例，已经以绑定状态运行了。
然后再以启动状态运行，那么绑定服务将转为启动服务。这个时候如果之前绑定的Activity被销毁了，也不会影响服务的运行。 直到收到挺住服务的指令。
- 先启动服务后绑定服务
> 如果当前Service已经以启动状态运行了，后来又以绑定状态运行。 那么当前的启动服务状态并不会转为绑定服务状态。不过还是会和Activity绑定。当解除绑定后，还是会走Service的启动状态生命周期。





###### 序列化（Parcelable和Serializable）

对象于内存写到磁盘 序列化
对象于磁盘读到内存 反序列化

差异
- Serializable 系统会自动生成一个uid。并保存到存储的磁盘上。当反序列化的时候，会检测当前类的uid和磁盘中uid是否一致。
- Serializable比较浪费内存。推荐使用Parcelable
- Parcelable使用比较麻烦
- 如果是内存间的传递数据使用Serializable，如果是将数据写到文件的话尽量使用Serializable



##### AIDL (进程通信)
> 进程间通信
###### 写服务端
> 客户端可以从服务端调用数据
- 新建Aidl文件(IMyAidlInterface.aidl)。包名就和项目的包名一致就ok。
![TIM截图20181026154534.png](https://upload-images.jianshu.io/upload_images/2953304-faf8378120bac282.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 新建用于服务端和客户端数据交换的类**必须实现Parcelable接口**
```java
public class Book implements Parcelable {
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "book name：" + name;
    }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(this.name);
    }

    public void readFromParcel(Parcel dest) {
        name = dest.readString();
    }

    protected Book(Parcel in) {
        this.name = in.readString();
    }

    public static final Creator<Book> CREATOR = new Creator<Book>() {
        @Override
        public Book createFromParcel(Parcel source) {
            return new Book(source);
        }

        @Override
        public Book[] newArray(int size) {
            return new Book[size];
        }
    };

```
- 在aidl中定义实体类（相当于将.java类型的数据变为.aidl的bean）**包名需要和.java的包一致**

![TIM截图20181026174810.png](https://upload-images.jianshu.io/upload_images/2953304-27a058ae419f7af4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```Java
// Book.aidl
package com.example.mydemo.bean;

parcelable  Book;
```

- 完成接口
```
package com.example.mydemo;

// Declare any non-default types here with import statements
import com.example.mydemo.bean.Book;

interface IMyAidlInterface {


     void addBook(in Book book);

     List<Book> getBookList();


}

```
build项目。会在app-build-generated-source-aild-dubug看到完成好的.java文件。

- 完成Service
```Java
public class AidlService extends Service {
    private List<Book> mBooks;

    public AidlService() {

    }


    //客户端与服务端绑定的时候回调
    @Override
    public IBinder onBind(Intent intent) {
        mBooks = new ArrayList<>();
        Book book=new Book("客户端");
        mBooks.add(book);
        Toast.makeText(getApplicationContext(),"绑定了",Toast.LENGTH_SHORT).show();
        return new MyBinder();
    }
    
    //客户端拿到MyBinder对象。从而调用这里接口。实现和服务端的交互
    class MyBinder extends IMyAidlInterface.Stub {


        @Override
        public void addBook(Book book) throws RemoteException {
            mBooks.add(book);
            Log.i("《《《",book.getName());
        }

        @Override
        public List<Book> getBookList() throws RemoteException {

            return mBooks;
        }
    }
}

```






###### 写客户端

- 将服务端写的aidl文件、实现Parcelable接口的实体类。复制到客户端（**包名必须和服务端的一致**）。build项目。

在客户端Activity中实现ServiceConnection接口。用于和客户端连接。（类似bindService和Service通信）
```Java
Activity界面



 @Override
    public void onServiceConnected(ComponentName name, IBinder service) {
        //得到客户的aidl中的service 
        mAidl = IMyAidlInterface.Stub.asInterface(service);
    }

    @Override
    public void onServiceDisconnected(ComponentName name) {

    }
    
        public void onClick(View view) {
        try {
            Intent intent = new Intent();
            intent.setPackage("com.example.mydemo");
            bindService(intent, this, BIND_AUTO_CREATE);
            //调用服务端的代码
//            List<Book> books =  mAidl.getBookList();
//            Toast.makeText(this,books.get(0).getName(),Toast.LENGTH_SHORT).show();
            Book book = new Book("newBook222222");
            mAidl.addBook(book);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
















