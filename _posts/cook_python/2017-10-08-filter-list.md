---
layout: post
title: 列表推导
category: CookPython
tags: Python
keywords: Python
---

```python

列表推导

mylist = [1, 4, -5, 10, -7, 2, 3, -1]
print([n for n in mylist if n > 0])
# [1, 4, 10, 2, 3]


生成器列表推导

pos = (n for n in mylist if n > 0) 
print(list(pos))
# [1, 4, 10, 2, 3]


过滤规则比较复杂时

values = ["1", "2", "-3", "-", "4", "N/A", "5"] 


def is_int(val):
    try: 
        x = int(val)
        print(x)
        return True 
    except ValueError:
        return False


ivals = list(filter(is_int, values))
print(ivals)
# ["1", "2", "-3", "4", "5"]


过滤后进行数据处理

print([n*n for n in mylist if n > 0])
# [1, 16, 100, 4, 9]

print([n if n > 0 else 0 for n in mylist])
# [1, 4, 0, 10, 0, 2, 3, 0]

```


```python

compress()函数会根据序列去选择输出对应位置为 True 的元素。

from itertools import compress

addresses = [
    "5412 NCLARK",
    "5148 NCLARK",
    "5800 E58TH",
    "2122 NCLARK"
    "5645 NRAVENSWOOD",
    "1060 WADDISON",
    "4801 NBROADWAY",
    "1039 WGRANVILLE",
]

counts = [0, 3, 10, 4, 1, 7, 6, 1]
more5 = [n > 5 for n in counts]
print(more5)
# [False, False, True, False, False, True, True, False]
print(list(compress(addresses, more5)))
# ['5800 E58TH', '4801 NBROADWAY', '1039 WGRANVILLE']

```
