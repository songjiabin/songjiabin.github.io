---
title: Django 2 创建视图、url定向操作
date: 2018-07-15
tags: 
- python
- mysql 
copyright: true
categories: python
---

 


<blockquote class="blockquote-center"> 一个不注意小事情的人，永远不会成功大事业。
</blockquote>

<!-- more -->




![](https://cdn.pixabay.com/photo/2018/08/26/14/04/aster-3632294__340.jpg)

#### 站点管理
##### 启动服务器 

> 这个服务器是python写的轻量级的服务器。仅仅在开发中使用  

```

python manage.py  runserver 

```

这样 http://127.0.0.1:8000/ 就可以访问了


> 负责添加，修改，删除内容

##### 配置Admin 应用
在../../setting.py中的 INSTALLED_APPS 加入 'django.contrib.admin' (默认是有的)

在开启服务器的情况下。

```
#创建超级用户 
python manage.py createsuperuser

在终端中依次输入  用户名 、 邮箱（随意填）、 密码(数字和英文的结合) 

```
这样就创建了你的账户和密码了。
现在就可以登录你的服务器

http://127.0.0.1:8000/admin  登录 使用你刚才创建的账户密码

![360截图20180902194525718.jpg](https://upload-images.jianshu.io/upload_images/2953304-a454cb78a53ed85c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

感觉都是英文。ok,进行汉化。

在../../setting.py中找到 
```
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'UTC'

```
修改为：

```
LANGUAGE_CODE = 'zh-Hans'
TIME_ZONE = 'Asia/Shanghai'
```

ok 重写启动服务器 就汉化了。

##### 管理数据表

其实主要的就是修改 应用/admin.py   

```Java 

from django.contrib import admin

# Register your models here.

# 引入建立的 魔板 也就是 表
from .models import Grades, Student


# 创建班级的时候默认加两个学生
class StudentInfo(admin.TabularInline):
    model = Student
    extra = 2
    pass


# 自定义 网页显示的样式
@admin.register(Grades)
class GradesAdmin(admin.ModelAdmin):
    #  创建班级的时候默认加两个学生
    inlines = [StudentInfo]

    # 列表页属性
    # 显示字段 列表中的字段 想显示啥 写啥
    list_display = ['pk', 'gname', 'gdate', 'ggirlnum', 'gboynum', 'isDelete']
    # 过滤器 过滤的字段
    list_filter = ['gname']
    # 搜索的条件
    search_fields = ['gname']
    # 添加分页 每5条分一页
    list_per_page = 5

    # 添加 修改属性 是 另一个界面
    # 规定属性的先后顺序
    # fields = ['gdate', 'ggirlnum', 'gboynum', 'isDelete', 'gname']

    # 添加分组   fields和 fieldsets 不能同时使用
    fieldsets = [
        # num 为 一组
        ('num', {"fields": ['gdate', 'ggirlnum', 'gname']}),
        # other 为 一组
        ('other', {"fields": ['gboynum', 'isDelete']})
    ]

    pass

@admin.register(Student)
class StudentAdmain(admin.ModelAdmin):

    # 修改默认内容
    def gender(self):
        if self.sgender:
            return '男'
        else:
            return '女'
        pass

    # 设置页面列的表名称
    gender.short_description = '性别'

    # 列表页属性

    # 显示字段 列表中的字段 想显示啥 写啥
    list_display = ['pk', 'sname', gender, 'sage', 'scontent', 'sGrades', 'isDelete']
    # 过滤器 过滤的字段
    list_filter = ['scontent']
    # 搜索的条件
    search_fields = ['scontent']
    # 添加分页 每5条分一页
    list_per_page = 5

    # 添加 修改属性 是 另一个界面
    # 规定属性的先后顺序
    # fields = ['gdate', 'ggirlnum', 'gboynum', 'isDelete', 'gname']

    # 添加分组   fields和 fieldsets 不能同时使用
    fieldsets = [
        # num 为 一组
        ('num', {"fields": ['sname', 'sGrades', 'sage', 'isDelete']}),
        # other 为 一组
        ('other', {"fields": ['sgender', 'scontent']})
    ]

    pass


# 注册 班级表
# admin.site.register(Grades, GradesAdmin) 可以使用装饰器来进行注册

# 注册 学生表
# admin.site.register(Student, StudentAdmain) 可以使用装饰器来进行注册


```


关于数据库中定义字段的关系关键字关系：

- ForeignKey 一对多 定义到多中 
比如:
```
# 学生属于哪个班级  用外键关联 
Grades 是班级表  Grades为班级表的名字 
sGrades = models.ForeignKey("Grades")

```
- 访问一对多
```
一的对象.模型小写(多的对象)_set
例子：
grade.sutdent.set 
```


> 但是发现数据库中表的名字是当前项目_类名。不是我们想要的。可以再model中进行修改，从而来修改表的名字。修改默认排序

![360截图20180909010013679.jpg](https://upload-images.jianshu.io/upload_images/2953304-65a71b3449a51eb8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


刷新  http://127.0.0.1:8000/admin 添加了表格信息



#### 视图的基本使用 

> 在 Django中，视图对web请求进行回应。视图就是一个python函数。在views.py中定义。 

在应用下面新建一个urls.py 用于匹配 url路径。web根据路径显示响应的视图

在项目路径下。找到urls.py将地址指向应用下的urls。具体代码如下。
```Python 
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # 当什么也不输入的时候会定位到 myFristApp这个文件下。
    url(r'^', include('myFristApp.urls'))
]

```

下面是 应用的urls.py 

```Python 
from django.conf.urls import url, include
from .views import index

urlpatterns = [

    # 当主页匹配到 什么地址也没有的时候  显示的是 index 界面
    url(r'^$', index)
]
```

定义好具体View。在下面定义好接受和返回的数据。
在应用的Views.py下面

```Python

from django.http import HttpResponse


# request 请求体
def index(request):
    return HttpResponse("我是基本密码")
    pass

```

#### 视图的基本使用

和视图同级的下面创建templates目录。里面存放模板

![360截图20180907000717765.jpg](https://upload-images.jianshu.io/upload_images/2953304-eeb126b19e6d1e55.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


有了模板的目录后。我们需要将配置模板的url地址。

找到项目的地址下的setting.py文件。
找到一个 
```
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
```
这个地址代表的就是项目的根目录。
继续找到TEMPLATES 下面的DIRS 向里面加入你的模板路径.

```Python 
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # 向下面添加你的模板路径
        'DIRS': [os.path.join(BASE_DIR,"templates")],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

#####  在新建的 templates 目录下面新建你的应用名称。应用名称下面是你的对应的html文件。


![360截图20180907210033995.jpg](https://upload-images.jianshu.io/upload_images/2953304-eb51d91ba59abb9b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#####  匹配你的url 当你想在浏览器中输入grades的时候，想要访问 Grades.html的时候。匹配你的 Grades.html

找到你应用下的urls.py文件。
如下定义
```Python 
 # 匹配班级的 页面 ../../grades  grades 是视图中
    url(r'^grades/$', view=grades),
```

ok，其实当你在浏览器输入 ../../grades的时候会通过这个路径找到 views.py下的grades类。

#####  匹配你的views 视图

```Python

# 视图： grades 班级的视图
def grades(request):
    # 去 models 中 取数据 也就是刚才创建表的那个 models中
    # 得到grades中所有的数据
    gradesList = Grades.objects.all()
    # 将数据传给 模板   模板渲染好后再传给浏览器
    return render(request, 'myFristApp/Grades.html', {'grades': gradesList})
    pass
    
```

#####  编辑你的html文件
```Python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>班级信息</title>
</head>
<body>
    <h1>班级信息列表</h1>
     <ul>
         {%for grade in grades%}
            <li>
             <a href="#">{{grade.gname}}</a>
            </li>
        {%endfor%}
     </ul>
    </body>
</html>
```


##### 当点击班级表的时候想查看班级表里面的学生信息

在Grades.html中修改点击传递id
![360截图20180907235122617.jpg](https://upload-images.jianshu.io/upload_images/2953304-0aafa11ca9781ad6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 url 中注册地址

```Python
# 匹配 ../../grades/1  当点击班级查看班级详情
url(r'^grades/(\d+)$', view=gradesStudent)
```

当点击的时候跳转到 graesStudent 的View中去
```Python
# 视图 当点开班级 查看班级详情
# num 为班级的id 根据班级的id  取出 班级内的学生信息
def gradesStudent(request, num):
    
    
    # 从班级表中找到 id=num的班级信息
    gradesInfo = Grades.objects.get(pk=num)
    # 从这个班级中得到下面所有的学生
    studentInfo = gradesInfo.student_set.all()
    return render(request, 'myFristApp/GradesStudent.html', {'studentInfo': studentInfo})
    pass
```

浏览器接收到信息后将请求的信息发动html中 如下：

```Python 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>班级下的学生信息</title>
</head>
<body>
    <h1>班级下的学生信息</h1>
        {% for i in studentInfo%}
            <li>
                <a href="#">{{i.sname}}</a>
            </li>
        {%endfor%}
</body>
</html>
```

ok 基本的流程就是这样。

代码：https://github.com/songjiabin/PyDemo

 
 






 
 
 


 
 