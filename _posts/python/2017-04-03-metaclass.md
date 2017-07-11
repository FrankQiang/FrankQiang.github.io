---
layout: post
title:  元类 
category: Python
tags: Python
keywords: Python
---

#### 元类的基本用法

```python

class Meta(type):                                           # 需要继承type
    
    def __new__(meta, name, bases, class_dict):             # 利用`__new__`创建实例的特性
        print(meta, name, bases, class_dict)
        return type.__new__(meta, name, bases, class_dict)


class T(object, metaclass=Meta):
    name = "John"

    def show(self):
        print("Begin")


t = T()

# <class '__main__.Meta'> 
# T 
# (<class 'object'>,) 
# {'__module__': '__main__', 
#    'show': <function T.show at 0x7f821536b730>, 
#    'name': 'John', 
#    '__qualname__': 'T'
# }
```

#### 元类的应用


```python

class Field(object):

    def __init__(self):
        self.name = None
        self.internal_name = None       

    def __get__(self, instance, instance_type):
        if instance is None:
            return self
        return getattr(instance, self.internal_name, '')

    def __set__(self, instance, value): 
        setattr(instance, self.internal_name, value)


class Meta(type):

    def __new__(meta, name, bases, class_dict):
        for key, value in class_dict.items():
            if isinstance(value, Field):    
                value.name = key    
                value.internal_name = '_' + key
        cls = type.__new__(meta, name, bases, class_dict)
        return cls


class Row(object, metaclass=Meta):
    pass


class Customer(Row):
    name = Field()
    age = Field()


c = Customer()
print("Before", repr(c.name), c.__dict__)
c.name = "John"
print("After", repr(c.name), c.__dict__)


# Before '' {}
# After 'John' {'_name': 'John'}
```
