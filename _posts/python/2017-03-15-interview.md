---
layout: post
title: Interview 
category: Python
tags: Python
keywords: Python
---

### Python 

#### 1 Namespace 

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

#### 2 Pass A Variable 

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

#### 3 instance Variable and class Variable

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

#### 4 List Comprehension 

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

#### 5 Generator 

```python

通常生成器是通过调用一个或多个yield表达式构成的函数生成的

def t(): 
    yield 5


t()  # <generator object ...>

l = [x for x range(5)]  # [0, 1, 2, 3, 4]

g = (x for x in range(5))  # <generator object ...>

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

#### 6 Dict Comprehension 

```python

keys = ["name", "age"]
values = ["Tom", 18]
d = {key: value for(key, value) in zip(keys, values)}
print(d)

# {'name': 'Tom', 'age': 18}

```

#### 7 Single Underscore and Double Underscore 

```python

class T(object):

    def __init__(self, name, age):
        self._name = name 
        self.__age = age


t = T("Tom", 18)
print(t._name)
print(t._T__age)
print(t.__age)

__foo__: 一种约定, Python内部的名字,用来区别其他用户自定义的命名,以防冲突.

_foo: 一种约定,用来指定变量私有. 可以被继承.

__foo: 是真正的私有变量, 不能被继承, 用来区别其它类相同的命名, 
       但也可以通过_classname__foo来访问,
       we are all consenting adults here 
       一般不会通过这种方法来操作变量.

```

#### 8 format 

```python

name = "John"
print("I'm {0}".format(name))
# I'm John
print("I'm {name}".format(name=name))
# I'm John
names = ["John", "Tom"]
print("I'm {0[0]}".format(names))
# I'm John

# ^、<、>分别是居中、左对齐、右对齐，后面带宽度
# :号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充

num = 3
print("{0:0^5}".format(num))
# 00300
print("{0:0<5}".format(num))
# 30000
print("{0:0>5}".format(num))
# 00003
print('{:,}'.format(1234567890))
# 1,234,567,890

```

#### 9 args and kwargs 

```python

它们的顺序必须是

* 位置参数 > 默认参数 > 可变参数 >  命名关键字参数 > 关键字参数


位置参数

def t(name):    # name 就是一个位置参数
    print(name)


默认参数

def t(name, age=18):  # age 就是一个默认参数
    print(name, age)

t("John")     # John 18
t("Tom", 16)  # Tom 16


默认参数最好用不可变类型


# 在每个人加一个成绩

def t(name, scores=[]):
    scores.append(90)
    print(name, scores)

t("John", [80])
t("Tom", [76])

t("Marsh")
t("Jim")

# John [80, 90]
# Tom [76, 90]
# Marsh [90]
# Jim [90, 90]  # 出现错误



由于 scores 参数的默认值只会在模块加载时计算一次，
所以凡是以默认形式来调用 t 函数的代码，都将共享同一份数组。
这会引发非常奇怪的行为。

def t(name, scores=None):
    if scores is None:
        scores = [90]
    else:
        scores.append(90)
    print(name, scores)

t("John", [80])
t("Tom", [76])

t("Marsh")
t("Jim")

# John [80, 90]
# Tom [76, 90]
# Marsh [90]
# Jim [90]
 

可变参数

def t(name, age=18, *args):  # 把 *args 称为可变参数
    print(name, age)         # 会把 78 83 95 转成一个 tuple (元组)
    for score in args:       # 所以当参数特别多时，会耗尽内存，导致崩溃
        print(score)

t("John", 18, 78, 83)        # 这里必须写 age，不然会被后面的值覆盖

或者

scores = [78, 83]
t("John", 18, *scores)

# John 18
# 78
# 83


命名关键字参数

必须需要一个或多个关键字参数

def t(name, age=18, *, country, **kw):
    print(name, age)
    print(country)
    for key, value in kw.items():
        print(key, value)

t("John", 18, country="China", gender="male")

或者

def t(name, age=18, *args, country, **kw):
    print(name, age)
    for score in args:
        print(score)
    print(country)
    for key, value in kw.items():
        print(key, value)

t("John", 18, 78, 83, country="China", gender="male")


关键字参数

def t(name, age=18, *args, **kw):
    print(name, age)
    for score in args:
        print(score)
    for key, value in kw.items():
        print(key, value)


t("John", 18, 78, 83, gender="male")

或者

others = {"gender": "male"}
t("John", 18, 78, 83, **others)

又或者

scores = [78, 83]
others = {"gender": "male"}
t("John", 18, *scores, **others)

# John 18
# 78
# 83
# gender male


```

#### 10 new and init 

```python

1. __new__是一个静态方法,而__init__是一个实例方法.

2. __new__方法会返回一个创建的实例,而__init__什么都不返回.

3. 只有在__new__返回一个cls的实例时后面的__init__才能被调用.

4. 当创建一个新实例时调用__new__,初始化一个实例时用__init__.

```

#### 11 Singleton 

```python

在我们工作中经常需要在应用程序中保持一个唯一的实例，

如：IO处理，数据库操作，配置文件等，由于这些对象都要占用重要的系统资源，

所以我们必须始终使用一个公用的实例，如果创造出来多个实例，就会导致许多问题，

比如占用过多资源，不一致的结果等

class Singleton(object):

    def __new__(cls, *args, **kwarg):
        if not hasattr(cls, "_instance"):
            orign = super(Singleton, cls)  # 不清楚什么意思 
            cls._instance = orign.__new__(cls, *args, **kwarg)
        return cls._instance


class MyClass(Singleton):
    pass 


my1 = MyClass()
my2 = MyClass()
print(id(my1))  # 140160695672168
print(id(my2))  # 140160695672168


另外一种单例模式

# mysingleton.py
class My_Singleton(object):
    def foo(self):
            pass

            my_singleton = My_Singleton()

# to use
from mysingleton import my_singleton

my_singleton.foo()

```

#### 12 作用域 

```python

* L （Local） 局部作用域
* E （Enclosing） 闭包函数外的函数中
* G （Global） 全局作用域
* B （Built-in） 内建作用域

以 L --> E --> G --> B 的规则查找，即：在局部找不到，便会去局部外的局部找，再找不到就会去全局找，再者去内建中找。


__builtins__.a = 0  # Built-in variable
        
b = 1               # Global variable
        

def outside():
    c = 2           # Enclosing variable

    def inside():
        d = 3       # Local variable
        print(a)
        print(b)
        print(c)
        print(d)

    inside()


outside()

# 0
# 1
# 2
# 3


修改Global variable

a = 0              
  
  
def outside():
    b = 1          

    def inside():
        global a    # 与全局变量a指向同一个对象
        a = 100
        c = 2      
        print(a)
        print(b)
        print(c)

    inside()

outside()
print(a)

# 100
# 1
# 2
# 100


修改Enclosing variable

a = 0              
  
  
def outside():
    b = 1          

    def inside():
        nonlocal b    # 与Enclosing variable b指向同一个对象
        b = 10
        c = 2      
        print(a)
        print(b)
        print(c)

    inside()
    print(b)

outside()

# 0
# 10
# 2
# 10


globals

a = 0
print(globals())        # {'a': 0 ...}


def outside():
    globals()['b'] = 1
outside()
print(globals())        # {'a': 0, 'b': 1 ...}


locals

def outside():
    a = 0
    print(locals())     # {'a': 0}
outside()

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
