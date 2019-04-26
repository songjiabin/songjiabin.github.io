---
title: Dart入门
date: 
tags: 
- flutter 
- dart 
copyright: true
categories: flutter
---



<blockquote class="blockquote-center">
人由自我限制而生发的对他人的狭窄念头，毫发无损于对方，只使自己捉襟见肘。 若能置身事外，才不会画地为牢。</blockquote>

<!-- more -->

Dart 线上运行环境
https://dartpad.dartlang.org/


##### ?. 
> 运算符在左边为null的情况下会阻断右边的调用。

##### hello world
```Java
  printNumber(num aNumber) {
          print('The number is  $aNumber'); // Print to console.
}
```

##### 没有初始化的变量都会被赋予默认值 null
```Java
  var name = 'Bob';
  var unInitializeValue1;   //未给初值的变量，默认值为 null
  Int unInitializeValue2;   //即使是Int型，默认值也是 null
```

#####  num
> int 取值范围：-2^53 to 2^53
- String -> int

```Java
var one = int.parse('1');
```
-  String -> double
```Java
var onePointOne = double.parse('1.1');
```
##### assert
> 断言函数 ， 在开发过程中， 除非条件为真，否则会引发异常。(断言失败则程序立刻终止)

> assert 是Dart语言里的的断言函数，仅在Debug模式下有效。
在开发过程中， 除非条件为真，否则会引发异常。(断言失败则程序立刻终止)。
```Java
var a ;
assert(a==null);
print(a);
```

##### Final 和 Const的用法
>   如果您从未打算更改一个变量，那么使用 final 或 const，不是var，也不是一个类型。
一个 final 变量只能被设置一次;const 变量是一个编译时常量。(Const变量是隐式的final。)
final的顶级或类变量在第一次使用时被初始化。
- 被final或者const修饰的变量，变量类型可以省略。

```Java
//可以省略String这个类型声明
final name1 = "张三";
//final String name1  = "张三";
    
const name2 = "李四";
//const String name2 = "李四";
```
- 被 final 或 const 修饰的变量无法再去修改其值。
```Java
 final name1 = "张三";
 // 这样写，编译器提示：a final variable, can only be set once
 // 一个final变量，只能被设置一次。
 //name1 = "zhangsan";
 const name2 = "李四";
 // 这样写，编译器提示：Constant variables can't be assigned a value
 // const常量不能赋值
 // name2 = "lisi";
```
- 常量的运算
```Java
const speed = 100; //速度（km/h）
const double distance = 2.5 * speed; // 距离 = 速度* 时间
final speed2 = 100; //速度（km/h）
final double distance2 = 2.5 * speed2; // 距离 =速度 * 时间
```

- const关键字不只是声明常数变量。您也可以使用它来创建常量值，以及声明创建常量值的构造函数。 任何变量都可以有一个常量值。
```Java
 // 注意: [] 创建的是一个空的list集合
  // const []创建一个空的、不可变的列表（EIL）。
  var varList = const []; // varList 当前是一个EIL
  final finalList = const []; // finalList一直是EIL
  const constList = const []; // constList 是一个编译时常量的EIL

  // 可以更改非final,非const变量的值
  // 即使它曾经具有const值
  varList = ["haha"];

  // 不能更改final变量或const变量的值
  // 这样写，编译器提示：a final variable, can only be set once
  // finalList = ["haha"];
  // 这样写，编译器提示：Constant variables can't be assigned a value  
  // constList = ["haha"];
```

##### 特殊数据类型
Dart支持以下特殊类型：

> numbers 数字

> strings 字符串

> booleans 布尔

> lists list集合（也称为数组）

> maps map集合

> runes 字符（用于在字符串中表示Unicode字符）

- （一）num数字类型
> num是数字类型的父类，有两个子类 int 和 double。
num类型包括基本的运算符，如+,-,/和*，位运算符，如>>，在int类中定义。如果num和它的子类没有你要找的东西，math库可能会找到。比如你会发现abs(),ceil()和floor()等方法。

