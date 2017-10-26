---
layout: post
title: OrderedDict 
category: CookPython
tags: Python
keywords: Python
---

#### 有序字典

```python

from collections import OrderedDict

d = OrderedDict()
d["foo"] = 1
d["bar"] = 2
d["spam"] = 3
d["grok"] = 4
print(d)
# OrderedDict([('foo', 1), ('bar', 2), ('spam', 3), ('grok', 4)])

OrderedDict 内部维护着一个根据键插入顺序排序的双向链表。
每次当一个新的元素插入进来的时候， 它会被放到链表的尾部。

对于一个已经存在的键的重复赋值不会改变键的顺序

d["foo"] = 5
print(d)
# OrderedDict([('foo', 5), ('bar', 2), ('spam', 3), ('grok', 4)])


一个 OrderedDict 的大小是一个普通字典的两倍，
因为它内部维护着另外一个链表。

需要权衡一下是否使用OrderedDict 带来的好处要大过额外内存消耗的影响。

```
