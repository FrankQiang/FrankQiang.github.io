---
layout: post
title: namedtuple
category: CookPython
tags: Python
keywords: Python
---

```python

tuple是 immutable 同样　namedtuple 也是  immutable

namedtuple的优点是 提高tuple可读性

如果你的目标是定义一个需要更新很多实例属性的高效数据结构，

那么namedtuple并不是最佳选择。


import collections

Grade = collections.namedtuple("Grade", ("score", "weight"))

grade = Grade(89, 0.45)
print(grade)
print(grade.score)
print(grade.weight)

# Grade(score=89, weight=0.45)
# 89
# 0.45


namedtuple 提供了一个_replace()方法

它会创建一个全新的namedtuple并将对应的字段用新的值取代

grade = grade._replace(score=90)
# Grade(score=90, weight=0.45)


```
