---
title: Object-box 数据库使用
date: 
tags: 
- android 
- 开源项目学习
copyright: true
categories: 开源项目学习
---



<blockquote class="blockquote-center">要么庸俗，要么孤独</blockquote>

<!-- more -->

##### objectbox 数据库操作


官网：https://docs.objectbox.io


> 这个数据库是比较出名的另一个 Android 数据库greenDAO的母公司开发的，一个NoSQL数据库，也就是一个非 SQLite 的数据库，据说速度完爆任何移动数据库
 



###### 初始化 引用
项目根目录下
```Java
buildscript {
    ext.objectboxVersion = '2.2.0' //最新Object-box版本
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'
         
        //引用
        classpath "io.objectbox:objectbox-gradle-plugin:$objectboxVersion"
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://jitpack.io" }
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```

model目录下：
```Java
apply plugin: 'io.objectbox' // apply last
```

###### 新建实体
```Java 
@Entity
public class JzmObjectBoxBean {
    
    @Id
    private long id;

    private String name;
    private String title;
    .... 
}
```
**build下项目**在会出现后面初始化全局的变量MyObjectBox 类
    
build完成后可以再项目modul目录下看到 objectbox-models的模块，下面生成了一个.json文件。此文件就是你所创建的数据库所对应的文件。
**objectBox 生成的objectbox-models/default.json，要一直随着代码更新，但不能删除重新更新，否则会导致UID不一致而崩溃**    




###### 初始化项目
在appLication中使用  

```Java 
boxStore = MyObjectBox.builder().androidContext(this).build();
```

###### 具体得到实体类的操作对象

```Java
Box<JzmObjectBoxBean> objectBoxBean = JzmApplication.getBoxStore().boxFor(JzmObjectBoxBean.class);
```


###### 开启调试界面
在model的build.gradle中添加依赖
```Java
debugImplementation "io.objectbox:objectbox-android-objectbrowser:$objectboxVersion"
releaseImplementation "io.objectbox:objectbox-android:$objectboxVersion"
```

**注意：当我再加上上面依赖后，总是build失败，com.android.dex.DexException这个错误，当把 `apply plugin: 'io.objectbox' `放到下面的时候就解决了。**


开始调试界面的话。需要开通的权限
```Java
<uses-permission android:name="android.permission.INTERNET" />
//在针对API 28或更高版本时运行保持活动服务所必需的
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
```

application onCreate() 中加上允许调试的代码

```Java 
if (BuildConfig.DEBUG) {
    boolean started = new AndroidObjectBrowser(boxStore).start(this);
    Log.i("ObjectBrowser", "Started: " + started);
}
```

OK 当你运行你的app时候会出现通知栏目，打开便是数据库的查看页。

想要在在电脑端查看数据库的话执行。
```Java 
adb forward tcp:8090 tcp:8090
```
便可查看
**使用模拟器不好使，真机可行**



###### 增（put）

```Java
/**
     * 增
     * 当插入的数据中id为0 那么表示插入的是一个新的数据。数据库会分配一个新的id
     */
    @Override
    public void insert(LoginBean loginBean) {
        Box<LoginBean> loginBox = MyApplication.getBoxStore().boxFor(LoginBean.class);
        loginBox.put(loginBean);
    }
    
    
    
```             


###### 删(remove)
```Java
  /**、
     * 删除具体的某个
     * @param loginBean
     */
    @Override
    public void delete(LoginBean loginBean) {
        loginBox.remove(loginBean);
    }

    /**
     * 删除所有的数据
     */
    @Override
    public void deleteAll() {
        loginBox.removeAll();
    }

```
###### 改(put)
改和增都是用的 put 。检测到如果put的对象之前有那么就会进行修改



###### 查（query）
```Java
 /**
     * 根据id查出数据
     *
     * @param id
     * @return
     */
    @Override
    public LoginBean getById(Long id) {
        LoginBean loginBean = loginBox.query().equal(LoginBean_.id, id).build().findFirst();
        return  loginBean;
    }
```


**注意：@ID标志的是你的id，此id不能任意指定，当分配id的时候回自动增长。如果想任意分配id的话，要修改注解。像：@Id(assignable = true)。这样注解的表示可以任意指定你的id了。**
 
 
 
###### 实体注解 
- @ID
> 值0（零）和-1（0xFFFFFFFFFFFFFFFF）不能用作ID。

