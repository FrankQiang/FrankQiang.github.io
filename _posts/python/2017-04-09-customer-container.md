---
layout: post
title:  自定义容器类型 
category: Python
tags: Python
keywords: Python
---

#### 简单的类型 

对于定制简单的容器类型，自己继承 list 或 dict

```python
class T(list):
  
    def __init__(self, members):
        super().__init__(members)

    def count(self):
        counts = {} 
        for item in self:
            counts.setdefault(item, 0)
            counts[item] += 1 
        return counts

t = T(['a', 'b', 'c', 'a'])
print(t.count())

# {'b': 1, 'c': 1, 'a': 2}
```

#### 复杂的类型

继承 collections.abc 中的 Sequence 类会使事情简单些。

该类具备容器常用的方法， 只需要实现`__len__` 和 `__getitem__` 就行了。

```python
from collections.abc import Sequence

class T(Sequence):
  
    def __init__(self):
        self.a = [1, 2]

    def __len__(self):             # 必须实现 获得容器的长度            
        return len(self.a)

    def __getitem__(self, index):  # 必须实现 根据索引获得 value
        return self.a[index]

t = T()
print(len(t))
print(t[0]

# 2
# 1
```
