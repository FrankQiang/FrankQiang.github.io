---
layout: post
title: defaultdict And setdefault 
category: CookPython
tags: Python
keywords: Python
---

```python

from collections import defaultdict

d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
d['b'].append(4)
print(d)
# defaultdict(<class 'list'>, {'a': [1, 2], 'b': [4]})
d = defaultdict(set)
d['a'].add(1)
d['a'].add(2)
d['b'].add(4)
print(d)
# defaultdict(<class 'set'>, {'a': {1, 2}, 'b': {4}})


defaultdict 会自动为将要访问的键(就算目前字典中并不存在这样的键)创建映射实体。 
如果你并不需要这样的特性，你可以在一个普通的字典上使用setdefault() 方法来代替。

d = {} 
d.setdefault('a', []).append(1)
d.setdefault('a', []).append(2)
d.setdefault('b', []).append(4)
print(d)
# {'a': [1, 2], 'b': [4]}

setdefault() 用起来有点别扭。因为每次调用都得创建一个新的初始值的实例(例子程序中的空列表[])。

```
