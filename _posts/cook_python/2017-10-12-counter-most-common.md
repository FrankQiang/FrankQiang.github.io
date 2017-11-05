---
layout: post
title: 元素频次统计
category: CookPython
tags: Python
keywords: Python
---

```python


元素值需可 hashable

from collections import Counter

words = [
    "look", "into", "my", "eyes", "look", "into", "my", "eyes",
    "the", "eyes", "the", "eyes", "the", "eyes", "not", "around", "the",
    "eyes", "don't", "look", "around", "the", "eyes", "look", "into",
    "my", "eyes", "you're", "under"
]

word_counts = Counter(words)
top_three = word_counts.most_common(3)
print(top_three)

# [('eyes', 8), ('the', 5), ('look', 4)]


print(word_counts["not"])
# 1
print(word_counts["eyes"])
# 8

morewords = ["why", "are", "you", "not", "looking", "in", "my", "eyes"]

word_counts.update(morewords)
print(word_counts["eyes"])
# 9

a = Counter(words)
b = Counter(morewords)
print(a)
# Counter({'eyes': 8, 'the': 5, 'look': 4, 'into': 3, 'my': 3, 'around': 2, "don't": 1, 'under': 1, 'not': 1, "you're": 1})
print(b)
# Counter({'eyes': 1, 'are': 1, 'why': 1, 'my': 1, 'looking': 1, 'you': 1, 'in': 1, 'not': 1})
print(a+b)
# Counter({'eyes': 9, 'the': 5, 'my': 4, 'look': 4, 'into': 3, 'not': 2, 'around': 2, 'looking': 1, 'why': 1, "you're": 1, "don't": 1, 'are': 1, 'in': 1, 'under': 1, 'you': 1})
print(a-b)
# Counter({'eyes': 7, 'the': 5, 'look': 4, 'into': 3, 'around': 2, 'my': 2, "you're": 1, "don't": 1, 'under': 1})

```
