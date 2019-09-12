---
title: Flutter 打包
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">习闲成懒，习懒成病。</blockquote>

<!-- more -->



##### 关于Flutter 打包的问题

######  申请.key文件
> E:\AS1\je\bin\keytool -genkey -v -keystore E:\flutter\key\key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias key

- `E:\AS1\je\bin\` 可以根据flutter doctor -v 中的java library找到

- `E:\flutter\key\key.jks` 为生成key文件的地址


##### 修改android目录下的builder文件，指定导包的key文件地址
```Java
def keystorePropertiesFile = rootProject.file("key.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
    compileSdkVersion 28

    lintOptions {
        disable 'InvalidPackage'
    }

    defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "com.jeno.flutter_github"
        minSdkVersion 16
        targetSdkVersion 28
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }

    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            //打包方式为正式   release
            signingConfig signingConfigs.release
        }
    }
}
```

##### 在android目录下面新建`key.properties`文件
```Java
storePassword=123456
keyPassword=123456
keyAlias=key
storeFile=E:\\flutter\\key\\key.jks
```
**注意：里面不用有空格，注意路径的\的使用**


##### 执行命令
```Java
flutter build apk
```

##### 查看打包文件
在   `项目\build\app\outputs\apk\release` 下面查看

项目地址：https://github.com/songjiabin/flutter
