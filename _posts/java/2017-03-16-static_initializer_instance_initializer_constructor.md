---
layout: post
title: Interview 
category: Java
tags: Java
keywords: Java
---

### Static Initializers vs Instance Initializers vs Constructors

```java

public class Test {

    static int staticVariable;
    int instanceVariable;

    // Static initialization block:
    static {
        System.out.println("Static initialization.");
        staticVariable = 5;
    }

    // Instance initialization block:
    {
        System.out.println("Instance initialization.");
        instanceVariable = 10;
    }

    // Constructor
    public Test() {
        System.out.println("Constructor executed.");
    }

    public static void main(String[] args) {
        new Test();
        new Test();
    }
}

Static initialization.
Instance initialization.
Constructor executed.
Instance initialization.
Constructor executed.


Static initialization 加载到 JVM 后执行.
Instance initialization 每次实例化后在 constructor 之前执行.
Constructor 每次实例化都执行.

```
