---
title: Django 1 部署、基本操作
date: 2018-07-13
tags: 
- python
- mysql 
copyright: true
categories: python
---

<blockquote class="blockquote-center"> 骐骥一跃，不能十步；驽马十驾，功在不舍；锲而舍之，朽木不折；锲而不舍，金石可镂。</blockquote>


<!-- more -->





#### 流程
##### 创建一个目录定位到下面。

```Java 
django-admin startproject djangoProject   

```

##### 查看目录下的结构

```Java
tree . /f 
```
![结构目录](https://upload-images.jianshu.io/upload_images/2953304-e6b74054e659434f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 配置数据库
```Java
../../..setting.py 
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
由于setting.py文件中默认配置的数据库是sqlite3。所有要进行修改配置的数据库。
##### 配置mysql
###### 在__init__文件中写入
```Java
import pymysql
pymysql.install_as_MySQLdb()
```
###### 修改setting.py
 
```Java 
../../..setting.py 
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'songjiab',  # songjiab 数据库的名字
        'USER': 'root',  # 数据库的用户名
        'PASSWORD': '',  # 密码
        'HOST': 'localhost',  # 主机
        'PORT': '3306'  # 端口号

    }
}
```

##### 创建应用
> 每个项目可以创建多个应用，每个应用进行一种业务处理。

###### 进入刚才创建的project目录下。
###### 开启命令
```
# 创建应用 第一个应用 名字是myFristApp 
python manage.py startapp myFristApp 

```
![创建的应用目录结构](https://upload-images.jianshu.io/upload_images/2953304-1350ebf4142a01f7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 将此应用激活
将此应用加入到../../setting中的 INSTALLED_APPS 中去。
```Java 
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myFirstApp'# 加入的应用  
]
```


###### 定义模型 
数据库中的一张表对应其中的一个模型

其实模型就是一个类。类里面的属性对应数据库中的字段

在新建的应用下面找到models.py 并操作如下


**注意：模型不需要生成主键。会自动生成，并且是递增的**

```Java 
from django.db import models


# Create your models here.

# 班级的模型 对应数据库中的 grades 表
class Grades(models.Model):
    # 定义字段 gname 班级名称
    # CharField 字符串类型的
    # max_length 最大长度是20
    gname = models.CharField(max_length=20)

    # 班级成立时间
    # DateTimeField 时间类型
    gdate = models.DateTimeField()

    # 班级的女生数量
    # IntegerField数字类型
    ggirlnum = models.IntegerField()

    # 班级的男生数量
    # IntegerField数字类型
    gboynum = models.IntegerField()

    # 是否删除
    isDelete = models.BooleanField()


# 学生的模型 对应数据库中的student
class Student(models.Model):
    # 学生的姓名
    sname = models.CharField(max_length=20)
    # 学生的性别
    sgender = models.BooleanField()
    # 学生的年龄
    sage = models.IntegerField()
    # 学生的简介
    scontent = models.CharField(max_length=20)
    # 学生属于哪个班级  用外键关联
    sGrades = models.ForeignKey("Grades")
    # 是否删除
    isDelete = models.BooleanField(default=False)

    pass

```

###### 在数据库中生产数据表 
- 生成迁移文件

定位到刚开始的目录下。开启终端

```Java 
python manage.py  makemigrations 

```
执行完后会在你新建的应用下面的migrations文件夹中生成0001_initial.py 文件。
其实就是创建数据库的文件。但是数据库中还没有数据表。需要进行执行迁移文件。



- 执行迁移 

在刚刚执行目录下面

```Java
python manage.py migrate 

```
这样你再查看数据下面的目录。就可以看到生成了很多的表。



#####  测试数据操作 

###### 查看所有数据
- 进入到 python shell

```Java
python manage.py shell 

```

- 进入终端中进行执行
```Java 
from myFristApp.models import Grades,Student 
from django.utils import timezone 
from datetime import * 

# 查看 Grades 中所有的数据
Grades.objects.all()   
# 查看 Student 中所有数据
Student.objects.all()   
```


###### 添加数据
> 添加数据其实就是添加模型的实力

```Java 
grades1 = Grades()
# 班级名
grades1.gname = '班级1'
# 创建班级的日期
grades1.gdate = datetime(year=2017,month=1,day=17)
# 女生的个数
grades1.ggirlnum= 10
# 男生个数
grades1.gboynum=39
# 是否删除
grades1.isDelete=False
# 存到数据库中
grades1.save()
```
这样数据库中就有了这条数据  


添加就有外键的数据

```Java
student1 = Student()
student1.sname = '鸣人'
student1.sgender = True
student1.sage = 19
student1.scontent = '我是一个好学生。我的理想是当火影！'
# 表示这么学生的班级是 grades1 外键！
student1.sGrades = grades1
student1.save()
```




###### 查询某个数据
```Java 
# 查询id 是 1 的数据 
Grades.objects.get(pk=1)  
```

###### 修改某个数据

```Java 
grades1.gname='基本密码'
grades1.save()

```


###### 删除数据
- 物理删除
```
grades1.delete()
```

###### 一对多  当一个班级中对应多个学生 
对象名.关联的列名小写_set.all()
```
# 查询grades班级中有多少个学生  student（要小写的）
grades.student_set.all()
```
###### 在grades1的班级下，创建一个名字佐助的学生。

```Java 
 不用.save()即可保存  
student2 = grades1.student_set.create(sname=u"佐助",sgender=True,sage=18,scontent=u'我是佐助，你们好')
```







 
 