###### （1）int类型
> int表示整数，int默认是64位二进制补码整数，int的取值不大于64位，具体取决于平台。编译为JavaScript时，整数仅限于valus，可以用双精度浮点值精确表示。可用的整数值包括-253和253之间的所有整数，以及一些幅度较大的整数。这包括一些大于2^63的整数。 因此，在编译为JavaScript的Dart VM和Dart代码之间，int类中的运算符和方法的行为有时会有所不同。例如，当编译为JavaScript时，按位运算符将其操作数截断为32位整数。
```Java
 int intNum1 = 10 ;
  print(intNum1);//结果是10
  int intNum2 = 0xDEADBEEF ;
  print(intNum2);//结果是3735928559
```

###### （2）double类型
> Dart的double是IEEE 754标准中规定的64位浮点数。double的最大值是：1.7976931348623157e+308，double类里面有一个常量maxFinite，我们通过语句print(double. maxFinite)可以得到double的最大值。
如果一个数字包含一个小数，那么它就是一个double类型。示例如下：

 ```Java
double doubleNum1 = 1.1;
print(doubleNum1); //结果是1.1
double doubleNum2 = 1.42e5;
print(doubleNum2); //结果是142000.0
```
###### (3)Dart2.1里面新增特性，当double的值为int值时，int自动转成double。
```Java
double test = 12;//打印结果是12.0
```
###### (4) Dart2.1,int也有api转成double。
```Java
 int test = 10;
   print(test.toDouble()); // 结果是： 10.0
```
###### Dart2.1,double也有api转成int,会把小数点后面的全部去掉。
```Java
double test2 = 15.1;
double test3 = 15.1234;
print(test2.toInt());// 结果是15
print(test3.toInt());// 结果是15
```

###### String字符串类型
- 使用单引号或双引号来创建一个字符串。
```Java
String str1 = '单引号基本使用demo.';
String str2 = "双引号基本使用demo.";
```
- 单引号或者双引号里面嵌套使用引号。
> 单引号里面嵌套单引号，或者//双引号里面嵌套双引号，必须在前面加反斜杠。
```Java
// 单引号里面有单引号，必须在前面加反斜杠
String str3 = '单引号里面有单引号it\'s，必须在前面加反斜杠.';
//双引号里面嵌套单引号（正常使用）
String str4 = "双引号里面有单引号it's.";
//单引号里面嵌套双引号（正常使用）
String str5 = '单引号里面有双引号，"hello world"';
//双引号里面嵌套双引号，必须在前面加反斜杠
String str6 = "双引号里面有双引号，\"hello world\"";

print(str3);// 双引号里面有单引号it's，必须在前面加反斜杠
print(str4);// 双引号里面有单引号it's.
print(str5);// 单引号里面有双引号，hello world"
print(str6);//双引号里面有双引号，"hello world"
```
- 关于转义符号的使用
> 声明raw字符串（前缀为r），在字符串前加字符r，或者在\前面再加一个\，
可以避免“\”的转义作用，在正则表达式里特别有用。
```Java
print(r"换行符：\n"); //这个结果是 换行符：\n
print("换行符：\\n"); //这个结果是 换行符：\n
print("换行符：\n");  //这个结果是 换行符：
```
- $ 使用
```Java
String replaceStr = 'Android Studio';
assert('你知道' +
        '${replaceStr.toUpperCase()}'
          + '最新版本是多少吗？' ==
          '你知道ANDROID STUDIO最新版本是多少吗？'); 
```
注：==操作符测试两个对象是否相等。assert是断言，如果条件为true，继续进行，否则抛出异常，中端操作。

##### bool布尔类型
```Java
// 检查是否为空字符串
  var emptyStr = '';
  assert(emptyStr.isEmpty);

  // 检查是否小于等于0
  var numberStr = 0;
  assert(numberStr <= 0);  

  // 检查是否为null
  var nullStr;
  assert(nullStr == null);

  // 检查是否为NaN
  var value = 0 / 0;
  assert(value.isNaN);
```
##### list集合，也成为数组
> 在Dart中，数组是List对象，因此大多数人只是将它们称为List。

- int类型list
```Java
 List list=[1,2,3];
 print(list);
```
- 要创建一个编译时常量const的list，示例如下：
```Java
List constantList = const[10,3,15];
print(constantList);// 输出结果  [10, 3, 15]
```
- add 添加
- remove 移除
- insert(0,4) 在索引为0的地方插入数字5
- indexOf(10) 查找值为10所对应的索引
- contain(4) 是否包含5 

##### map集合
- 直接声明，用{}表示，里面写key和value，每组键值对中间用逗号隔开。

