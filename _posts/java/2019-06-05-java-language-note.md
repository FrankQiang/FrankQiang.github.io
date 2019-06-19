---
layout: post
title: Java language note 
category: Java
tags: Java
keywords: Java
---

* 从 Java 10 开始局部变量不再需要指明类型, 编译器自动匹配类型

```java
String myVar = "A string!";

var myVar = "A string!";

var list = new ArrayList();

var myNum = new Integer(123);

var myClassObj = new MyClass();
```


* Java Array 一旦被创建, 长度不能再改变, Java List 创建后可以改变长度

```java
String[] stringArray = new String[10];

int[] intArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

* String

```java
Java 9 以前字符串每个字符在 JVM 中由两个字节表示 (由 UTF-16 编码) 

从 Java 9 开始如果字符串中都是 ISO-8859-1/Latin-1 则每个字符由一个字节表示


\r 回车 回到行首

\n 换行 切换到下一行


JVM 会维护一个 String 池, myString1 和 myString2 指向同一个对象 

String myString1 = "Hello World";
String myString2 = "Hello World";

如果想不指向同一个对象

String myString1 = new String("Hello World");
String myString2 = new String("Hello World");


String 在 Java 中是 immutable 意味着一旦创建就不能再改变

String one = "Hello";
String two = "World";
String three = one + " " + two;

实际是这样的
String three = new StringBuilder(one).append(two).toString();

所以多个字符串拼接时这样效率更高

String[] strings = new String[]{"one", "two", "three", "four", "five" };

StringBuilder temp  = new StringBuilder();
for(String string : strings) {
    temp.append(string);
}
String result = temp.toString();


replace 与 replaceAll 的区别

都可以替换所有匹配的字符串

replaceAll 能用正则匹配, replace 不能


String theString = "This is a good day to code";

byte[] bytes1 = theString.getBytes(); 用所在机器默认的编码 
byte[] bytes2 = theString.getBytes(Charset.forName("UTF-8"); 指定编码
```

* Constructor

```java
类没有构造函数, 编译器会默认创建一个无参构造函数

类有参数构造函数后, 编辑器不会创建无参数构造函数, 需要自己手动创建无参数构造函数

子类的对象实例时，默认会先调用父类的无参数的构造函数

若父类未定义无参数构造函数，则在编译阶段报错。

子类调用了父类的有参构造函数，则可以通过编译和运行。
```
