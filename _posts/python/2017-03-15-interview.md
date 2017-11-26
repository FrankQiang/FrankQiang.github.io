---
layout: post
title: Interview 
category: Python
tags: Python
keywords: Python
---

### Python 

#### Namespace 

```python

>>> a = 5
>>> globals()
{'a': 5, ...}  a 指向5这个对象 

>>> a = "test"
>>> globals()
{'a': 'test', ...}  a 指向'test'这个对象

在Python源码中, 有这样一句话: Names have no type, but objects do.

Python的名字实际上是一个字符串对象,

它和所指向的目标对象一起在名字空间中构成一项 {name: object} 关联。

这样Python就拥有了动态语言的动能

```

#### Pass A Variable 

```python

当一个参数传递给函数的时候, 相当于a (local) = a (global) 赋值操作

虽然它们不是同一个变量, 但它们任然指向同一个对象.

# immutable  For example string, tuple and number
a = 1

print(id(a))  # 10914368


def fun(a):
    print(id(a))  # 10914368
    a = 2         #　这里改变a (local) 指向的对象
    print(id(a))  # 10914400


fun(a)
print(id(a))  # 10914368

# mutable  For example list, dict and set
a = [] 
print(id(a))  # 140408380111368


def fun(a):
    print(id(a))  # 140408380111368
    a.append(1)   # 这里没有改变a (local) 指向的对象
    print(id(a))  # 140408380111368


fun(a)
print(id(a))  # 140408380111368

```

### Algorithm 

```python

```

### Mysql 

```python

```

### Linux 

```python

```

### Other 

```python

```
