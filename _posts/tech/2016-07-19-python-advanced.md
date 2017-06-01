---
layout: post
title: Python进阶
category: 技术
tags: Python3
keywords: Python3  Advanced
---

## 1. 基本问题

### 1.1 版本

目前看来，Python3已经是主流了。点击[http://py3readiness.org/](http://py3readiness.org/)查看主流第三方库和框架对Python3的支持。

### 1.2 风格规范

[Google Python编码规范](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)

### 1.2 虚拟机

深入理解Python3。

### 1. 示例

这个示例有两个文件

```
# sub.py

def info():
    print("I'm a sub module")

```

```
# main.py

import sub

print("I'm the main processor")

if __name__ == '__main__':
    sub.info()

```

执行main.py程序

```
python3 main.py

```

输出结果

```
I'm the main processor
I'm a sub module
```

### 2. 运行原理

### 2.1 模块

在Python中将sub.py文件视为一个模块(Module)， 为避免模块相同， 可以通过包(Package)来组织模块. 包下面必须有`__init__.py`文件. `__init__.py`本身是一个模块， 且模块名就是包名.这里main.py作为top-level脚本.下面会讲到。

### 2.2编译

执行python3 main.py， 将会启动Python的解释器，然后将main.py编译成一个字节码对象PyCodeObject。 在Python3的世界中，一切都是对象。

在运行期间，编译结果也就是PyCodeObject对象，只会存在内存中，当这个模块执行完后， 会把编译结果保存到pyc文件中，下次就不用编译了，直接加载到内存中，pyc只是PyCodeObject对象在硬盘的表现形式。同级目录的pyc文件都存放在`__pycache__`文件夹下。

#### 注意:
Python只会对那些以后可能继续被使用和载入的模块才会生成pyc文件。Python认为使用了import指令的模块，属于这种类型.而main.py只是临时用一次.所以在`__pycache__`中只有sub模块的pyc文件。

### 2.3pyc文件

一个pyc文件包含三部分信息，分别是Python的magic number， pyc文件创建的时间信息，和PyCodeObject对象。

magic number是Python定义的一个整数值.一般来说， 不同版本的Python都会定义不同的magic number，这个值是来保证Python的兼容性的.比如低版本编译的pyc文件不能让高版本的Python程序来执行。只要检查magic number就可以了。

pyc文件创建的时间信息，用来对比源文件最后修改的时间，如果晚于源文件修改时间，将重新编译新的pyc文件。

### 2.4Python虚拟机

main.py编译后，接下来就由Python虚拟机来执行字节码指令。Python虚拟机会从编译的PyCodeObject对象中依次读入每一条字节码指令，并在当前的上下文环境中执行字节码指令。

### 2.5绝对引入与相对引入

直接运行`python main.py`

#### 绝对引入的使用

    from pkg import moduleX
    from pkg.subpkg import moduleY

    需要注意的是，需要从包的顶级目录开始依次写下。

#### 相对引入的使用

    from .moduleX import num
    from ..moduleX import num
    from ...moduleX import num

   一个`.`代表当前目录， 每多一个`.`就代表指向上一层目录。同时每个目录应该是一个包。

[引入原理](https://laike9m.com/blog/pythonxiang-dui-dao-ru-ji-zhi-xiang-jie,60/)

[http://stackoverflow.com/questions/14132789/python-relative-imports-for-the-billionth-time#answer-14132912](http://stackoverflow.com/questions/14132789/python-relative-imports-for-the-billionth-time#answer-14132912)


### 3 内置类型

### 3.1 数字

#### bool

None、0、空字符串、以及没有元素的容器对象都可视为 False,反之为 True。

True 可以当1，False可以当0.

#### int

sys.maxsize查看支持的最大数。

#### float

使用用双精度浮点数 (float),不能 "精确" 表示某些十进制的小数值。尤其是 "四舍五入(round)" 的结果,可能和预想不同。

```CPP
>>> 3/2
1.5
>>> 3//2
1

>>> 3*0.1 == 0.3    # 这个容易导致奇怪的错误
False
>>> round(2.675, 2) # 并没有想象的四舍五入
2.67
```

如果需要,可用用 Decimal 代替,它能精确控制运算精度、有效数位和 round 的结果。

```CPP
from decimal import Decimal, ROUND_UP, ROUND_DOWN

>>> 3*Decimal('0.1') == Decimal('0.3')
True
>>> Decimal('2.675').quantize(Decimal('.01'), ROUND_UP)
Decimal('2.68')
>>> Decimal('2.675').quantize(Decimal('.01'), ROUND_DOWN)
Decimal('2.67')
```

### 3.2字符串

```CPP
>>> ','.join(['a', 'b', 'c'])
'a,b,c'
>>> "a,b,c".split(',')
['a', 'b', 'c']
>>> "a\nb\nc".splitlines()
['a', 'b', 'c']
>>> "a\nb\nc".splitlines(True)
['a\n', 'b\n', 'c']
>>> "abc".upper()
'ABC'
>>> "Abc".lower()
'abc'

>>> " abc".lstrip()
'abc'
>>> "abc ".rstrip()
'abc'
>>> " abc ".strip()
'abc'

>>> "abc".strip("ac")
'b'
>>> "abc".replace('a', 'A')
'Abc'

>>> "a\tbc".expandtabs(4)
'a   bc'

>>> "abc".ljust(5, '0')
'abc00'
>>> "abc".rjust(5, '0')
'00abc'
>>> "abc".center(5, '0')
'0abc0'

>>> "123".zfill(6)
'000123'
```

`format`

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

常见的字符序列

```CPP
>>> from string import ascii_letters
>>> ascii_letters
'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
>>> from string import digits
>>> digits
'0123456789'

>>> from string import Template
>>> Template("$name").substitute(name="John")
'John'

>>> Template("${name}").substitute(name="John")
'John'

>>> Template("${name}").safe_substitute(name="John")   # 没有找到值， 不会报错。
'John'
```

`池化`

在 Python 进程中,无数的对象拥有一堆类似 `__name__`、`__doc__` 这样的名字,池化有助于减少对象数量和内存消耗, 提升性能。

用 intern() 函数可以把运行期动态生成的字符串池化。

```CPP
>>> s = ''.join(['a', 'b', 'c'])                                                                                                      >>> import sys          
>>> s is "abc"
False

>>> sys.intern(s) is "abc"
True

```

### 3.3 list

```CPP
>>> [x for x in range(10)]
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> l = [x for x in range(10)]
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> l.append(10)
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> l.insert(1, 0)
>>> l
[0, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> l.pop()
10
>>> l
[0, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> l.pop(8)
7
>>> l
[0, 0, 1, 2, 3, 4, 5, 6, 8, 9]
>>> l.remove(1)
>>> l
[0, 0, 2, 3, 4, 5, 6, 8, 9]
>>> l.count(0)
2

>>> l
['a', 'b', 'c']
>>> l.index('c')
2
```


可用 bisect 向有序列表中插入元素。

```CPP
>>> import bisect
>>> l = [x for x in range(10)]
>>> l
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> bisect.insort(l, 2)
>>> l
[0, 1, 2, 2, 3, 4, 5, 6, 7, 8, 9]
```
`关于性能`

对于频繁增删元素的大型列表,应该考虑用用链表等数据结构代替。


某些时候,可以考虑用数组代替列表。 和列表存储对象指针不同,数组直接内嵌数据,既省了创建对象的内存开销,又又提升了读写效率。

```CPP
>>> import array
>>> a = array.array('l', range(10))
>>> a
array('l', [0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> a.tolist()
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```


### 3.4 tuple

```CPP
>>> t = (2,)
>>> t
(2,)

>>> t = ('a', 'b', 'c', 'a')
('a', 'b', 'c', 'a')
>>> t.count('a')
2
>>> t.index('c')
2

```

标准库另提供了特别的 namedtuple,可用用名字访问元素项。

```CPP
>>> from collections import namedtuple
>>> User = namedtuple("User", "name age")
>>> u = User("John", 20)
>>> u.name
'John'
>>> u.age
20

```

### 3.5 dict

生成dict

```CPP
dict.fromkeys(['a', 'b', 'c'], 1)

>>> l1 = ['a', 'b', 'c']
>>> l2 = [1, 2, 3]
>>> dict(zip(l1, l2))
{'c': 3, 'b': 2, 'a': 1}
```

基本操作

```CPP
>>> d = {"name": "John", "age": 20}
>>> d["gender"] = "boy"
>>> d
{'gender': 'boy', 'age': 20, 'name': 'John'}
>>> d.update({"country": "china"})
>>> d
{'gender': 'boy', 'age': 20, 'country': 'china', 'name': 'John'}
>>> d.pop("gender")
'boy'
>>> d
{'age': 20, 'country': 'china', 'name': 'John'}
```

获取数据

```CPP
>>> d = {'a': 1, 'b': 2}
>>> d.get('c')
>>> d.get('a')
1
>>> d
{'a': 1, 'b': 2}

d = {'a': 1, 'b': 2}
>>> d.setdefault('a', 0)
1
>>> d.setdefault('c', 3)
3
>>> d
{'b': 2, 'a': 1, 'c': 3}
```

```CPP
d = {'a': 1, 'b': 2}
>>> d.values()
dict_values([1, 2])
>>> d.keys()
dict_keys(['a', 'b'])
>>> d.items()
dict_items([('a', 1), ('b', 2)])
```

扩展

```CPP
>>> from collections import defaultdict
>>> d = defaultdict(list)
>>> d['a'].append(1)  # key "a" 不存在,直接用 list() 函数创建一个空列表作为 value。
>>> d['a'].append(2)
>>> d['a']
[1, 2]
```

字典是哈希表,默认迭代是无序的。有序可以用 OrderedDict。

```CPP
>>> d = {}
>>> d['a'] = 1
>>> d['b'] = 2
>>> d['c'] = 3
>>> d['d'] = 4
>>> d
{'a': 1, 'b': 2, 'd': 4, 'c': 3}


>>> from collections import OrderedDict
>>> od = OrderedDict()
>>> od['a'] = 1
>>> od['b'] = 2
>>> od['c'] = 3
>>> od['d'] = 4
>>> od
OrderedDict([('a', 1), ('b', 2), ('c', 3), ('d', 4)])
```

### 3.6 set

集合只能存储可哈希对象,一样有只读版本 frozenset。

```CPP
判重公式:(a is b) or (hash(a) == hash(b) and eq(a, b))
```

```CPP
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
>>> 1 in s
True
>>> 4 in s
False
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.remove(4)
>>> s
{1, 2, 3}

>>> s.update(set([4, 5]))
>>> s
{1, 2, 3, 4, 5}
>>> s.pop()
1

```

集合运算

```CPP
>>> s1 = set("abc")
>>> s2 = set("bcd")
>>> s1 & s2
{'b', 'c'}
>>> s1 | s2
{'b', 'd', 'c', 'a'}
>>> s1 - s2
{'a'}
>>> s1 ^ s2
{'a', 'd'}

>>> set("abcd").isdisjoint("bc") # 判断是否没有交集
False

```



### 4异常

除了多个else，和其它语言没什么区别。

```
def test(n):
    try:
        if n % 2:
            raise Exception("Error message!")
    except Exception as ex:
        print(ex)
    else:
        print("Error")
    finally:
        print("Finally")
```
关键字raise抛出异常，else只在没有异常发生时执行。无论如何finally都会执行。

可以有多个except分支捕获不同类型的异常。

```
def test(n):
    try:
        if n == 0:
            raise NameError()
        if n == 1:
            raise KeyError()
        if n == 2:
            raise IndexError()
        else:
            raise Exception()
    except (NameError, KeyError) as ex:  #  可以同时捕获不同类型的异常
        print(type(ex))
    except IndexError as ex:             # 捕获具体类型异常
        print("IndexError")
    except:                              # 捕获任意类型异常
        print("Exception")
```

支持在except中重新抛出异常

```
try:
    try:
        raise Exception("Exception")
    except:
        print("Catch")
        raise                           # 原样抛出异常
except:
    print("Finally catch")
```

如果需要，可用sys.exc_info()获取调用堆栈上的最后异常信息。

```
def test():
    try:
        raise KeyError("Key error")
    except:
        exc_type, exc_value, traceback = sys.exc_info()
        sys.excepthook(exc_type, exc_value, traceback) # 显示异常信息
```

warnings

```
def test():
    warnings.warn("hi")  # 默认只显示警告信息，不中断执行。
    print("test")
```
### 4.1assert

```
def test(n):
    assert n > 0, "n必须大于0"  # 错误信息是可选的
    print(n)
```
当条件不符时，抛出AssertionError错误。assert受只读__debug__控制，可以在启动时添加"-O"参数使其失效。

```
python3 -O test.py
```
