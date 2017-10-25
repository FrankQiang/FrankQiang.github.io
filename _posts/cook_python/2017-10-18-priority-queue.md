---
layout: post
title: Priority Queue 
category: CookPython
tags: Python
keywords: Python
---

```python

import heapq


class PriorityQueue:
    def __init__(self):
        self._queue = [] 
        self._index = 0

    def push(self, item, priority):
        heapq.heappush(self._queue, (-priority, self._index, item))
        self._index += 1 

    def pop(self):
        return heapq.heappop(self._queue)[-1]


class Item:

    def __init__(self, name):
        self.name = name 

    def __repr__(self):
        return'Item({!r})'.format(self.name)


q = PriorityQueue()
q.push(Item('foo'), 1)
q.push(Item('bar'), 5)
q.push(Item('spam'), 4)
q.push(Item('grok'), 1)

print(q.pop())
# Item('bar')
print(q.pop())
# Item('spam')
print(q.pop())
# Item('foo')
print(q.pop())
# Item('grok')


函数 heapq.heappush() 和 heapq.heappop()分别在队列 _queue 上插入和删除一个元素， 

并且队列_queue保证第一个元素拥有最小优先级。 heappop() 函数总是返回”最小的”的元素，

这就是保证队列pop操作返回正确元素的关键。 

另外，由于push和pop操作时间复杂度为O(N)，其中N是堆的大小。



队列包含了一个 (-priority,index, item) 的元组。 

优先级为负数的目的是使得元素按照优先级从高到低排序。


index 变量的作用是保证同等优先级元素的正确排序。 

通过保存一个不断增加的 index下标变量，可以确保元素安装它们插入的顺序排序。 

而且， index 变量也在相同优先级元素比较的时候起到重要作用。



Item实例是不支持排序的


如果使用元组 (priority, item) ，只要两个元素的优先级不同就能比较。 

但是如果两个元素优先级一样的话，一样不能比较。


通过引入另外的 index 变量组成三元组 (priority,index, item)，就可以了。


a = (1, 0, Item('foo'))
b = (5, 1, Item('bar'))
c = (1, 2, Item('grok'))
print(a < b)
# True
print(a < c)
# True
```
