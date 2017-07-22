---
layout: post
title: 装饰器 
category: Python
tags: Python
keywords: Python
---

#### 不带参数的装饰器 

```python

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print("Start")
        return func(*args, **kwargs)
    return wrapper


@log             # 相当于 t = log(t)
def t():
    print("End")


t()

# Start
# End

```

#### 带参数的装饰器 

```python

def log(name):
    print(name)
        
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print("Start")
            return func(*args, **kwargs)
        return wrapper
    return decorator


@log("Tom")      # 相当于 t = log("Tom")(t)
def t():
    print("End")


t()

# Tom
# Start
# End

```
