---
layout: post
title: Python3 笔记
category: 技术
tags: Python3
keywords: Python3  Advanced
---

深入理解Python3。

### 1.1 模块

在Python中将.py的文件视为一个模块(Module), 为避免模块相同， 通过包(Package)来组织模块. 包下面必须有`__init__.py`文件. `__init__.py`本身是一个模块, 且模块名就是包名.

### 1.2编译

执行python3 test.py, 将会启动Python的解释器，然后将test.py编译成一个字节码对象PyCodeObject. 在Python3的世界中，一切都是对象.

在运行期间,编译结果也就是PyCodeObject对象,只会存在内存中,当这个模块执行完后, 会把编译结果保存到pyc文件中,下次就不用编译了,直接加载到内存中,pyc只是PyCodeObject对象在硬盘的表现形式.同级目录的pyc文件都存放在`__pycache__`文件夹下.

### 1.3pyc文件

一个pyc文件包含三部分信息,分别是Python的magic number, pyc文件创建的时间信息,和PyCodeObject对象.

magic number是Python定义的一个整数值.一般来说, 不同版本的Python都会定义不同的magic number,这个值是来保证Python的兼容性的.比如低版本编译的pyc文件不能让高版本的Python程序来执行.只要检查magic number就可以了.

pyc文件创建的时间信息,用来对比源文件最后修改的时间,如果晚于源文件修改时间,将重新编译新的pyc文件.

### 1.4字Python虚拟机

test.py编译后,接下来就由Python虚拟机来执行字节码指令.Python虚拟机会从编译的PyCodeObject对象中依次读入每一条字节码指令,并在当前的上下文环境中执行字节码指令.

### 1.5绝对引入与相对引入
