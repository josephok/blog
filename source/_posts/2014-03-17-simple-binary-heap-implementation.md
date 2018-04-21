---
layout: post
title: '二叉堆简单实现'
date: 2014-03-17 02:37
comments: true
categories: 数据结构与算法
---
* 二叉堆的定义

二叉堆是一棵完全二叉树。二叉堆满足堆特性：父节点的键值总是保持固定的序关系于任何一个子节点的键值，且每个节点的左子树和右子树都是一个二叉堆。
当父节点的键值总是大于或等于任何一个子节点的键值时为最大堆。 当父节点的键值总是小于或等于任何一个子节点的键值时为最小堆。

这里我们实现 **最小二叉堆堆** 。

* 二叉堆的基本操作

下图是一个最小二叉堆，使用数组形式表示：数组索引从1开始。
![](http://interactivepython.org/runestone/static/pythonds/_images/heapOrder.png)

1. 插入新元素

![](http://interactivepython.org/runestone/static/pythonds/_images/percUp.png)

比如插入`7`，首先将`7`加在数组的末尾，然后与其父节点比较，如果小于父节点则与之交换，直到大于父节点或到达树的根部。

2. 删除最小元素

![](http://interactivepython.org/runestone/static/pythonds/_images/percDown.png)

首先移除根部的元素。然后将最后一个元素移到根部，再向下过滤，遇到比它小的元素则交换，直到到达叶子节点或者没有比它更小的元素。

* 二叉堆实现

``` python binary_heap 

"""
BinaryHeap类：
方法：
delete_min：删除最小元素，时间复杂度为O(logN)
insert: 插入元素，时间复杂度为O(logN)
find_min: 返回最小元素，时间复杂度为O(1)
is_empty: 判断是否为空
"""

class BinaryHeap:
	def __init__(self, *args):
		i = len(args) // 2
		self.size = len(args)
		# index从1开始
		self.items = [None] + list(args)
		while i > 0:
			self.percolate_down(i)
			i -= 1

	def percolate_down(self, i):
		while i * 2 <= self.size:
			min_child_index = self.min_child(i)
			if self.items[i] > self.items[min_child_index]:
				self.items[i], self.items[min_child_index] = self.items[min_child_index], self.items[i]
			i = min_child_index
	
	def min_child(self, i):
		if 2 * i + 1 > self.size:
			return 2 * i
		else:
			if self.items[2*i] < self.items[2*i+1]:
				return 2 * i
			else:
				return 2 * i + 1

	def percolate_up(self, i):
		while i // 2 > 0:
			if self.items[i] < self.items[i // 2]:
				self.items[i], self.items[i // 2] = self.items[i // 2], self.items[i]
			i //= 2

	def delete_min(self):
		min_value = self.items[1]
		self.items[1] = self.items[self.size]
		self.size -= 1
		self.items.pop()
		self.percolate_down(1)
		return min_value

	def insert(self, data):
		self.size += 1
		self.items.append(data)
		self.percolate_up(self.size)

	def find_min(self):
		return self.items[1]

	def is_empty(self):
		return self.size == 0

# test
if __name__ == '__main__':
	binary_heap = BinaryHeap(9, 6, 5, 2, 3)

	# find_min
	# will print The minimum data of this Binary Heap is 2
	print("The minimum data of this Binary Heap is {0}".format(binary_heap.find_min()))

	# delete min
	binary_heap.delete_min()
	# will print The minimum data of this Binary Heap is 3
	print("The minimum data of this Binary Heap is {0}".format(binary_heap.find_min()))

	# insert 1
	# will print The minimum data of this Binary Heap is 1
	binary_heap.insert(1)
	print("The minimum data of this Binary Heap is {0}".format(binary_heap.find_min()))
	
	print(binary_heap.is_empty()) # False
	for i in range(5):
		binary_heap.delete_min()

	print(binary_heap.is_empty()) # True
	print(binary_heap.items) # [None]
```