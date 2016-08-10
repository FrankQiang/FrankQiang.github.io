---
layout: post
title: Python 技巧
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
