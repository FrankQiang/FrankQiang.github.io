---
layout: post
title: key of sort attrgetter
category: CookPython
tags: Python
keywords: Python
---

#### lambda

```python

class User:
    def __init__(self, user_id):
        self.user_id = user_id

    def __repr__(self):
        return "User({})".format(self.user_id)


users = [User(23), User(3), User(99)]
print(sorted(users, key=lambda u: u.user_id))
# [User(3), User(23), User(99)]

```

#### attrgetter 代替　lambda

```python

attrgetter函数通常会运行的快点，

并且还能同时允许多个字段进行比较。

from operator import attrgetter


class User:
    def __init__(self, user_id):
        self.user_id = user_id

    def __repr__(self):
        return "User({})".format(self.user_id)


users = [User(23), User(3), User(99)]
print(sorted(users, key=attrgetter("user_id")))
# [User(3), User(23), User(99)]

```
