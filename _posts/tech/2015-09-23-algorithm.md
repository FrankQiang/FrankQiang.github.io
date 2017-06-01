---
layout: post
title: 算法
category: 技术
tags: algorithm
keywords: algorithm
---

### 排序

#### 插入法

时间复杂度 n^2

```CPP
import random
n = int(input("please enter list length: "))
nums = []

for x in range(n):
    nums.append(random.randint(1, 100))
print(nums)

for i in range(1, n):
    j = i
    while j > 0:
        if nums[j] < nums[j-1]:
            nums[j], nums[j-1] = nums[j-1], nums[j]
        j = j-1

print(nums)
```

#### 快排

时间复杂度 logN

空间复杂度 1

```CPP
import random
n = int(input("please enter list length: "))
nums = []

for x in range(n):
    nums.append(random.randint(1, 100))
print(nums)


def quick_sort(nums, low, high):
    if low < high:
        index = move(nums, low, high)
        quick_sort(nums, index+1, high)
        quick_sort(nums, low, index-1)


def move(nums, low, high):
    value = nums[low]
    i, j = low, low+1
    while j <= high:
        if nums[j] < value:
            i += 1
            nums[j], nums[i] = nums[i], nums[j]
        j += 1
    nums[low], nums[i] = nums[i], nums[low]
    return i

quick_sort(nums, 0, len(nums)-1)
print(nums)
```

#### 堆排

时间复杂度 logN

空间复杂度 1

```CPP
import random
n = int(input("please enter list length: "))
nums = []

for x in range(n):
    nums.append(random.randint(1, 100))
print(nums)


def flow_up(nums, start, end):
    root = start
    while True:
        child = 2*root + 1
        if child > end:
            break
        if child+1 <= end and nums[child] < nums[child+1]:
            child = child+1
        if nums[root] < nums[child]:
            nums[root], nums[child] = nums[child], nums[root]
            root = child
        else:
            break


def heap_sort(nums):
    for start in range((len(nums)-2)//2, -1, -1):
        flow_up(nums, start, len(nums)-1)

    for end in range(len(nums)-1, 0, -1):
        nums[end], nums[0] = nums[0], nums[end]
        flow_up(nums, 0, end-1)

heap_sort(nums)
print(nums)

```


#### 并归

时间复杂度 logN

空间复杂度 n

```CPP
import random
n = int(input("please enter list length: "))
nums = []

for x in range(n):
    nums.append(random.randint(1, 100))
print(nums)


def merge(list_a, list_b):
    j, k = 0, 0
    list_c = []
    while j < len(list_a) and k < len(list_b):
        if list_a[j] < list_b[k]:
            list_c.append(list_a[j])
            j += 1
        else:
            list_c.append(list_b[k])
            k += 1

    list_c += list_a[j:]
    list_c += list_b[k:]
    return list_c


def binary(nums):
    if len(nums) <= 1:
        return nums
    else:
        index = len(nums)//2
        left = binary(nums[:index])
        right = binary(nums[index:])
        return merge(left, right)

print(binary(nums))

```

### 树

#### 二叉树

理想时间复杂度logN,但可能出现最坏情况,可以通过随机化数据，避免最坏的情况。
```CPP
class Node(object):

    def __init__(self):
        self.left = None
        self.right = None
        self.value = None


class Tree(object):

    def __init__(self):
        self.root = Node()

    def add(self, value):
        node = self.root
        while node.value:
            if node.value < value:
                if not node.right:
                    node.right = Node()
                node = node.right
            else:
                if not node.left:
                    node.left = Node()
                node = node.left
        node.value = value
        def factory(self):
            l = [1, 4, 2, 6, 8]
            for i in l:
                self.add(i)

        def display(self, node):
            print(node.value)
            if node.left:
                self.display(node.left)
            if node.right:
                self.display(node.right)

        def main(self):
            self.factory()
            self.display(self.root)
```

#### 字典树

```CPP
class Node:
        def __init__(self):
            self.value = None
            self.children = {}  # children is of type {char, Node}


class Trie:
    def __init__(self):
        self.root = Node()

    def insert(self, value):
        value = value.lower()
        node = self.root
        for char in value:
            if char not in node.children:
                child = Node()
                node.children[char] = child
                node = child
            else:
                node = node.children[char]
        node.value = value
        def search(self, value):
            node = self.root
            for char in value:
                if char in node.children:
                    node = node.children[char]

            if node.value and (len(node.value) == len(value)):
                print(node.value)
            else:
                print("no found")

    if __name__ == "__main__":

        trie = Trie()
        trie.insert("sorry")
        trie.insert("hello")
        trie.insert("thank")
        trie.insert("fuck")
        trie.search("than")
        trie.search("hello")
```
