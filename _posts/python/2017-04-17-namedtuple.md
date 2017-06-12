---
layout: post
title:  namedtuple 
category: Python
tags: Python
keywords: Python
---

```python
import collections

Grade = collections.namedtuple("Grade", ("score", "weight"))

grade = Grade(89, 0.45)
print(grade)
print(grade.score)
print(grade.weight)

# Grade(score=89, weight=0.45)
# 89
# 0.45



Grade = collections.namedtuple("Others", ("score", "weight"))

grade = Grade(89, 0.45)
print(grade)

# Others(score=89, weight=0.5)
```
