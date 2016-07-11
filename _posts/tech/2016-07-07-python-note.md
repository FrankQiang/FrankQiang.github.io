---
layout: post
title: Pythons 笔记
category: 技术
tags: Python3
keywords: Python3基础
---

这里记录我学习Python3。

## 1.实现杨辉三角 Generator

```CPP
# 杨辉三角

          1
        1   1
      1   2   1
    1   3   3   1
  1   4   6   4   1
1   5   10  10  5   1

```

```CPP
# 实现代码

def triangles():
    a = [1]
    while True:
        yield a
        a = [sum(i) for i in zip([0] + a, a + [0])]

# zip 函数
# a = [1, 2]  b = [4, 5]
# zip(a,b) [(1, 4), (2, 5)]
```

字符串首字母大写函数 capitalize()

#### 生成素数

```CPP
def odd_iter():
    n = 1
    while True:
        n += 2
        yield n


def is_dividable(n):
    return lambda x: x % n > 0


def primes():
    yield 2
    it = odd_iter()
    while True:
        n = next(it)
        yield n
        it = filter(is_dividable(n), it)
```
#### 内置排序

```CPP
sort  在原列表排序
sorted 产生新的排序列表
