---
layout: post
title: groupby
category: CookPython
tags: Python
keywords: Python
---

```python

from operator import itemgetter
from itertools import groupby

rows = [
    {"address": "5412 NCLARK", "date": "07/01/2012"},
    {"address": "5148 NCLARK", "date": "07/04/2012"},
    {"address": "5800 E58TH", "date": "07/02/2012"},
    {"address": "2122 NCLARK", "date": "07/03/2012"},
    {"address": "5645 NRAVENSWOOD", "date": "07/02/2012"},
    {"address": "1060 WADDISON", "date": "07/02/2012"},
    {"address": "4801 NBROADWAY", "date": "07/01/2012"},
    {"address": "1039 WGRANVILLE", "date": "07/04/2012"},
]


rows.sort(key=itemgetter("date"))
for date, items in groupby(rows, key=itemgetter("date")):
    print(date)
    for i in items:
        print(' ', i)

# 07/01/2012
#   {'date': '07/01/2012', 'address': '5412 NCLARK'}
#   {'date': '07/01/2012', 'address': '4801 NBROADWAY'}
# 07/02/2012
#   {'date': '07/02/2012', 'address': '5800 E58TH'}
#   {'date': '07/02/2012', 'address': '5645 NRAVENSWOOD'}
#   {'date': '07/02/2012', 'address': '1060 WADDISON'}
# 07/03/2012
#   {'date': '07/03/2012', 'address': '2122 NCLARK'}
# 07/04/2012
#   {'date': '07/04/2012', 'address': '5148 NCLARK'}
#   {'date': '07/04/2012', 'address': '1039 WGRANVILLE'}

```


```python

这里不用先将记录排序。

如果对内存占用不是很关心，

这种方式会比先排序然后再通过 groupby() 函数迭代的方式运行得快一些。

from collections import defaultdict
rows_by_date = defaultdict(list)

rows = [
    {"address": "5412 NCLARK", "date": "07/01/2012"},
    {"address": "5148 NCLARK", "date": "07/04/2012"},
    {"address": "5800 E58TH", "date": "07/02/2012"},
    {"address": "2122 NCLARK", "date": "07/03/2012"},
    {"address": "5645 NRAVENSWOOD", "date": "07/02/2012"},
    {"address": "1060 WADDISON", "date": "07/02/2012"},
    {"address": "4801 NBROADWAY", "date": "07/01/2012"},
    {"address": "1039 WGRANVILLE", "date": "07/04/2012"},
]

for row in rows:
    rows_by_date[row["date"]].append(row)

for r in rows_by_date["07/01/2012"]:
    print(r)

# {'address': '5412 NCLARK', 'date': '07/01/2012'}
# {'address': '4801 NBROADWAY', 'date': '07/01/2012'}

```
