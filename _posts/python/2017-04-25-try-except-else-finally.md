---
layout: post
title:  关于try/except/else/finally 
category: Python
tags: Python
keywords: Python
---

#### try/except/else/finally 

```python
    try:
        3/0                             # 处理可能有异常的模块 
    except ZeroDivisionError as e:
        print("Zero Division")          # 有异常后执行
    else:
        print("Infinity")               # try模块没有异常后执行
    finally:
        print("Always runs after try")  # 总是执行
```
