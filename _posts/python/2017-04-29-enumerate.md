---
layout: post
title:  关于enumerate 
category: Python
tags: Python
keywords: Python
---

```python
    a = ['a', 'b', 'c'] 
  
    for x in a:
        print(x)
```

#### 用enumerate获取索引

```python
    a = ['a', 'b', 'c'] 
    for index, value in enumerate(a, 1):
        print(index, value)
```

enumerate 提供第二个参数，以指定开始计数时所用的值（默认为 0）。
