---
layout: post
title: 序列去重
category: CookPython
tags: Python
keywords: Python
---

#### 去重后不维持原本的顺序

```python

a = [1, 5, 2, 1, 9, 1, 5, 10]
print(set(a))
# {1, 2, 10, 5, 9}

```

#### 去重后维持原本的顺序， 元素值为 hashable

```python

def dedupe(items):
    seen = set()
    for item in items:
        if item not in seen:
            yield item 
            seen.add(item)


a = [1, 5, 2, 1, 9, 1, 5, 10]
print(list(dedupe(a)))
# [1, 5, 2, 9, 10]

```


#### 去重后维持原本的顺序，元素值不为 hashable

```python

def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item 
            seen.add(val)


a = [{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 1, 'y': 2}, {'x': 2, 'y': 4}]
print(list(dedupe(a, key=lambda d: (d['x'], d['y']))))
# [{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 2, 'y': 4}]

```
