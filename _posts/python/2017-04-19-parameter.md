---
layout: post
title:  参数 
category: Python
tags: Python
keywords: Python
---

它们的顺序必须是

* 位置参数 > 默认参数 > 可变参数 > 关键字参数

#### 位置参数

```python
def t(name):    # name 就是一个位置参数
    print(name)
```

#### 默认参数

```python
def t(name, age=18):  # age 就是一个默认参数
    print(name, age)

t("John")     # John 18
t("Tom", 16)  # Tom 16
```

#### 默认参数最好用不可变类型

```python

# 在每个人加一个成绩

def t(name, scores=[]):
    scores.append(90)
    print(name, scores)

t("John", [80])
t("Tom", [76])

t("Marsh")
t("Jim")

# John [80, 90]
# Tom [76, 90]
# Marsh [90]
# Jim [90, 90]  # 出现错误
```

由于 scores 参数的默认值只会在模块加载时计算一次，
所以凡是以默认形式来调用 t 函数的代码，都将共享同一份数组。
这会引发非常奇怪的行为。

```python
def t(name, scores=None):
    if scores is None:
        scores = [90]
    else:
        scores.append(90)
    print(name, scores)

t("John", [80])
t("Tom", [76])

t("Marsh")
t("Jim")

# John [80, 90]
# Tom [76, 90]
# Marsh [90]
# Jim [90]
```
 

#### 可变参数

```python
def t(name, age=18, *args):  # 把 *args 称为可变参数
    print(name, age)         # 会把 78 83 95 转成一个 tuple (元组)
    for score in args:       # 所以当参数特别多时，会耗尽内存，导致崩溃
        print(score)

t("John", 18, 78, 83)        # 这里必须写 age，不然会被后面的值覆盖

或者

scores = [78, 83]
t("John", 18, *scores)

# John 18
# 78
# 83
```


#### 关键字参数

```python
def t(name, age=18, *args, **kw):
    print(name, age)
    for score in args:
        print(score)
    for key, value in kw.items():
        print(key, value)


t("John", 18, 78, 83, gender="male")

或者

others = {"gender": "male"}
t("John", 18, 78, 83, **others)

又或者

scores = [78, 83]
others = {"gender": "male"}
t("John", 18, *scores, **others)

# John 18
# 78
# 83
# gender male
```
