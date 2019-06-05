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
