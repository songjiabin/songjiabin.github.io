---
title: flutter io 文件的操作
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">时光逆转成红色的晨雾，昼夜逐渐平分。。</blockquote>

<!-- more -->
 
 
 ##### 引入第三方插件
 - 在`pubspec.yaml`中 加入 插件的引用 
 ```Java
 dependencies:
  flutter:
    sdk: flutter
  path_provider: ^0.4.1
 ```
 - 终端加载插件
 ```Java
 flutter packages get
 ```
 最后在项目中引入
 
 
##### 查看目录
 
###### 临时目录
 `getTemporaryDirectory`
###### 文档目录
 `getApplicationDocumentsDirectory`
###### sd卡目录
 `getExternalStorageDirectory`
 
###### 打印结果 
- 临时目录/data/data/com.jeno.flutter_github/cache
- 文档目录/data/data/com.jeno.flutter_github/app_flutter
- sd卡目录/storage/emulated/0

 
 ##### 新增文件、读取文件
 
 ```
   /**
   * 加载文件
   * async 异步
   */
  localPath() async {
    try {
      //得到临时目录
      var tempDir = await getTemporaryDirectory();
      String tempPath = tempDir.path;

      //文档目录
      var appDorDir = await getApplicationDocumentsDirectory();
      String appDocPath = appDorDir.path;

      //sd卡目录
      var sdDir = await getExternalStorageDirectory();
      String sdPaht = sdDir.path;


      print('临时目录' + tempPath);
      print('文档目录' + appDocPath);
      print('sd卡目录' + sdPaht);

      ////文件的读写操作
      Directory(appDocPath + "/我的文件1/" + "txt文件").create(
          recursive: true).then((Directory d) {
        return new File(d.path + "/" + "1.txt").create(recursive: true).then((
            File file) {
          file.writeAsString('往缓存文件中加入数据2').then((File file) {
            //当写完数据后才能读取
            file.readAsString().then((String data) {
              print('数据是:$data');
            });
          });
        });
      });
    } catch (e) {
      print(e);
    }
  }
 ```
 
 ##### 文件的删除
 ```Java
  //文件的删除操作
      Directory(appDocPath + "/我的文件1/" + "txt文件").delete(recursive: true).then((
          FileSystemEntity fileSystemEntity) {
        print('删除path' + fileSystemEntity.path);
      });
    } catch (e) {
      print(e);
    }
 ```



 