---
title: android通过浏览器打开app
date: 
tags: 
- android 
- 日常问题
copyright: true
categories: android
---



<blockquote class="blockquote-center">长风破浪会有时，直挂云帆济沧海</blockquote>

<!-- more -->

有个需求当从微信分享出去的连接在浏览器打开的时候需要打开默认的本地app
暂定需求是打开本地app,并不跳转到具体的某个activity地址。所以默认打开MainActivity。

```Java
    后台连接跳转的url：	hmapp://android.jeno/hmapp
   <intent-filter>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data
                    android:host="android.jeno"
                    android:path="/hmapp"
                    android:scheme="hmapp">
                </data>
            </intent-filter>
```

这样遇到一个问题。当启动app后，界面上的图标不见了。后来了解到，当添加了 data后属于隐士启动。会默认的隐藏图标。
解决办法如下：

```Java
<activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>

            </intent-filter>

            <intent-filter>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data
                    android:host="android.jeno"
                    android:path="/hmapp"
                    android:scheme="hmapp">
                </data>
            </intent-filter>


        </activity>
```

 `<category android:name="android.intent.category.BROWSABLE" />`的意思是允许浏览器打开自己。