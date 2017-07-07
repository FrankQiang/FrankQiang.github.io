---
layout: post
title:  关于变量 
category: Python
tags: Python
keywords: Python
---

* L （Local） 局部作用域
* E （Enclosing） 闭包函数外的函数中
* G （Global） 全局作用域
* B （Built-in） 内建作用域

以 L --> E --> G --> B 的规则查找，即：在局部找不到，便会去局部外的局部找，再找不到就会去全局找，再者去内建中找。

```python

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

```

#### 修改Global variable

```python
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
```

#### 修改Enclosing variable

```python
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
```


#### globals

```python
a = 0
print(globals())        # {'a': 0 ...}


def outside():
    globals()['b'] = 1
outside()
print(globals())        # {'a': 0, 'b': 1 ...}
```

#### locals

```python
def outside():
    a = 0
    print(locals())     # {'a': 0}
outside()
```
