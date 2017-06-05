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

    # 1 a
    # 2 b
    # 3 c
```

enumerate 提供第二个参数，确定索引的起始值（默认为 0）。
