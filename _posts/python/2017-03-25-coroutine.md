---
layout: post
title: coroutine 
category: Python
tags: Python
keywords: Python
---

#### 协程 

```python
def consumer():
    result = 9
    while True:
        result = yield result
c = consumer()
print(c.send(None))           # 启动生成器
print(c.send(1))              # 传值给生成器
print(c.send(2))
c.close()                     # 结束生成器

# 9
# 1
# 2
```
