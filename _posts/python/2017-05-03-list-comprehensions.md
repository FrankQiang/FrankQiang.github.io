---
layout: post
title:  关于列表推导式 
category: Python
tags: Python
keywords: Python
---

#### 列表推导式 

```python
    a = [1, 2, 3, 4]

    [x**2 for x in a]  # [1, 4, 9. 16]
```

#### 列表推导多重循环

表达式会按照从左至右的顺序来执行

```python
    matrix= [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

    [x for row in matrix for x in row] 
    # [1, 2, 3, 4, 5, 6, 7, 8, 9]

    [[x**2 for x in row] for row in matrix]
    # [[1, 4, 9], [16, 25, 36], [49, 64, 81]]

    [[x for x in row if x % 3 ==0] for row in matrix if sum(row) >= 10]
```

这个例子已经有点复杂。

在列表推导中，最好不要使用两个以上的表达式。
可以使用两个条件、两个循环或一个条件搭配一个循环。

如果很复杂，应该用 if 和 for 语句写成辅助函数。

```python
    matrix= [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

    [[x for x in row if x % 3 == 0] for row in matrix if sum(row) >= 10]
    # [[6], [9]]
```

#### map

```python
    a = [1, 2, 3, 4]

    b = map(lambda x: x**2, a)  # 惰性生成
    list(b)  # [1, 4, 9, 16]
```

#### filter

```python
    a = [1, 2, 3, 4]

    b = filter(lambda x: x % 2 == 0, a)  # 惰性生成
    list(b)  # [2, 4]
```
