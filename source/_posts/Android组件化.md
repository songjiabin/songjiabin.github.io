---
title: Android组件化
date: 
tags: 
- android 
copyright: true
categories: android
---



<blockquote class="blockquote-center">那天的天很蓝，微风吹起你的长发和衣角，你在画风景，而我在画你。</blockquote>

<!-- more -->


参考文章：https://mp.weixin.qq.com/s/g1XIJ7vPl5yj1_thuV6b9Q

在此基础上做了改进。

前言不说，直接上思路。




![TIM截图20181225175205.png](https://upload-images.jianshu.io/upload_images/2953304-93714e8f0a851a70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



> 每个模块可以作为lib为app服务，而且每个模块又可以单独的运行。

##### 创建config.gradle文件并在build.gradle中引入
> 里面可以控制模块是否可以单独运行。和所以模块包括app(主模块)基础配置

```Java
ext {

    //有重用的地方写在这里面，保证代码一致
    android = [
            compileSdkVersion       : 28,
            buildToolsVersion       : "28.0.0",
            minSdkVersion           : 16,
            targetSdkVersion        : 28,
            versionCode             : 1,
            versionName             : "1.0.0",
            androidSupportSdkVersion: "28.0.0",
    ]


    // 定义某个模块是否可以单独运行
    isAloneRun = [
            // 是否需要单独编译 true表示需要，false表示不需要
            isNeedChatModule: false
    ]

    //所以库的依赖
    dependencies = [
            "appcompat-v7": "com.android.support:appcompat-v7:${android["androidSupportSdkVersion"]}",
            "constraint-layout"      : 'com.android.support.constraint:constraint-layout:1.1.3',//约束性布局
            "junit"                  : "junit:junit:4.12",
            "androidJUnitRunner"     : "android.support.test.runner.AndroidJUnitRunner",
            "espresso-core"          : 'com.android.support.test.espresso:espresso-core:3.0.2',//测试依赖，新建项目时会默认添加，一般不建议添加
            "test:runner"            : 'com.android.support.test:runner:1.0.2',//测试依赖，新建项目时会默认添加，一般不建议添加


            "easypermissions"        : 'pub.devrel:easypermissions:2.0.0',//权限管理
            "arouter-api"            : 'com.alibaba:arouter-api:1.3.1',//ARouter api
            "arouter-compiler"       : 'com.alibaba:arouter-compiler:1.1.4' //ARouter
    ]
}
```
引入build.gradle
```Java
apply from: "config.gradle"
```


##### 定义comm模快做为libray。其他模块进行入库此模块。每个模块中引入基础配置
```Java
apply plugin: 'com.android.library'

android {
    //引入进出配置     
    compileSdkVersion rootProject.ext.android["compileSdkVersion"]
    buildToolsVersion rootProject.ext.android["buildToolsVersion"]



    defaultConfig {
         //引入进出配置     
        minSdkVersion rootProject.ext.android["minSdkVersion"]
        targetSdkVersion rootProject.ext.android["minSdkVersion"]
        versionCode rootProject.ext.android["versionCode"]
        versionName rootProject.ext.android["versionName"]

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        
        //阿里框架 ARouter 引入代码
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [moduleName: project.getName()]
            }
        }

    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    testImplementation rootProject.ext.dependencies["junit"]
    androidTestImplementation rootProject.ext.dependencies["test:runner"]
    androidTestImplementation rootProject.ext.dependencies["espresso-core"]
    
    //引入ARouter 这里必须是api方式引入 Gradle 3.0后，用api引入，其他的模块才能够使用
    api rootProject.ext.dependencies["arouter-api"]
    annotationProcessor rootProject.ext.dependencies["arouter-compiler"]
    api rootProject.ext.dependencies["easypermissions"]

}
```


##### app主模块配置
```Java
apply plugin: 'com.android.application'

android {
    compileSdkVersion rootProject.ext.android["compileSdkVersion"]
    buildToolsVersion rootProject.ext.android["buildToolsVersion"]
    defaultConfig {
        applicationId "jzm.jeno.com.myapplication"
        minSdkVersion rootProject.ext.android["minSdkVersion"]
        targetSdkVersion rootProject.ext.android["minSdkVersion"]
        versionCode rootProject.ext.android["versionCode"]
        versionName rootProject.ext.android["versionName"]
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [moduleName: project.getName()]
            }
        }
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {


    implementation fileTree(include: ['*.jar'], dir: 'libs')

    implementation rootProject.ext.dependencies["appcompat-v7"]
    implementation rootProject.ext.dependencies["constraint-layout"]
    testImplementation rootProject.ext.dependencies["junit"]
    androidTestImplementation rootProject.ext.dependencies["test:runner"]
    androidTestImplementation rootProject.ext.dependencies["espresso-core"]




    api rootProject.ext.dependencies["arouter-api"]
    annotationProcessor rootProject.ext.dependencies["arouter-compiler"]


    implementation project(':commonlib')


    if (!rootProject.ext.isAloneRun.isNeedChatModule) {
        //如果模块可以自己编译并单独运行的话不要加载模块
        //如果模块当做一个library的话，那么加载模块
        implementation project(':chat')
    }

}
```

##### 单独运行的模块配置
里面有详细的注释    
```Java
if (rootProject.ext.isAloneRun.isNeedChatModule) {
    //当作为单独的项目的时候 为 application
    apply plugin: 'com.android.application'
} else {
    //当不是单独运行的时候，作为一个library
    apply plugin: 'com.android.library'
}

android {
    compileSdkVersion rootProject.ext.android["compileSdkVersion"]
    buildToolsVersion rootProject.ext.android["buildToolsVersion"]



    defaultConfig {
        //当作为一个单独的项目的时候，有自己的id
        if (rootProject.ext.isAloneRun.isNeedChatModule) {
            applicationId "jzm.jeno.com.chat"
        }
        minSdkVersion rootProject.ext.android["minSdkVersion"]
        targetSdkVersion rootProject.ext.android["targetSdkVersion"]
        versionCode rootProject.ext.android["versionCode"]
        versionName rootProject.ext.android["versionName"]

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [moduleName: project.getName()]
            }
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

     resourcePrefix "chat_"
    //如果在 library 的 build.gradle 中添加 resourcePrefix ，则所有资源必须以此 prefix 开头，否则会报错。


    // 配置当作为单独运行的一个项目审核，需要配置执行路径代码
    sourceSets {
        main {
            if (rootProject.ext.isAloneRun.isNeedChatModule) {
                //当作为单独的modul运行的时候
                manifest.srcFile 'src/main/runalone/AndroidManifest.xml'
                java.srcDirs = ['src/main/java', 'src/main/runalone/java']
                res.srcDirs = ['src/main/res', 'src/main/runalone/res']
            } else {
                //当作为一个library运行的时候
                manifest.srcFile 'src/main/AndroidManifest.xml'
            }
        }
    }


}

dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation rootProject.ext.dependencies["appcompat-v7"]
    implementation rootProject.ext.dependencies["constraint-layout"]
    testImplementation rootProject.ext.dependencies["junit"]
    androidTestImplementation rootProject.ext.dependencies["test:runner"]
    androidTestImplementation rootProject.ext.dependencies["espresso-core"]
    implementation project(':commonlib')

    //引入Arouter
    api rootProject.ext.dependencies["arouter-api"]
    annotationProcessor rootProject.ext.dependencies["arouter-compiler"]


}

当你模块作为单独运行的一个项目的时候，需要使用AndroidManifest.xml文件和application。这个时候就得我们重新创建一个新的applicaion文件和AndroidManifest.xml了。如下图所示。新增的runalone中的包名，方法名字要和作为library运行的时候一致才可以。
```

![TIM截图20181225182109.png](https://upload-images.jianshu.io/upload_images/2953304-5e91ea919a6242bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 使用跳转路由 ARouter

ARouter介绍看：https://github.com/alibaba/ARouter

**在需要使用ARouter的每个模块都必须进行引入否则回报ARouter there's no route matched的错误**
进行需要的地方进行跳转使用
```Java
//简单使用
 ARouter.getInstance().build("/chat/MainActivity") //目标页面
                       .navigation();
```
在跳转到的界面配置路径
```Java
@Route(path = "/chat/MainActivity")
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.chat_activity_main);
        ARouter.getInstance().inject(this);
        Toast.makeText(this, "收到传送过来的数据：", Toast.LENGTH_LONG).show();
    }
}

```

###### 天坑指南
- **项目根目录必须引入apply from: "config.gradle"**
- **在使用arouter的每个模块都要进行下面操作**
```Java
 defaultConfig {
        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [moduleName: project.getName()]
            }
        }
    }
```
- **在需要单独运行的每个模块要配置自己的application和资源配置文件**
```Java
sourceSets {
        main {
            if (rootProject.ext.isAloneRun.isNeedChatModule) {
                manifest.srcFile 'src/main/runalone/AndroidManifest.xml'
                java.srcDirs = ['src/main/java', 'src/main/runalone/java']
                res.srcDirs = ['src/main/res', 'src/main/runalone/res']
            } else {
                manifest.srcFile 'src/main/AndroidManifest.xml'
            }
        }
    }
```
- **Gradle 3.0后当引入公共模块后并想使用公共模块中的库，需要使用api引入。具体可以看 https://blog.csdn.net/u014649598/article/details/80938483**
```Java
// common  libray中 
dependencies {
    api rootProject.ext.dependencies["easypermissions"]

}
```

项目地址：https://github.com/songjiabin/ModularDemo
