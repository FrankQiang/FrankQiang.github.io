---
layout: post
title: Python3 笔记
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
