---
layout: post
title: Python基础
category: 技术
tags: Python
keywords: Python skill
---

### 1 交换变量

在其它语言中，需要这样交换变量

```CPP
temp = a
a = b
b = temp
```
在Python中这样就完成交换变量

```CPP
a, b = b, a
```

* Python会先将右边的b, a生成一个tuple(元组)，存放在内存中；

* 之后会执行赋值操作，这时候会将tuple拆开；

* 然后将tuple的第一个元素赋值给左边的第一个变量，第二个元素赋值给左边第二个变量。

### 2 Python控制台的"_"

`_`用来存储上一次的打印结果。

当结果为None的时候，控制台不会打印，`_ `里面存储的值也就不会改变。

```CPP
>>> name = "John"
>>> name
'John'
>>> _
'John'
>>> name = None
>>> name
>>> _
'John'
```

### 3 合并字符串

```CPP
# No
colors = ['red', 'blue', 'green', 'yellow']

result = ''
for s in colors:
    result += s
```
join只能用于元素是字符串的list

```CPP
# Yes
result = ''.join(colors)
```

### 4 关键字in
当你需要判断一个KEY是否在dict中或者要遍历dict的KEY时，最好的方法是使用关键字in

```CPP
# No
if d.has_key('c'):  # 在Python3中已经去除该用法
    print True

for key in d.keys():
    print key
```

```CPP
# Yes
if 'c' in d:
    print True

for key in d:
    print key
```

Python的dict对象是对KEY做过hash的，而keys()方法会将dict中所有的KEY作为一个list对象；所以，直接使用in的时候执行效率会比较快，代码也更简洁。

### 5 字典


`get`如果该key不存在抛出KeyError。

```CPP
# No
d = {'a': 1, 'b': 2}
d['c']
```

```CPP
# Yes
d = {'a': 1, 'b': 2}
d.get('c')
```

`fromkeys`通过一个list生成dict，但是要提供默认值。

```CPP
dict.fromkeys(['a', 'b', 'c'], 1)
```

`setdefault`有些情况下，我们需要给dict的KEY一个默认值。

```CPP
d = {'a': 1, 'b': 2}
>>> d.setdefault('a', 0)
1
>>> d.setdefault('c', 3)
3
>>> d
{'b': 2, 'a': 1, 'c': 3}
```

### 6 字典的组装

```CPP
>>> l1 = ['a', 'b', 'c']
>>> l2 = [1, 2, 3]
>>> dict(zip(l1, l2))
{'c': 3, 'b': 2, 'a': 1}
```

### 7 Python中的True值

对于自己声明的class，如果你想明确地指定它的实例是True或False，你可以自己实现class的`__nonzero__`(Python2)`__bool__`(Python3)或`__len__`方法。

当你的class是一个container时，你可以实现`__len__`方法。

```CPP
class Test(object):

    def __init__(self, data):
        self.data = data

    def __len__(self):
        return len(self.data)
```

如果你的class不是container，你可以实现`__bool__`方法。

```CPP
class Test1(object):

    def __init__(self, value):
        self.value = value

    def __bool__(self):
        return bool(self.value)
```

### 8 `enumerate`索引和元素

在遍历列表的时候，可以通过enumerate方法来获取遍历时的index。

 enumerate方法是惰性方法，所以它只会在需要的时候生成一项。

```CPP
>>> l1 = ['a', 'b', 'c']
>>> for (index, value) in enumerate(l1):
...     print(index, value)
...
0 a
1 b
2 c

>>> e = enumerate(l1)
>>> e.__next__()
(0, 'a')
```

### 9 字符串格式化`format`

```CPP
>>> name = "John"
>>> "I'm {0}".format(name)
"I'm John"
>>> "I'm {name}".format(name=name)
"I'm John"

>>> names = ["John", "Tom"]
>>> "I'm {0[0]}".format(names)
"I'm John"

# ^、<、>分别是居中、左对齐、右对齐，后面带宽度
# :号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充
>>> num = 3
>>> "{0:0^5}".format(num)
'00300'
>>> "{0:0<5}".format(num)
'30000'
>>> "{0:0>5}".format(num)
'00003'

>>> '{:,}'.format(1234567890)
'1,234,567,890'
```

### 10 排序
sort改变原list，sorted不改变原list，同时返回新list。

```CPP
>>> l1 = ['c', 'a', 'b']
>>> l1.sort()
>>> l1
['a', 'b', 'c']
>>> l2 = ['c', 'b', 'a']
>>> sorted(l2)
['a', 'b', 'c']
>>> l2
['c', 'b', 'a']

这里通过key完成更复杂的排序，其原理为，通过key进行排序，原list根据排序结果进行一一对应，为最终排序结果。
>>> l1 = ["John", "Tom", "jack"]
>>> l1.sort(key=str.lower)
>>> l1
['jack', 'John', 'Tom']
```

### 11 获取脚本参数

```CPP
import sys

print(sys.argv[0])
print(sys.argv[1])

# 脚本名：    sys.argv[0]
# 参数1：     sys.argv[1]
# 参数2：     sys.argv[2]
# ...
```


### 12 实现杨辉三角 Generator

```CPP
# 杨辉三角

          1
        1   1
      1   2   1
    1   3   3   1
  1   4   6   4   1
1   5   10  10  5   1

```

```CPP
# 实现代码

def triangles():
    a = [1]
    while True:
        yield a
        a = [sum(i) for i in zip([0] + a, a + [0])]

# zip 函数
# a = [1, 2]  b = [4, 5]
# zip(a,b) [(1, 4), (2, 5)]
```

字符串首字母大写函数 capitalize()

### 13 生成素数

```CPP
def odd_iter():
    n = 1
    while True:
        n += 2
        yield n


def is_dividable(n):
    return lambda x: x % n > 0


def primes():
    yield 2
    it = odd_iter()
    while True:
        n = next(it)
        yield n
        it = filter(is_dividable(n), it)
```
### 14 内置排序

```CPP
sort  在原列表排序
sorted 产生新的排序列表
```