```Java
Map companys = {'first': '阿里巴巴', 'second': '腾讯', 'fifth': '百度'};
print(companys);//打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度}
```
- 先声明，再去赋值。
```Java
 Map companys1 = new Map();
 companys1['first'] = '阿里巴巴';
 companys1['second'] = '腾讯';
 companys1['fifth'] = '百度';
 print(companys1);
 //打印结果 {first: 阿里巴巴, second: 腾讯, fifth:百度}
```


- 要创建一个编译时常量const的map，请在map文字之前添加const
```Java
final fruitConstantMap = const {2: 'apple',10: 'orange',18: 'banana'};
//打印结果：{2: apple, 10: orange, 18: banana}
```
- 添加元素。格式: 变量名[key] = value,其中key可以是不同类型。
```Java
//添加一个新的元素，key为“5”，value为“华为”
  companys[5] = '华为';
  print(companys);//打印结果 {first: 阿里巴巴, second: 腾讯, fifth: 百度, 5: 华为}
```

- 修改元素。格式：变量名[key] = value
```Java
  companys['first'] = 'alibaba';
  print(companys);//打印结果 {first: alibaba, second: 腾讯, fifth: 百度, 5: 华为}
```
- 查询元素
```Java
  bool mapKey = companys.containsKey('second');
  bool mapValue = companys.containsValue('百度');
  print(mapKey); //结果为：true
  print(mapValue); //结果为：true
```

- 删除元素.可以使用map的remove或者clear方法。
```Java
companys.remove('first');// 移除key为“first”的元素。
  print(companys);// 打印结果{second: 腾讯, fifth: 百度, 5: 华为}

  companys.clear();// 清空map集合的数据。
  print(companys);// 打印结果{}
```
- **关于map集合的小结：**
 > 1.创建map有两种方式。
2.map的key类型不一致也不会报错。
3.添加元素的时候，会按照你添加元素的顺序逐个加入到map里面，哪怕你的key不连续。
比如key分别是 1,2,4，看起来有间隔，事实上添加到map的时候{1:value,2:value,4:value} 这种形式。
4.添加的元素的key如果是map里面某个key的英文，照样可以添加到map里面，
比如可以为3和key为three可以同时存在。
5.map里面的key不能相同，但是value可以相同,value可以为空字符串或者为null。



#####  `?.`像`.`一样，但最左边的操作数可以为空。
> 其实和.差不多，只不过左边可以为null，要是左边为null的话，整个的为null
```Java
 // If p is non-null, set its y value to 4.
 p?.y = 4;
```



##### ..级联符号..
> 级联符号..允许您在同一个对象上进行一系列操作。 除了函数调用之外，还可以访问同一对象上的字段。其实相当于java的链式调用。

```Java
String s = (new StringBuffer()
        ..write('test1 ')
        ..write('test2 ')
        ..write('test3 ')
        ..write('test4 ')
        ..write('test5'))
      .toString();
print(s); // test1 test2 test3 test4 test5
```
##### ?? 三目运算符的一种形式。
> expr1 ?? expr2 表示如果expr1非空，则返回其值; 否则返回expr2的值。


##### ~/ 除法，返回一个整数结果，其实就是取商。
> 小学都学过：被除数 ÷ 除数 = 商 ... 余数，在Dart里面A ~/ B = C，这个C就是商，这个语句相当于Java里面的A / B = C。Dart与java不同的是，Dart里面如果使用A / B = D语句，这个结果计算出来的是真实的结果。示例如下：

```Java
 var result1 = 15/7;
  print(result1); // 结果是：2.142857...
  var result2 = 15~/7;
  print(result2); // 结果是：2
```
> 顺便提一下取模操作，在Dart里面A % B = E，这个E就是余数，%符号表示取模，例如：

```Java
 var result3 = 15%7;
  print(result3); // 结果是：1
```

###### as、is与is!
```Java
class Test {
  static int funs = 5;

  Test() {
    print('构造函数 Test');
  }
  static fun() {
    print('Test fun函数');
  }
}

class Test2 extends Test {
  Test2() {
    print('构造函数 Test2');
  }
  void fun() {
    print('Test2 fun函数');
  }
}

void main(){
  print(test2 is Test);  // true
  print(test is! Test2);  // true

  (test2 as Test2).fun();  // Test2 fun函数
  // 相当于
  // if (test2 is Test) {
  //   test2.fun();
  // }
```


