---
title: Android 开发艺术探索学习
date: 
tags: 
- android 
- 读书
copyright: true
categories: android
---



<blockquote class="blockquote-center">长风破浪会有时，直挂云帆济沧海</blockquote>

<!-- more -->

> 最近在读这本书。写一下自己的感悟和理解。顺便做个笔记
###### onStart 和 onResume 。onPause和onStop 他们之间有什么实质性的不同呢？

onStart onStop 是从Activity是否可见这个角度来回调的。
onResume onPasue 是从Activity 是否位于前台这个角度来回调。除了这两个区别没别的明显区别了。

###### 在onPause中不能做太重量级、耗时的操作。因为在onPause执行完成后才能打开新的Activity。新的Activity才能执行onResume方法。

###### 为了在屏幕翻转或者出现异常情况下不让Activity生命周期重建。

设置在配置文件中
```Java
 <activity android:name=".MainActivity"
            android:configChanges="orientation|screenSize"
            >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```
上面的orientation 是屏幕方向发生了改变 。 screenSize是当屏幕的尺寸信息发生了改变，比如旋转屏幕的时候，屏幕的尺寸会发生改变。这个值和编辑选项有关系。在编译配置中minSdkVersion和targetSdkVersion均都低于13的时候，此选项不会导致Activity重启。一般情况下。两个值起码有一个会大于13。所以当你想让Activity的生命周期不重新来的话，设置两个选项就好了。 


######  默认情况下，Activity启动的时候，会默认的放到启动它的任务栈中。

当从Application中启动Activity的时候会报错：
```Java
 Caused by: android.util.AndroidRuntimeException: Calling startActivity() from outside of an Activity  context requires the FLAG_ACTIVITY_NEW_TASK flag. Is this really what you want?
```
是因为启动它的Application并没有所在的任务栈。所以被启动的Activity不知道该去哪个任务栈中去。
解决方法是：给被启动的Activity指定一个任务栈
```Java
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        Intent intent = new Intent(this, MainActivity.class);
        //指定任务
        //指定的任务栈的类型是 singleTask 
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        this.startActivity(intent);
    }
}
```


######  任务栈中有一个参数TaskAffinity。 标识了任务栈的名字。默认为包名。allowTaskReparenting知识 

主要和allowTaskReparenting属性和singleTask配对使用。另外任务栈和分为前台任务栈和后台任务栈。后台任务栈的Activity为暂停状态。

TaskAffinity 和 singleTask（如果栈中没有会创建一个需要的任务栈） 配对使用的时候。被指定两个属性的Activity会运行在指定TaskAffinity属性名字的任务栈中


TaskAffinity 和 allowTaskReparenting 结合使用。
比如 应用A 启动了 应用B中Activity。如果被启动的该Activity的allowTaskReparenting=true。的话。该应用B中的此Activity会直接从A的任务栈转移到B的任务栈中去。

列子： 应用A启动应用B中的ActivityC。然后点击home键盘。再启动应用B。效果是：显示的事刚才A 启动的 ActivityC ,并没有显示B的主Activity。 原因是 A启动了B中C的时候。c会跑到A的任务栈中，但是C是属于B的。所以C创建了任务栈（包名）。当再次点击B的时候已经有任务栈了 就没有创建直接显示C。



###### IPC 进程间通信或者跨进程通信

- 两个进程之间进行数据交换的过程。Android中最有特色的通信方式就是Binder了，通过binder可以轻松实现进程间通信。出了Binder，还支持Socker。通过Socker可以实现任意两个终端间的通信。当然同一个设备上的两个进程通过Socker通信也是可以的

- 什么时候需要多进程呢？
- 一个应用因为某些原因需要采取多进程的方式来实现。可能是某些特殊的模块必须使用单进程，可能是为了加大使用内存为了获取更多的内存空间。
- 当前应用需要向其他应用获取数据

