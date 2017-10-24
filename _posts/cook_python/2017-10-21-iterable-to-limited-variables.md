---
layout: post
title: head, *tail = [1, 10, 7] 
category: CookPython
tags: Python
keywords: Python
---

```python

任何可迭代对象都可以通过赋值语句给多个变量赋值。 

但是一个可迭代对象的元素个数超过变量个数时，会抛出 ValueError 。

可是我们又不想写出很多变量和可迭代对象的元素一一对应。

items =[1, 10, 7, 4,5, 9]
head, *tail =items

print(head)
print(tail)

# 1
# [10, 7, 4,5, 9]

```
