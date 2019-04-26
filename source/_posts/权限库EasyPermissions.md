---
title: 权限库EasyPermissions
date: 
tags: 
- android 
- 开源项目学习
copyright: true
categories: 开源项目学习
---



<blockquote class="blockquote-center">我们应该不虚度一生</blockquote>

<!-- more -->


> 在android 6.0之后，如果应用程序需要危险权限，则用户必须明确向应用授予该权限。而EasyPermissions这个库就是对权限的操作做了很好的封装。


当我们不适用权限库来自己操作的时候

例如：我们请求照相机的权限
请求我们的权限
```Java
 if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
            //权限判断，没有就去请求所需权限，传参 需要申请的权限(可以多个)， requestCode请求码用于结果回调里判断
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CAMERA}, PERMISSIONS_REQUEST_CODE);
        } else {
            //有权限直接拍照，6.0以下的手机拍照走这里
            takePhoto();
        }
```
请求完成后那么需要接受回调信息

```Java

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (PERMISSIONS_REQUEST_CODE == requestCode) {
            for (int x : grantResults) {
                if (x == PackageManager.PERMISSION_DENIED) {
                    //权限拒绝了
                    Log.i("》》》", "拒绝...");
                    return;
                } else if (x == PackageManager.PERMISSION_GRANTED) {
                    //成功
                }
            }
            takePhoto();
        }
    }
```
--- 
上面是使用传统的方法,那么试用EasyPermissions
- 引入
```Java
apply plugin: 'com.android.application'

android {
    compileSdkVersion 28 //需要使用28版本编译
    defaultConfig {
        applicationId "jzm.jeno.com.myapplication"
        minSdkVersion 16
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
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
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    implementation 'pub.devrel:easypermissions:2.0.0'//里面需要support：appcompat 28
}
```
- 重写 onRequestPermissionsResult
```Java
 @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        //直接将请求的结果交给Easypermissions来处理
        EasyPermissions.onRequestPermissionsResult(requestCode, permissions, grantResults, this);
    }
```
- 检查权限是否授权
```Java
就是访问你所需要授权的权限 
EasyPermissions.hasPermissions(Context context,String... perms) 用于检查权限是否授权，第二个参数可传多个权限值。
```
- 如果没有授权那么请求权限
```Java
EasyPermissions.requestPermissions(context,String rationale,int requestCode,String... perms) 用于请求权限，rationale 是提示文字，requestCode 请求码，最后是多个权限值。
```

- 注解 @AfterPermissionGranted的使用
>  EasyPermissions 库提供了权限请求的回调。所以要实现`EasyPermissions.PermissionCallbacks接口`。

- 完整代码
```Java
public class MainActivity extends AppCompatActivity implements EasyPermissions.PermissionCallbacks {


    private static final int PERMISSIONS_REQUEST_CODE = 1001;
    private static final int REQUEST_SETTINGS_CODE = 1100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        findViewById(R.id.btn_camer).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                init();
            }
        });
    }

    @AfterPermissionGranted(PERMISSIONS_REQUEST_CODE)
    private void init() {
        String[] perms = {Manifest.permission.CAMERA};
        //权限判断，第一次弹出系统的授权提示框
        if (EasyPermissions.hasPermissions(this, perms)) {
            // TODO: 2018/12/17  如果有权限那么直接在此执行代码
            takePhoto();
        } else {
            //说明没有同意你的权限，需要手动申请
            //没有权限的话，先提示，点确定后弹出系统的授权提示框
            EasyPermissions.requestPermissions(this, "拍照过程需要用到相机权限",
                    PERMISSIONS_REQUEST_CODE, perms);

            //也可以进行自定义
//            EasyPermissions.requestPermissions(
//                    new PermissionRequest.Builder(this, PERMISSIONS_REQUEST_CODE, perms)
//                            .setRationale("拍照过程需要用到相机权限")
//                            .setPositiveButtonText("确定")
//                            .setNegativeButtonText("拒绝")
//
//                            .build());

        }
    }


    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        //直接将请求的结果交给Easypermissions来处理
        EasyPermissions.onRequestPermissionsResult(requestCode, permissions, grantResults, this);
    }


    @Override
    public void onPermissionsGranted(int requestCode, @NonNull List<String> perms) {
        //同意了 权限
        takePhoto();
    }

    @Override
    public void onPermissionsDenied(int requestCode, @NonNull List<String> perms) {
        //拒绝了权限
        if (EasyPermissions.somePermissionPermanentlyDenied(this, perms)) {
            //在权限弹窗中，用户勾选了'不在提示'且拒绝权限的情况触发，可以进行相关提示。
            Log.i("》》》", "不再提示...拒绝了...");

            // 用户勾选了不再提醒，引导用户进入设置界面进行开启权限

            Intent intent = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
            Uri uri = Uri.fromParts("package", getPackageName(), null);
            intent.setData(uri);
            startActivityForResult(intent, REQUEST_SETTINGS_CODE);


        } else {
            //拒绝了
            Log.i("》》》", "拒绝了...");
        }
    }


    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == REQUEST_SETTINGS_CODE) {
            Toast.makeText(this, "再次判断是否同意了权限，再进行自定义处理",
                    Toast.LENGTH_LONG).show();
        }


    }

    private void takePhoto() {
        //拍照
        Log.i("》》》", "拍照啦...");
    }
}
```
源码解析看：https://juejin.im/post/5bf4b776518825170d1ab40b







