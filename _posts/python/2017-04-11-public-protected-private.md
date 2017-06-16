---
layout: post
title:  public protected 和 private 
category: Python
tags: Python
keywords: Python
---

```python
class T(object):
    
    def __init__(self):
        self.name = "John"  # public     类的内部和外部都可以访问, 会被子类继承  
        self._age = 18      # protected  类的内部和外部都可以访问，会被子类继承
        self.__id = 123     # private    只能在类的内部访问，并且不被子类继承

t = T()
print(t.name)
print(t._age)
print(t.__id)

# John
# 18
# AttributeError: 'T' object has no attribute '__id'

print(t._T__id)  # 这种方式可以获得 private 字段内容
                 # Python 不会保证private的私密性

# 123
```

#### protected

protected 字段，提醒本类以外的使用这种字段的时候要小心。


#### private

能用 protected 字段就避免用 private 字段，只有一种情况除外

当子类不受自己控制时，用protected 字段，可能被子类的 protected 字段覆盖

```python
class T(object):
    
    def __init__(self):
        self._value = 't'

    def get_value(self):
        return self._value


class TT(T):

    def __init__(self):
        super().__init__()
        self._value = "tt"

tt = TT()
print(tt.get_value())

# tt
```

用 private 字段解决

```python
class T(object):
    
    def __init__(self):
        self.__value = 't'

    def get_value(self):
        return self.__value


class TT(T):

    def __init__(self):
        super().__init__()
        self._value = "tt"

tt = TT()
print(tt.get_value())

# t
```
