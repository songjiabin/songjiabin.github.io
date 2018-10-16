---
title: Django 3-1 视图操作（创建和查询）
date: 2018-07-15
tags: 
- python
- mysql 
copyright: true
categories: python
---

<blockquote class="blockquote-center">愿每次回忆，对生活都不感到负疚</blockquote>


<!-- more -->







#### 修改数据库的结构
##### 删除数据库

```Python

# 删除 数据库 songjiabin
drop database songjiabin;

```

##### 删除生产的迁移文件迁移的文件 并新建数据库 

![360截图20180909010700320.jpg](https://upload-images.jianshu.io/upload_images/2953304-7adecc28296fda33.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```Python 
# 新建数据库
create datebase songjiabin;
```

##### 重写部署迁移文件 生产迁移文件

```Python 
python manage.py  makemigrations 
```


##### 执行迁移文件
```Python 
python manage.py migrate 
```

ok 生产新的迁移文件。可在数据库中查看表。


#### 自定义管理器
> 当你查询表中所有的数据的时候。使用```Student.objects.all() ``` 此进行查询。 objects 为里面自动生成的一个管理器。我们可以进行修改等操作。


当在模型类中定义了自定义管理器后，那么系统自动生成的objects就不存在了。


![360截图20180909150249741.jpg](https://upload-images.jianshu.io/upload_images/2953304-7934c2ae3e49208b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下次查询Student中的数据改为：
```Python 
Stuent.studentManager.all()

```

当有个需求比较要查询 isDelete =False 的所有数据
- 我们需要重写模型管理器。定义查询时当给定条件
```Python 
# 学生模型管理器

class StudentManager(models.Manager):
    #相当一重写里面的all()查询   
    def get_queryset(self):
        return super(StudentManager, self).get_queryset().filter(isDelete=False)
    pass
```

这样就可以查出 Student 里面isDelete =False 的数据了。

下面是大体的流程


- 然后开始定义项目的url 指向应用的url 。
```Python 
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # 指向具体的应用的url 
    url(r'^',include('myFristDemo.urls'))
]
```

- 在应用的url中 进行匹配../../student.html的信息。
在应用的url.py中匹配
```Python 
urlpatterns = [
    url(r'^$', view=index),

    # 匹配学生的 页面 ../../Student
    url(r'^student/$', view=student),

]
```

- 当有具体的url匹配到student.html 后。逻辑到views.py中处理。
```Python 
# 匹配学生的 页面 ../../Student
    url(r'^student/$', view=student),
```
- 在views.py中处理 student.html 文件内容。
```Python 
# 查出  学生表中 isDelete = False的数据  并交给 html文件展示并传递给流量器
def student(request):
    studentInfo = Student.studentManager.all()
    return render(request, 'myFristDemo/Students.html', {'studentInfo': studentInfo})
    pass
```
- 在处理文件内容的时候。引入model和数据库进行交互。将查出来的数据传递到student.html文件。
```HTML 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>学生信息</title>
</head>
<body>

<!--处理学生的信息  （ 学生中 isDelete = False 的数据 ）-->
{%for i in studentInfo%}
<li>
    <a href="#">{{i.sname}}</a>
</li>
{%endfor%}
</body>
</html>
```

 
 
#### 添加新的学生对象

- 给Student 中添加一个类方法。每次都调用这个类中的方法。从而完成学生对象的创建。
```Python
# 学生的模型 对应数据库中的student
class Student(models.Model):
    # 生成自定义模型管理器
    # studentManager = models.Manager()

    studentManager = StudentManager()

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

    # 更改本表后 记录所有的 最后的一次时间
    lastTime = models.DateTimeField(auto_now=True)

    # 自动生成创建时间
    createTime = models.DateTimeField(auto_created=True)

    def __str__(self):
        return self.sname

    class Meta:
        # 数据库中的表 名字是 grades
        db_table = "student"
        # 按照 id 进行升序
        # ordering =['-id'] 降序
        ordering = ['id']
        pass

    # 定义类方法 
    # 类方法。用于创建学生对象
    @classmethod
    def createStudent(cls, name, gender, age, content,
                      grade, isDelete, lastTime, createTime):
        # 新建学生 并 返回
        newStudent = cls(sname=name,
                         sgender=gender,
                         sage=age,
                         scontent=content,
                         sGrades=grade,
                         isDelete=isDelete,
                         lastTime=lastTime,
                         createTime=createTime)
        return newStudent
        pass

    pass
```
之后在 views.py中调用
```Python 

# 添加学生 方式 1
def addstudent(request):
    grade = Grades.objects.get(pk=1)

    student = Student.createStudent('基本密码',
                                    True,
                                    25,
                                    "我是基本密码",
                                    grade,
                                    False,
                                    "2018-05-25",
                                    "2018-09-25")
    student.save()


    return HttpResponse('添加成功')
    pass
```

- 在自定义的管理器中进行添加 

当创建学生对象的时候直接调用学生的中的StudentManager.createStudent()就好了。

```Python 

