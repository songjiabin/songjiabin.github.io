---
title: Django 3-2 视图操作（创建和查询）
date: 2018-07-15
tags: 
- python
- mysql 
copyright: true
categories: python
---


<blockquote class="blockquote-center">愿每次回忆，对生活都不感到负疚</blockquote>


<!-- more -->




####  比较 
- **gt         大于**
- **gte        大于等于**
- **lt         小于**
- **lte        小于等于**
 

查询 id>=3的数据
```Python
# 查询   比较 > < =
url(r'^comparestudent/$', view=comparestudent)
```

```Python
# 比较 > = <
# gt、gte、lt、lte：大于、大于等于、小于、小于等于
def comparestudent(request):
    # 查询 id > = 3数据
    compareStudent = Student.studentManager.filter(id__gte=3)
    return render(request,'myFristDemo/compareStudent.html',
                  {'compareStudent': compareStudent})
    pass
```



####  日期查询
> year、month、day、week_day、hour、minute、second：对日期时间类型的属性进行运算。

查询年份是2010年的人

```Python

def datestudent(request):
    # 查询  年 是 2010 年的数据 (createTime字段中年 是 2010 年的数据)
    dateStudent = Student.studentManager.filter(createTime__year=2010)
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass
```

查询  2010 10.1  后的数据

```Python

def afterDatestudent(request):
    # 查询 2010 10.1 后的数据
    # filter(bpub_date__gt = date(1980,1,1))
    dateStudent = Student.studentManager.filter(createTime__gt=date(2010, 10, 1))
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass
    
```


查询时间范围内的数据
```
def rangeDatestudent(request):
    # 查询 2010 10.1 后的数据
    # filter(bpub_date__gt = date(1980,1,1))
    rangeStudent = Student.studentManager.filter(createTime__range=(date(2010,9,24),date(2018,2,23)))
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': rangeStudent})
    pass
```





####  exclude 返回不满足条件的数据。相当于sql语句中where部分的not关键字

```Python
def excludeStudent(request):
    # 查询 不包含 id = 2  的数据 
    rangeStudent = Student.studentManager.exclude(id=2)
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': rangeStudent})
    pass
```


####  F对象
> 作用：用于类属性之间的比较条件。

例子：

```Python
# 引入 
from django.db.models import F
# 查询 学生信息中 age>id 的数据
# 每个学生中 age > id 的数据
dateStudent = Student.studentManager.filter(sage__gt=F('id'))
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
```

 



####  Q对象
> 用于查询时的逻辑条件。not and or，可以对Q对象进行&|~操作。

例子：

id > 4 并且 age >10 的数据

```Python 
# 查询 Student 中 age> 10 且 id> 4的数据


from django.db.models import Q
def QStudent(request):
    dateStudent = Student.studentManager.filter((Q(id__gt = 4)&Q(sage__gt = 10)))
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass
    
~ 去反的意思
~((Q(id__gt = 4)&Q(sage__gt = 10)))
```



####  order_by
> 排序,
进行查询结果进行排序。
**默认为升序**
- 升序

```Python

# 排序  查询 Student 的信息 ，并按照 id 从小到大排序
def orderByStudent(request):
    dateStudent = Student.studentManager.all().order_by("id")
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass

```


- 降序  -id 

```Python 

# 排序  查询 Student 的信息 ，并按照 id 从大到小排序
def orderByStudent(request):
    dateStudent = Student.studentManager.all().order_by("-id")
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass

```




```Python 
# 排序  查询 Student 的信息 ，并按照 id 从小到大排序
def orderByStudent(request):
  
    # 将 id > 3 的数据 并。从大到小排序

    dateStudent = Student.studentManager.filter(id__gt=3).order_by("-id")
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass

```




####  聚合函数
> 对查询结果进行聚合操作。----->**返回一个字典**

返回的结果是 字段名字__聚合的名称 
如：查询id的个数  
```{'id__count':5}```




###### aggregate 调用此进行聚合  

例子：

查询所有数据的个数

```Python
# 查询所有学生的个数
def countStudent(request):
    dateStudent = Student.studentManager.all().aggregate(Count("id"))
    print(dateStudent)
    return HttpResponse(dateStudent.get('id__count'))
    pass

返回的结果是字典 ： {'id__count':5}
```

查询学生年龄的总和

```Python
# 查询所有学生年龄的总和 
def sumStudent(request):
    dateStudent = Student.studentManager.all().aggregate(Sum("sage"))
    print(dateStudent)
    return HttpResponse(dateStudent.get('sage__sum'))
    pass
````



###### count
> 数量
统计满足条件数据的总数目。----->**返回一个值**
```Python 

def count2Student(request):
    dateStudent = Student.studentManager.all().count()
    print(dateStudent)
    return HttpResponse(str(dateStudent))
    pass
    
```




###### exists
> 判断查询集中是否有数据，如果有则返回True，没有则返回False。

```Python 
# 是否查询的有数据呢
def existsStudent(request):
    dateStudent = Student.studentManager.all().exists()
    print(dateStudent)
    return HttpResponse(dateStudent)
    pass
```



###### 一对多查询 关联查询

查询  学生中名字是'基本密码'所在班级的信息
```
# 查询学生表中  名字 为 基本密码 的学生 班级信息
def getGradesOfNames(request):
    gradesInfo  = Grades.objects.filter(student__sname__contains= ('基本密码'))
    return render(request, 'myFristDemo/Grades.html', {'gradesInfo': gradesInfo})
    pass
```


查询 班级名字 是 '班级1' 中的所有学生的信息  

```
# 查询 班级表中名字 为'班级1'的班级中所有的学生信息
def getGradesOfAllStudent(request):
    dateStudent= Student.studentManager.filter(sGrades__gname='班级1')
    return render(request, 'myFristDemo/dateStudent.html',
                  {'dateStudent': dateStudent})
    pass
```
