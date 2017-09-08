---
layout: post
title: 关于读取文件
category: 技术
tags: 读取文件
keywords: 读取文件, 笔记
---

```
Character   Meaning
'r'     open for reading (default)
        read 读取文件，文件必须存在 (默认)

'w'     open for writing, truncating the file first
        write 写文件，写文件之前清空文件。若文件不存在则建立该文件。

'x'     open for exclusive creation, failing if the file already exists
        create 创建不存在的文件，并可以写。

'a'     open for writing, appending to the end of the file if it exists
        append 若文件已经存在则追加，若文件不存在则建立该文件。

'b'     binary mode
        以二进制形式读或写文件

't'     text mode (default)
        以文本形式读或写文件 (默认)

'+'     open a disk file for updating (reading and writing)
        在 read write create append 的基础上同时有读写的能力

```


* `r` 打开只读文件，该文件必须存在。

* `r+` 打开可读写的文件，该文件必须存在。

* `w` 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。

* `w+` 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。

* `a` 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。

* `a+` 以附加方式打开可读写的文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾后，即文件原先的内容会被保留。


* `rb` 打开只读二进制文件，该文件必须存在。

* `wb`打开只写二进制文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。
