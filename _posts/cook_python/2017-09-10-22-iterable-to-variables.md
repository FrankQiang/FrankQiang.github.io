---
layout: post
title: Iterable to Variables 
category: CookPython
tags: Python
keywords: Python
---

#### Read 

```python

任何可迭代对象都可以通过赋值语句给多个变量赋值。 

唯一的前提就是变量的数量必须跟可迭代对象元素的数量是一样的。

t = (1, 2)
x, y = t
print(x)
print(y)

# 1
# 2

l = [3, 4]
i, j = l
print(i)
print(j)

# 3
# 4

```
