---
layout: post
title:  关于生成器 
category: Python
tags: Python
keywords: Python
---


#### 列表生成式

```python
    l = [x for x range(5)]  # [0, 1, 2, 3, 4]
```


#### 生成器

```python
    g = (x for x in range(5))  # <generator object <genexpr> at 0x7ff2dd15dea0>
```

注意：
    由生成器表达式所返回的那个迭代器是有状态的，用过一轮之后，就不要反复使用了

```python
    g = (x for x in range(5))
    print(list(g))  # [0, 1, 2, 3, 4]
    print(list(g))  # []
```


#### 当数据非常多时

对于列表生成式，当输入的数据比较少时，不会出问题。

但如果输入的数据非常多，那么可能会消耗大量内存，并导致程序崩溃。

这时候考虑生成器。

```python
    g = (len(x) for x in open('t.txt'))
```


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