###### ShareUID 
Android系统为每个应用分配一个唯一的UID。具有相同UID的应用才能共享数据。两个应用必须有个相同的ShraeUID，而且签名文件必须相同。才可以共享数据。
```Java
应用A 
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:sharedUserId="com.share"
          package="jzm.jeno.com.demob">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>

                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>

</manifest>

应用B
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:sharedUserId="com.share"
          package="jzm.jeno.com.demoa">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>

                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>

</manifest>
```
很明显上面两个应用的包名不同，但是ShareUID是一样的。
下面从应用A中获取应用B中的数据
```Java

        try {
            //得到应用B的包名 
            Context ct = this.createPackageContext("jzm.jeno.com.demob", Context.CONTEXT_IGNORE_SECURITY);
            //得到应用B的shrarePreferceces文件
            SharedPreferences sp = ct.getSharedPreferences("appInfo", MODE_PRIVATE);
            String str2 = sp.getString("appname", "service");
            Log.d("mythou", "share preference-->" + str2);
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
```
**如果想得到另一个应用的资源文件，必须要存在该资源文件所对应的名字。否则会编译报错**



###### 进程间的通信的问题
- 静态变量和单例失效
- 线程间同步无效
- SharePreferences 的可靠性下降
> 因为SharePreferences 不支持两个进程之间同时写操作。否则导致数据的丢失。因为SharePreferences 是读写的xml文件。并发读写会出问题的。
- Application 会多次创建
> 每当创建一个应用，系统会为其分配独立的虚拟机内存。然后运行其Application。每当运行一个进程就会创建一个AppLicaton。

###### Serializable
> 序列化和反序列。其中 serialVersionUID 的作用很大。当序列化的时候，将serialVersionUID写入到硬件中。当反序列化的时候，拿出serialVersionUID和类中的serialVersionUID进行对比。如果一样则序列化成功。如果不写serialVersionUID的话，会自动生成serialVersionUID。当实体类改变的时候，serialVersionUID也会改变。反序列话的时候，就不能成功。

###### Parcelableh和Serializable区别

> Parcelableh和Serializable区别  都可以用于序列化。Serializable是Java序列化接口，用起来开销很大。因为里面要进行很多的io操作。Parcelableh是Android独有的序列化方式。Parcelableh主要用在内存的序列化上。当将对象序列化到硬件设备或者将对象序列化进行网络传输的话。这种情况建议使用Serializable。


######  跨进程通信之AIDL
- Binder 
> Android的一个类，实现了IBinder接口。从IPC来说它是Android中一种跨进程通信方式。从应用层来说Binder是服务端和客户端通信的媒介。当bindServer的时候，服务端会返回一个包含服务端的相关数据的Binder对象。通过就可以获取服务端的数据了。