# 学生模型管理器
class StudentManager(models.Manager):
    def get_queryset(self):
        return super(StudentManager, self).get_queryset().filter(isDelete=False)

    # 创建学生的对象
    def createStudent(self, name, gender, age, content,
                      grade, isDelete, lastTime, createTime):
        # 返回的事当前调用的对象
        student = self.model()
        student.sname = name
        student.sgender = gender
        student.sage = age
        student.scontent = content
        student.sGrades = grade
        student.isDelete = isDelete
        student.lastTime = lastTime
        student.createTime = createTime
        return student
        pass

    pass
```
下面是views.py中的代码
```Python 
# 添加学生 方式 2  使用自定义管理器
def addstudent2(request):
    grade = Grades.objects.get(pk=1)

    student = Student.studentManager.createStudent('鸣人',
                                    True,
                                    25,
                                    "我是鸣人",
                                    grade,
                                    False,
                                    "2018-01-25",
                                    "2018-02-22")
    student.save()


    return HttpResponse('添加成功')
    pass
```

#### 查询集

##### 匹配所有的数据 没有任何的条件限制  类似于 select * from student

```Python
# 重定向url 
url(r'^allStudents/$',view=allStudents)
```
```
# Viwes.py中的代码 查询所有数据
def allStudents(request):
    allStudents = Student.studentManager.all()
    return render(request, 'myFristDemo/allStudents.html', {'allStudents': allStudents})
    pass
```
 


##### 返回表格中**满足条件的一条且只能有一条数据**

```Python
 # 匹配表中唯一的一个数据
 # 重定向url 
url(r'^getOneStudent/$',view=getOneStudent)
```

```Python
# 得到唯一的一个数据
def getOneStudent(request):
    oneStudent = Student.studentManager.get(pk=2)
    return render(request, 'myFristDemo/getOneStudent.html', {'oneStudent': oneStudent})
    pass
```

> **注意 这里只允许查到一个数据。要是查不到的话会抛异常```DoesNotExi```。相反的，如果查到的不只是一个数据，则会```报MultipleObjectsReturned``` 。注意到异常的处理**

 

##### filter
> filter():参数写查询条件，返回满足条件的数据。----->返回一个QuerySet对象

######  exact 判断等于 
```Python
url(r'^exactStudent/$',view=getExactStudent)
```

```Python
# exact 相等
def getExactStudent(request):
    # 返回的是一个集合 不是一个数据
    # 如果filter()里面没有参数 表示为全查
    exactStudent = Student.studentManager.filter(id=5)
    
    # id_exact = 5  和 id = 5  是等价的 
    exactStudent = Student.studentManager.filter(id__exact=3)

    return render(request,'myFristDemo/exactStudent.html', {'exactStudent': exactStudent})
    pass

```
###### 模糊查询 contains(包含)
查询 Student 中 sname包含 '基本'的所有信息。
```Python
# url 
# contains 查询 模糊查询 
url(r'^contains/$',view=containsStudent)
```

```Python
# 模糊查询
def containsStudent(request):
    # 查询 Student 中 sname 包含 基本 的所有数据
    containStudents = Student.studentManager.filter(sname__contains='基本')
    return render(request, 'myFristDemo/containStudents.html', {'containStudents': containStudents})
    pass
```

 

查询 以 啥开头 或者 以啥结尾的数据
```
# starwiths 以什么开头   endwiths 以什么结尾
url(r'^StarwithOrendWith/$',view=StarwithOrendWith)
```


```
def StarwithOrendWith(request):
    # 查询 Student 中 基本开头的数据
    startWithOrEndWithStudents = Student.studentManager.filter(sname__startswith='基本')
# 查询 Student 中 人结尾 的数据
    startWithOrEndWithStudents = Student.studentManager.filter(sname__endswith='人')
    return render(request, 'myFristDemo/startWithOrEndWithStudents.html', {'startWithOrEndWithStudents': startWithOrEndWithStudents})
    pass
```

 




###### 空查询
```Python 
 # 查询  某 sname 不为空的数据
    url(r'^notnullstudent/$',view=notnullstudent)
```

```Python
# 某些字段不为 null 数据
def notnullstudent(request):
    # 查询学生表中 name 不是空的所有数据
    notnullStudents = Student.studentManager.filter(sname__isnull=True)
    return render(request, 'myFristDemo/notnullstudent.html',
                  {'notnullStudents': notnullStudents})
    pass
```


###### 范围查询 in where id in (1,3,5)

```Python
# 查询  在什么范围之内的数据  
url(r'^wherestudent/$', view=wherestudent)
```
```
# 范围内的查询
def wherestudent(request):
    # 查询 id  是 1 2 3  的数据
    whereStudent = Student.studentManager.filter(id__in=[1, 2, 3])
    return render(request, 'myFristDemo/whereStudent.html',
                  {'whereStudent': whereStudent})
    pass
```
 











