---
title: MongoDB
date: 2018-08-13 14:38:35
tags: 
- python
- MongoDB 
copyright: true
categories: python
---

<blockquote class="blockquote-center">不要给自己颓废的机会</blockquote>
<!-- more -->



#### 流程

- 打开终端  定位到 MongoDB的安装目录下的bin文件夹中
- 找到你自己建立的.../data 目录
- 在终端执行：mongod.exe  --dbpth=data/bin(你自己创建的目录)。这样就启动了一个 mongoDB的服务  也就是创建了一个数据库  数据库的地址在 你的 data 目录下。
- 连接刚才创建的数据库  - 新开启一个终端 定位到 MongoDB的安装目录下的bin文件夹中。执行  mongo.exe  开启数据操作。会产生MongoDB的交互界面。



#### 操作数据库
##### 查看所有的数据库  
- show dbs 

##### 创建或者使用数据库
- use myDB 创建数据库如果没有则创建否则切换到新的数据库中去(***刚刚创建的数据库查看的话，并没有，需要向里面插入数据才可以。**)

##### 删除当前的数据库（**首先必须要是正在操作的数据库**）
- db.dropDatabase()

##### 查看当前数据库
- db或者db.getName() 查看当前操作的数据库

##### 插入数据 
- db.student.insert({name:"tom",age:18,gender:1,address:"河北衡水",isDelete:0})   操作当前的数据库，并创建一个student的集合，并向里面加入一条数据。

##### 断开连接
- exit 



#### 集合操作
##### 查看当前数据下所有的集合(就是当前数据库下有几个表)
- show collections

##### 创建集合(创建表)
- db.createCollection("class")
- db.class.insert("文档") 如果集合不存在则创建，并插入文档

##### 删除集合（删除表）
- db.class.drop() 删除名字为class的集合


#### 文档操作
##### 使用insert方式
- db.集合名.insert(文档) db.class.insert(文档) 
- db.student.insert([{name:"基本密码",age:19,address:"北京土著",gender:1},{name:"鸣人",age:20,address:"忍者村",gender:1}])

##### 使用save方式 
**如果不指定_id字段，那么save和insert是一样的。如果指明了_id字段，那么会更新指定_id的数据。**
- db.student.save({name:"Daily",age:22,gender:0,address:"加拿大",isDelete:0}) 
-  db.student.save({_id:ObjectId("5b8b8b09fdabd6efbf9b7aa2"),name:"是不是加拿大",age:88,gender:0,address:"美国",isDelete:0})




#### 文档的更新
- 将名字是 鸣人 数据 age变为44

$set 将以前变为新的

db.student.update({name:"鸣人"},{$set:{"age":44}})

$inc 在之前的基础上累加更新

db.student.update({name:"鸣人"},{$inc:{"age":44}})

{multi:true} 表示为全改   

db.student.update({name:"鸣人"},{$set:{"age":44}},{multi:true})





#### 文档的删除
在执行remove之前。先执行find()命令来判断执行条件是否存在。
删除名字为tom的字段
- db.student.remove({name:"tom"})
删除名字为tom的数据  justOne 如果有多个只删除查到的第一个
- db.student.remove({name:"tom"},{justOne:true})


#### 文档的查询
- db.student.find() 查询student表中的所有数据  

- db.student.find({name:"基本密码"}) 查询name为基本密码的数据


- db.student.find({gender:0},{age:1,name:1})  想查看gender为0的数据。并且还要数据中的age和name字段。

- db.student.find({},{age:1,name:1}) 查询所有数据。里面只包含age和name两个字段   

- db.student.findOne({gender:1})
查询所有数据中的第一条数据 



##### pretty 格式化数据显示方式  显示的是json格式
db.student.find({},{age:1,name:1}).pretty()





#### 条件操作符号

##### 大于          $gt 

年龄大于20的

```
db.student.find({age:{$gt:20}}).pretty()
```



#####   大于等于    $gte 
年龄大于等于20的
```
db.student.find({age:{$gte:20}}).pretty() 
```




#####  小于          $lt
年龄小于20的
``` 
db.student.find({age:{$lt:20}}).pretty()
```


#####   小于等于      $lte
年龄小于等于20的
```
db.student.find({age:{$lte:20}}).pretty()
```


##### 大于等于...小于等于....
年龄大于等于19 小于等于80 
```
db.student.find({age:{$gte:19,$lte:80}}).pretty() 
```



#####   等于          $:
年龄等于20 
```
db.student.find({age:20}).pretty() 
```




#####   使用_id进行查询
查询 _id是具体的某个值 的数据
```
db.student.find({_id:ObjectId("5b8b8a2cfdabd6efbf9b7aa0")}).pretty() 
```

#####   查询某个结果集的数据的条数
得到最后的条数

```
db.student.find().count()
```

#####   查询某个字段的值当中是否包含另一个值
查找name中包含 '密码' 字段的  
```
 db.student.find({name:/密码/}).pretty() 
```

#####   查询某个字段的值是否以另一个值开头
```
 db.student.find({name:/^基/})
```




#####   ADN   
查找gender为1 并且 age > 10 的数据  
```
db.student.find({gender:1,age:{$gt:10}}).pretty() 
```


#####   OR

查询age等于44 或者 age<20  的数据   
```
db.student.find({$or:[{age:44},{age:{$lt:20}}]}).pretty() 

```



#####   limit skip   
得到前面的两条数据 

```
db.student.find().limit(2).pretty() 

```

跳过两条再拿数据    

```
db.student.find().skip(2).pretty()  
```

一般分页的话 limit 和 skip 结合使用  


#####  排序  
1 升序
2 降序 

```
db.student.find().sort({age:1})

```


