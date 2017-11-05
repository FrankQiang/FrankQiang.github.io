---
layout: post
title: key of sort itemgetter
category: CookPython
tags: Python
keywords: Python
---

#### lambda

```python

from operator import itemgetter

rows = [
    {"fname": "Brian", "lname": "Jones", "uid": 1003},
    {"fname": "David", "lname": "Beazley", "uid": 1002},
    {"fname": "John", "lname": "Cleese", "uid": 1001},
    {"fname": "Big", "lname": "Jones", "uid": 1004}
]

rows_by_fname = sorted(rows, key=lambda r: r["fname"])
rows_by_lfname = sorted(rows, key=lambda r: (r["lname"], r["fname"]))

```

#### itemgetter 代替　lambda

```python

使用 itemgetter() 方式会运行的稍微快点

from operator import itemgetter

rows = [
    {"fname": "Brian", "lname": "Jones", "uid": 1003},
    {"fname": "David", "lname": "Beazley", "uid": 1002},
    {"fname": "John", "lname": "Cleese", "uid": 1001},
    {"fname": "Big", "lname": "Jones", "uid": 1004}
]

rows_by_fname = sorted(rows, key=itemgetter("fname"))
print(rows_by_fname)

rows_by_lfname = sorted(rows, key=itemgetter("lname", "fname"))
print(rows_by_lfname)

```
