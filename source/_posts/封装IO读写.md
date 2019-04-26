---
title: 封装IO读写
date: 
tags: 
- android 
- 日常问题
- 工具类
copyright: true
categories: android
---


<blockquote class="blockquote-center">竹杖芒鞋轻胜马，谁怕？一蓑烟雨任平生</blockquote>

<!-- more -->



##### 读写data/data/文件(应用本包下文件写入和读取)

```Java
public class FileHelper {


    /**
     * 在data/data/本包中写入文件:追加文件模式
     *
     * @param fileName    文件名
     * @param fileContent 文件内容
     * @param append      是否以追加模式
     */
    public static void writeInLocal(Context ctx, String fileName, String fileContent, boolean append) {
        FileOutputStream fos = null;
        try {
            fos = ctx.openFileOutput(fileName, append ? Context.MODE_APPEND : Context.MODE_PRIVATE);
            write(fos, fileContent);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            close(fos);
        }
    }


    /**
     * 在data/data/本包中读取文件
     *
     * @param fileName 文件名
     * @return 文件内容
     */
    public static String readInLocal(Context ctx, String fileName) {
        FileInputStream fis = null;
        String result = null;
        try {
            fis = ctx.openFileInput(fileName);
            result = read(fis);
            //关闭输入流
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(fis);
        }
        return result;
    }


    /**
     * 写入OutputStream
     *
     * @param os          输出流
     * @param fileContent 文本内容
     */
    private static  void write(OutputStream os, String fileContent) {

        try {
            if (fileContent != null) {
                os.write(fileContent.getBytes());
            }
            close(os);//关闭输出流
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 读取InputStream
     *
     * @param is 输入流
     * @return 流转化的字符串
     * @throws IOException IO异常
     */
    private static String read(InputStream is) throws IOException {
        byte[] temp = new byte[1024];
        int len = 0;
        StringBuilder sb = new StringBuilder("");
        while ((len = is.read(temp)) > 0) {
            sb.append(new String(temp, 0, len));
        }
        return sb.toString();
    }


    /**
     * 关闭可关闭对象
     *
     * @param io 可关闭对象
     */
    private static void close(Closeable io) {
        if (io != null) {
            try {
                io.close();
            } catch (IOException e) {
                Log.e("eee", e.getLocalizedMessage());
            }
        }
    }
}

```

使用
```Java
FileHelper.writeInLocal(this, "writeInLocal.txt", "基本密码", true);
String content = FileHelper.readInLocal(this, "writeInLocal.t
```

##### 读取assets文件夹的文件

```Java
public static void writeInAssets(Context ctx, String fileName, String fileContext) {
     OutputStream out = null;
     InputStream is = null;
     try {
         is = ctx.getAssets().open(fileName);
     } catch (IOException e) {
         e.printStackTrace();
     }
 }
```

使用
```Java
  String data = FileHelper.readInAssets(this, "FileJson.json");
```