> ID为0的对象(如果ID是Long类型，则为 null）被认为是新的。插入这样的对象，数据库会认为是新数据并分配一个新的id。

> 如果您尝试使用ID大于当前最高ID的对象，则ObjectBox将引发错误。

> 如果要自己分配ID，您可以将ID注解更改为@Id(assignable = true) 。这将允许ID可以有任何值，包括0和-1。

- @Index

> 当需要对一个参数频繁操作。可以引入@Index。提高性能 比如当有个参数是和id具有相同的性质。要根据此id进行查询等操作。那么就得加上此属性。


- @NameInDb
> 允许您在数据库级别上为属性自定义一个名称。这允许您重命名Java字段而不影响数据库级别上的属性名称(方便修改类)。 
在数据库中的名字为 isLoginInDB 而非 isLogin
```Java
@NameInDb("isLoginInDB")
private boolean isLogin;
```


- @Transient
> 在数据库中不显示该字段。该属性不会被持久化
在数据库中该字段并没有生成        
```Java
@Transient
private String isHaveInDB;
```
###### 查询
- 单条件查询 查找用户中名字为“基本密码”的数据
```Java
List<LoginBean> loginBeanList = loginBox.query().equal(LoginBean_.loginName, "基本密码").build().find();
```
- 多条件查询 从所有用户中查询First name 是Joe、1970年以后出生、并且LastName 是O开头的数据：
```Java
List<LoginBean> loginBeanList = loginBox.query().eqeal(LoginBean_.firstName,"Joe").greater(LoginBean_yearOfBirth,1970).startWith(LoginBean_lastName,"0").find();
```

- find()
> 要返回匹配查询的所有实体

- findFirst()
> 要仅返回第一个结果

- findUnique()
> 如果你希望得到一个唯一的结果调用 findUnique()来代替。如果查询结果唯一，则返回该对象，如果查询结果不唯一，会抛出异常，如果没有查询结果则返回null。

- 重用query 和参数
```Java
Query<LoginBean> query = loginBox.query().equal(LoginBean_.loginName, "").build();
        List<LoginBean> joeList = query.setParameter(LoginBean_.loginName, "Joe").find();
        List<LoginBean> JbmmList = query.setParameter(LoginBean_.loginName,"基本密码").find();
```
这里我们使用了同一个查询对象来查找两组用户，每个用户的名字都不同。请注意， 构建查询时，我们仍然需要初始化firstName属性的值 。因为我们使用setParameter ()覆盖值。我们可以在初始构建查询时传递一个空字符串。
**重用query的好处是不用每次通过build来获取，频繁查询会提高性能。**
- 分页
> 当数据库中有大量数据时，这是非常有用的。你不能仅仅使用查询条件来限制结果，还可以使用Limit， 
用offset和limit参数查找（long offset ，long limit ）方法。


offset ：第一个 偏移量结果被跳过。 
limit ：返回此查询的数量。
```Java
查找所有的数据 查出来的结果跳过前面的5个，向后找10个 
List<LoginBean> loginBeanListS = loginBox.query().build().find(5,10);
```


- order 排序
```Java
//查找fristName是 Joe 并且  lastName 以降序排序并不区分大小写的数据      
loginBox.query().equal(LoginBean_.firstName, "Joe")
    .order(LoginBean__.lastName, QueryBuilder.DESCENDING | QueryBuilder.CASE_SENSITIVE)
    .find();
```

- in 在...之内
```Java
//查找id是 1 、 26  、27 的数据 
List<LoginBean> loginBeanList1 = loginBox.query().in(LoginBean_.id,newlong[]{1, 26,27}).build().find();

```


- between 之间
```Java
//查找 id 从30到35之间的数据   
List<LoginBean> loginBeanList1 =  loginBox.query().between(LoginBean_.id,30,35).build().find();
```


- notNull isNull 
```Java
//当loginName不为空并且id是10，和20的数据 
List<LoginBean> loginBeanList1 = loginBox.query().notNull(LoginBean_.loginName).in(LoginBean_.id, new long[]{10, 20}).build().find();
 
//当loginName为空并且id是10，和20的数据  
List<LoginBean> loginBeanList1 =loginBox.query().isNull(LoginBean_.loginName).in(LoginBean_.id, newlong[]{10, 20}).build().find();
```

- startsWith（以...开始）、endsWith(以...结束)、contains包含等。。。




- parameterAlias 设置属性的别名
 ```Java
  //将属性值 loginName 设置为 name 在进行查询 
QueryBuilder<LoginBean> query = loginBox.query()
        .equal(LoginBean_.loginName, "").parameterAlias("name");

List<LoginBean> loginBeanLisquery.build().setParameter("name","基本密码A0").find();
 ```


- 查找具体的某个属性 并返回属性的列表
```Java
  //得到 所有的loginName集合
String [] stringName = loginBox.query().in(LoginBean_.id,new long[]{31,32}).build().property(LoginBean_.loginName).findStrings();
```
**默认情况下，不返回空值。
但是，如果属性为null，则可以指定要返回的替换值**
```Java
// 返回 'unknown'  if email is null
String[] emails = userBox.query().build()
    .property(User_.email)
    .nullValue("unknown")
    .findStrings();
```


- distinct 过滤到相同项  
```Java 
//返回 属性中LoginName的所有值，并且 LoginName不能有重复的值    
String[] stringName = loginBox.query().in(LoginBean_.id, new long[]{31, 32}).build().property(LoginBean_.loginName).distinct().findStrings();
```
**.distinct(StringOrder.CASE_SENSITIVE) 表示区分大小写 **




###### 修改类名或者字段名字
- 修改类名
```Java
//在类上添加@Uid,然后build。然后再Gradle Console中会产生错误，并提示出一个@Uid字段。将这个字段复制到需要修改类下面
@Entity
@Uid(刚才生成的UID)
public class LoginBeanAgin {

    @Id
    private long id;


    private String loginName;
    private String loginPsw;
    ...
}

将生成的uid赋值后并修改类的名字重新build，便修改成功。成功后，刚才的uid即可删除
```

- 修改字段名称和类名一致并且之前的数据并不会丢失



后续待补...

[github地址](https://github.com/songjiabin/androidDemo/blob/master/app/src/main/java/com/example/mydemo/activity/ObjectBoxActivity.java)










