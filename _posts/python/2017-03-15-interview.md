---
layout: post
title: Interview 
category: Python
tags: Python
keywords: Python
---

### Python 

#### Namespace 

```python

>>> a = 5
>>> globals()
{'a': 5, ...}  a 指向5这个对象 

>>> a = "test"
>>> globals()
{'a': 'test', ...}  a 指向'test'这个对象

在Python源码中, 有这样一句话: Names have no type, but objects do.

Python的名字实际上是一个字符串对象,

它和所指向的目标对象一起在名字空间中构成一项 {name: object} 关联。

这样Python就拥有了动态语言的动能

```

#### Pass A Variable 

```python

当一个参数传递给函数的时候, 相当于a (local) = a (global) 赋值操作

虽然它们不是同一个变量, 但它们任然指向同一个对象.

# immutable  For example string, tuple and number
a = 1

print(id(a))  # 10914368


def fun(a):
    print(id(a))  # 10914368
    a = 2         #　这里改变a (local) 指向的对象
    print(id(a))  # 10914400


fun(a)
print(id(a))  # 10914368

# mutable  For example list, dict and set
a = [] 
print(id(a))  # 140408380111368


def fun(a):
    print(id(a))  # 140408380111368
    a.append(1)   # 这里没有改变a (local) 指向的对象
    print(id(a))  # 140408380111368


fun(a)
print(id(a))  # 140408380111368

```

#### instance Variable and class Variable

```python

class Person:
    name = "aaa"


p1 = Person()
p2 = Person()
p1.name = "bbb"         # 创建实例变量name 并指向新的对象
print(id(p1.name))      # 140078039596032
print(id(p2.name))      # 140078039563040 访问类变量
print(id(Person.name))  # 140078039563040 访问类变量


class Person:
    name = [] 


p1 = Person()
p2 = Person()
p1.name.append(1)       # 访问类变量
print(id(p1.name))      # 140078039616392 访问类变量
print(id(p2.name))      # 140078039616392 访问类变量
print(id(Person.name))  # 140078039616392 访问类变量

```

#### List Comprehension 

```python

a = [1, 2, 3, 4]

[x**2 for x in a]  # [1, 4, 9. 16]


表达式会按照从左至右的顺序来执行

matrix= [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

[x for row in matrix for x in row] 
# [1, 2, 3, 4, 5, 6, 7, 8, 9]

[[x**2 for x in row] for row in matrix]
# [[1, 4, 9], [16, 25, 36], [49, 64, 81]]

[[x for x in row if x % 3 ==0] for row in matrix if sum(row) >= 10]


这个例子已经有点复杂。

在列表推导中，最好不要使用两个以上的表达式。
可以使用两个条件、两个循环或一个条件搭配一个循环。

如果很复杂，应该用 if 和 for 语句写成辅助函数。

matrix= [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

[[x for x in row if x % 3 == 0] for row in matrix if sum(row) >= 10]
# [[6], [9]]

map 代替列表推导
a = [1, 2, 3, 4]

b = map(lambda x: x**2, a)  # 惰性生成
list(b)  # [1, 4, 9, 16]

filter 代替列表推导
a = [1, 2, 3, 4]

b = filter(lambda x: x % 2 == 0, a)  # 惰性生成
list(b)  # [2, 4]


```

#### Generator 

```python

l = [x for x range(5)]  # [0, 1, 2, 3, 4]

g = (x for x in range(5))  # <generator object <genexpr> at 0x7ff2dd15dea0>

注意：
    由生成器表达式所返回的那个迭代器是有状态的，
	用过一轮之后，就不要反复使用了

g = (x for x in range(5))
print(list(g))  # [0, 1, 2, 3, 4]
print(list(g))  # []

对于列表生成式，当输入的数据比较少时，不会出问题。

但如果输入的数据非常多，那么可能会消耗大量内存，并导致程序崩溃。

这时候考虑生成器。

g = (len(x) for x in open('t.txt'))

```

### Algorithm 

```python

```

### Mysql 

```python

```

### Linux 

```python

```

### Other 

```python

```