##### sd卡读写
```Java
public class SDUtils {


    /**
     * 判断是否存在SD卡
     *
     * @return 是否存在SD卡
     */
    private boolean hasSdCard() {
        return Environment.getExternalStorageState()
                .equals(Environment.MEDIA_MOUNTED);
    }

    /**
     * 在SD卡中创建文件的核心代码
     *
     * @param savePath    保存的绝对路径(路径不存在会自动创建上级文件夹)
     * @param fileContent 文件内容
     * @param append      是否以追加模式
     */
    private File writeFileWithAbsolutePath(String savePath, String fileContent, boolean append) {
        FileOutputStream fos = null;
        File filePic = null;
        try {
            filePic = new File(savePath);
            if (!filePic.exists()) {
                filePic.getParentFile().mkdirs();
                filePic.createNewFile();
            }
            fos = append ?
                    new FileOutputStream(savePath, true) : new FileOutputStream(savePath);
            write(fos, fileContent);

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            close(fos);
        }
        return filePic;
    }

    /**
     * 在SD卡中创建文件暴露方法
     *
     * @param filename    文件名:(形式："XX/YY/ZZ.UU")
     * @param fileContent 文件内容
     * @param append      是否以追加模式
     */
    public File writeFile2SD(String filename, String fileContent, boolean append) {
        return writeFileWithAbsolutePath(PathUtils.getSDPath() + File.separator + filename, fileContent, append);
    }

    /**
     * 在SD卡中创建空文件
     *
     * @param filename 文件名
     * @return 文件对象
     */
    public File createFile(String filename) {

        return writeFile2SD(filename, "", false);
    }


    /**
     * 在SD卡中读取文件
     *
     * @param filename 文件名
     * @return 文件内容
     */
    private String readFileWithAbsolutePath(String filename) {
        String result = null;
        FileInputStream input = null;
        if (hasSdCard()) {
            try {
                input = new FileInputStream(filename);//文件输入流
                result = read(input);//读取InputStream
                close(input); //关闭输入流
            } catch (IOException e) {
                e.printStackTrace();
                Log.e(">>>", e.toString());
            } finally {
                close(input);
            }
        }
        return result;
    }

    /**
     * 在SD卡中读取文件
     *
     * @param fileName 文件名
     * @return 文件内容
     */
    public String readFromSD(String fileName) {
        return readFileWithAbsolutePath(PathUtils.getSDPath() + File.separator + fileName);
    }


    /**
     * 写入OutputStream
     *
     * @param os          输出流
     * @param fileContent 文本内容
     */
    private static void write(OutputStream os, String fileContent) {

        try {
            if (fileContent != null) {
                os.write(fileContent.getBytes());
            }
            close(os);//关闭输出流
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 读取InputStream
     *
     * @param is 输入流
     * @return 流转化的字符串
     * @throws IOException IO异常
     */
    private static String read(InputStream is) throws IOException {
        byte[] temp = new byte[1024];
        int len = 0;
        StringBuilder sb = new StringBuilder("");
        while ((len = is.read(temp)) > 0) {
            sb.append(new String(temp, 0, len));
        }
        return sb.toString();
    }


    /**
     * 关闭可关闭对象
     *
     * @param io 可关闭对象
     */
    private static void close(Closeable io) {
        if (io != null) {
            try {
                io.close();
            } catch (IOException e) {
                Log.e("eee", e.getLocalizedMessage());
            }
        }
    }


}

```
PathUtils 类
```Java
public class PathUtils {

    public static String getSDPath() {
        return Environment.getExternalStorageDirectory().getAbsolutePath();
    }
}
```
使用：
```Java
 SDUtils sdUtils = new SDUtils();

//在SD卡追加模式创建：data/writeFile2SD.txt文件，写入"toly"
sdUtils.writeFile2SD("data/writeFile2SD.txt", "toly", true);
//在SD卡上创建一个空文件
sdUtils.createFile("create/create.txt");


//读取data/writeFile2SD.txt文件
String read = sdUtils.readFromSD("data/writeFile2SD.txt");
Log.i("》》》", read);//tolytolytolytolytolytoly
```

**android 6.0以上注意动态权限**


##### 读写缓存具体实现
###### 读写缓存到包目录下边

定义接口 
```Java
/**
 * author : 宋佳
 * time   : 2019/01/31
 * desc   : 缓存策略接口
 * version: 1.0.0
 */

public interface CacheStrategy {

    /**
     * 存储缓存
     *
     * @param key   文件名
     * @param value 文件内容
     * @param time  有效时间 单位：小时
     */
    void setCache(String key, String value, long time);

    /**
     * 获取缓存
     *
     * @param key 文件名
     * @return 文件内容
     */
    String getCache(String key);

}
```

继承
```Java
/**
 * author : 宋佳
 * time   : 2019/01/31
 * desc   : 以文件保存缓存 到本包 <br/>
 * version: 1.0.0
 */

public class InnerFileStrategy extends BaseFileStrategy {


    /**
     * @param
     */
    public InnerFileStrategy(Context context) {
        //将目录定位到包名下    
        super(context.getCacheDir().getPath());
    }
}
```

###### 读写缓存到SD卡目录下边
继承
```Java
/**
 * author : 宋佳
 * time   : 2019/01/31
 * desc   :
 * version: 1.0.0
 */

public class SDFileStrategy extends BaseFileStrategy {

    /**
     * @param
     *  以文件保存缓存 到SD卡cacheData目录 <br/>
     */
    public SDFileStrategy(Context context) {
        //将目录定位到 / cacheData下面    
        super(Environment.getExternalStorageDirectory() + "/cacheData");
    }
}

```


**具体的使用如下**
```Java
//使用 包下面的路径进行缓存读写    
new InnerFileStrategy(this).setCache("基本密码", "基本密码", 1);
String content = new InnerFileStrategy(this).getCache("基本密码");
Log.i("》》》", content);

//使用SD缓存 读写
new SDFileStrategy(this).setCache("基本密码", "基本密码", 2);
String content2 = new InnerFileStrategy(this).getCache("基本密码");
Log.i("》》》", content);
```








