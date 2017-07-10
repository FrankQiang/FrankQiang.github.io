---
layout: post
title:  __getattr__ __getattribute__ __setattr__ 
category: Python
tags: Python
keywords: Python
---

#### `__getattr__` 

```python
class T(object):

    def __init__(self):
        self.name = "John"

    def __getattr__(self, attr):        # 如果访问实例没有的属性，就会触发这个方法
        print("Called __getattr__")
        value = 18
        setattr(self, attr, value)      # 这里为实例没有的属性设置默认值
        return value


t = T() 
print(t.name)
print(t.age)
print(t.age)


# John
# Called __getattr__
# 18
# 18
```


#### `__getattribute__` 

```python

class T(object):

    def __init__(self):
        self.name = "John"
        
    def __getattribute__(self, attr):              # 访问实例的属性，就会触发这个方法
        print("Called __getattribute__")
        try:
            return super().__getattribute__(attr)  # 用这种方式获取属性值。
        except AttributeError:                     # 如果用self.name获取属性值，会再次触发这个方法，形成死循环。
            value = 18
            setattr(self, attr, value)
            return value


t = T()
print(t.name)
print(t.age)


# Called __getattribute__
# John
# Called __getattribute__
# 18
```

#### `__setattr__` 

```python
class T(object):

    def __setattr__(self, name, value):    # 对实例属性赋值，就会触发这个方法
        print("Called __setattr__") 
        super().__setattr__(name, value)   # 用这种方式为实例属性赋值
                                           # 如果用setattr赋值，会再次触发这个方法，形成死循环。


t = T()
t.age = 18
print(t.age)


# Called __setattr__
# 18
```