- AIDL
> 跨进程通信的方式。目的是要看看通过AIDL生产的Binder类。
- 创建AIDL
Book为实体类（必须实现Parcelableh）
```java
public class Book implements Parcelable {
    public int BookId;

    public String BookName;

    .... 
}

```
Book.aidl 为进程间通信需要的实体类型（先创建此类，不然会报不让名字重复）
```Java
// Book.aidl
package com.example.sonbjiabin.myapplication.aidl;

// Declare any non-default types here with import statements
//用于传递数据的实体
parcelable Book;

```
IBookManager.aidl 接口(远程客户端用于调用的接口)
下面是目录结构
```Java
// IBookManager.aidl
package com.example.sonbjiabin.myapplication;
import com.example.sonbjiabin.myapplication.aidl.Book;
// Declare any non-default types here with import statements

interface IBookManager {
    //Book注意导包
    List<Book> getBookList();

    void addBook(in  Book book);
}
```
![TIM截图20190120202305.png](https://upload-images.jianshu.io/upload_images/2953304-86fa9f37d935d6cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


完成后build。在app/build/source/.. 生产了IBookManager。
此IBookManager为生成的Binder类。
```Java
//继承IInterface接口 实现了asBinder方法  并返回了 IBinder 类型的类
public interface IBookManager extends android.os.IInterface
{
/** Local-side IPC implementation stub class. */
//内部类 实现了上面的IBookManager接口。继承了Binder 类。 所以Stub类就是一个 Binder
public static abstract class Stub extends android.os.Binder implements com.example.sonbjiabin.myapplication.IBookManager
{
private static final java.lang.String DESCRIPTOR = "com.example.sonbjiabin.myapplication.IBookManager";
/** Construct the stub at attach it to the interface. */
public Stub()
{
this.attachInterface(this, DESCRIPTOR);
}
/**
 * Cast an IBinder object into an com.example.sonbjiabin.myapplication.IBookManager interface,
 * generating a proxy if needed.
 */
public static com.example.sonbjiabin.myapplication.IBookManager asInterface(android.os.IBinder obj)
{
if ((obj==null)) {
return null;
}
android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
if (((iin!=null)&&(iin instanceof com.example.sonbjiabin.myapplication.IBookManager))) {
return ((com.example.sonbjiabin.myapplication.IBookManager)iin);
}
return new com.example.sonbjiabin.myapplication.IBookManager.Stub.Proxy(obj);
}
//说明Stub是Ibinder 是Binder
@Override public android.os.IBinder asBinder()
{
return this;
}
@Override public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException
{
switch (code)
{
case INTERFACE_TRANSACTION:
{
reply.writeString(DESCRIPTOR);
return true;
}
case TRANSACTION_getBookList:
{
data.enforceInterface(DESCRIPTOR);
java.util.List<com.example.sonbjiabin.myapplication.aidl.Book> _result = this.getBookList();
reply.writeNoException();
reply.writeTypedList(_result);
return true;
}
case TRANSACTION_addBook:
{
data.enforceInterface(DESCRIPTOR);
com.example.sonbjiabin.myapplication.aidl.Book _arg0;
if ((0!=data.readInt())) {
_arg0 = com.example.sonbjiabin.myapplication.aidl.Book.CREATOR.createFromParcel(data);
}
else {
_arg0 = null;
}
this.addBook(_arg0);
reply.writeNoException();
return true;
}
}
return super.onTransact(code, data, reply, flags);
}

//当服务端和客户端不在同一个进程的时候
private static class Proxy implements com.example.sonbjiabin.myapplication.IBookManager
{
private android.os.IBinder mRemote;
Proxy(android.os.IBinder remote)
{
mRemote = remote;
}
@Override public android.os.IBinder asBinder()
{
return mRemote;
}
public java.lang.String getInterfaceDescriptor()
{
return DESCRIPTOR;
}
@Override public java.util.List<com.example.sonbjiabin.myapplication.aidl.Book> getBookList() throws android.os.RemoteException
{
android.os.Parcel _data = android.os.Parcel.obtain();
android.os.Parcel _reply = android.os.Parcel.obtain();
java.util.List<com.example.sonbjiabin.myapplication.aidl.Book> _result;
try {
_data.writeInterfaceToken(DESCRIPTOR);
mRemote.transact(Stub.TRANSACTION_getBookList, _data, _reply, 0);
_reply.readException();
_result = _reply.createTypedArrayList(com.example.sonbjiabin.myapplication.aidl.Book.CREATOR);
}
finally {
_reply.recycle();
_data.recycle();
}
return _result;
}
@Override public void addBook(com.example.sonbjiabin.myapplication.aidl.Book book) throws android.os.RemoteException
{
android.os.Parcel _data = android.os.Parcel.obtain();
android.os.Parcel _reply = android.os.Parcel.obtain();
try {
_data.writeInterfaceToken(DESCRIPTOR);
if ((book!=null)) {
_data.writeInt(1);
book.writeToParcel(_data, 0);
}
else {
_data.writeInt(0);
}
mRemote.transact(Stub.TRANSACTION_addBook, _data, _reply, 0);
_reply.readException();
}
finally {
_reply.recycle();
_data.recycle();
}
}
}
// 当客户端调用的时候 知道是调用了 get 还是 add      
static final int TRANSACTION_getBookList = (android.os.IBinder.FIRST_CALL_TRANSACTION + 0);
static final int TRANSACTION_addBook = (android.os.IBinder.FIRST_CALL_TRANSACTION + 1);
}
//添加 getBooklist()方法
public java.util.List<com.example.sonbjiabin.myapplication.aidl.Book> getBookList() throws android.os.RemoteException;
//添加addBook()方法
public void addBook(com.example.sonbjiabin.myapplication.aidl.Book book) throws android.os.RemoteException;
}

```
上面是自动生成Binder代码。上面有注释



######  跨进程通信之 Messenger
> Messenger （实现了Parcelable）信使。通过他可以在不同的进程中传递Message对象，在Messenger中放入我们需要传递的数据，就可以轻松地实现数据的进程间传递了。Messenger是一种轻量级的IPC方案，它的顶层是AIDL。看洗下面的构造方法

```Java
  public Messenger(Handler target) {
        mTarget = target.getIMessenger();
    }
    
  //很明显是Binder相关 
  public Messenger(IBinder target) {
        mTarget = IMessenger.Stub.asInterface(target);
    }
    
```

进程间通信的话实现下服务端和客户端。服务端抒写服务，客户端连接服务通过Messenger进行通信。

- 服务端

```Java
public class MessageService extends Service {

    //用于处理客户端发送过来的消息
    private static class MessageHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 1:
                    Bundle bundle = msg.getData();
                    String key = bundle.getString("msg");
                    Log.i(">>>", key);
                    break;

                default:
                    super.handleMessage(msg);
                    break;
            }
        }
    }

    //新建Messenger 并关联 Handler
    //Messenger 的作用是将客户端发送的消息给MessageHandler处理
    private Messenger mMessenger = new Messenger(new MessageHandler());


    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        //在这里返回Messager的binder对象。
        //将此Binder返回到客户端    
        return mMessenger.getBinder();
    }
}
```
**此服务端运行在单独的进程中**
```Java
 <!--指定单独的进程 ：属于当前的私有进程  -->
        <service android:name=".MessageService"
                 android:process=":remote">
        </service>
```

- 客户端
```Java
public class MainActivity extends AppCompatActivity {
    private Messenger mService;

    //连接远程的服务端
    private ServiceConnection serviceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            //根据服务端返回的Binder对象创建Messenger对象
            mService = new Messenger(service);
//            Message message = Message.obtain(null, 1);
            Message message = new Message();
            message.what = 1;
            Bundle bundle = new Bundle();
            bundle.putString("msg", "Hello world");
            message.setData(bundle);
            try {
                mService.send(message);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Intent intent = new Intent(this, MessageService.class);
        bindService(intent, serviceConnection, Context.BIND_AUTO_CREATE);
    }
}
```

这样就通过 Messenger 完成了进程间的数据的传递了

当想要让服务端接受到消息后返回消息的话，需要做如下修改
- 服务端
> 只需要接收到消息后，创建消息并交给Messenger。让他交给客户端    
```Java
public class MessageService extends Service {


    //用于处理客户端发送过来的消息
    private static class MessageHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 1:
                    Bundle bundle = msg.getData();
                    String key = bundle.getString("msg");
                    Log.i(">>>", key);
                    
                    //得到Messenger对象
                    Messenger messenger = msg.replyTo;
                    //创建消息message
                    Message message = new Message();
                    Bundle bundle1 = new Bundle();
                    //给客户端发送 回应

                    bundle1.putString("key", "收到了...");
                    message.what = 2;
                    message.setData(bundle1);
                    try {
                        messenger.send(message);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                    break;

                default:
                    super.handleMessage(msg);
                    break;
            }
        }
    }

    //新建Messenger 并关联 Handler
    //Messenger 的作用是将客户端发送的消息给MessageHandler处理
    private Messenger mMessenger = new Messenger(new MessageHandler());


    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        //在这里返回Messager的binder对象
        return mMessenger.getBinder();
    }
}
```

- 客户端
> 客户端接收到消息的话，那么也该使用 Messenger ,并用其关联Handler接收消息。
```Java
public class MainActivity extends AppCompatActivity {
    private Messenger mService;

    private Messenger mGetReplyMessenger = new Messenger(new MessenagerHandler());


    private static class MessenagerHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 2:
                    String message = msg.getData().getString("key");
                    Log.i("》》》", message);
                    break;

                default:
                    break;
            }
        }
    }


    //连接远程的服务端
    private ServiceConnection serviceConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            //根据服务端返回的Binder对象创建Messenger对象
            mService = new Messenger(service);
//            Message message = Message.obtain(null, 1);
            Message message = new Message();
            message.what = 1;
            Bundle bundle = new Bundle();
            bundle.putString("msg", "Hello world");
            message.setData(bundle);
            try {
                 //告诉服务端  message 里面的 Messenger 是这个  当返回的时候知道在那里接收
                message.replyTo = mGetReplyMessenger;
                mService.send(message);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {

        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Intent intent = new Intent(this, MessageService.class);
        bindService(intent, serviceConnection, Context.BIND_AUTO_CREATE);

    }


}
```
**重点是    `message.replyTo = mGetReplyMessenger`** 让message中的messenger指定到客户端中。

上面是两个


######  跨进程通信之 Messenger 



 