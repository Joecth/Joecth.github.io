---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-09-25
---





## 840 · Range Sum Query - Mutable

Given an integer array `nums`, and then you need to implement two functions:

- `update(i, val)` Modify the element whose index is i to val.
- `sumRange(l, r)` Return the sum of elements whose indexes are in range of [l, r][*l*,*r*].

#### My Code

```python
class BIT:
    def __init__(self, n):
        self.arr = [0] * (n + 1)
    def add(self, i, val):
        now = i + 1
        while now < len(self.arr):
            self.arr[now] += val
            now += now & -now
        # print(self.arr)

    def query(self, i):
        ans = 0
        now = i + 1
        while now > 0:
            ans += self.arr[now]
            now -= now & -now
        return ans

class NumArray:

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.bit = BIT(len(nums))
        for i in range(len(nums)):
            self.bit.add(i, s[i])

    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        # self.bit.add(i, val)      # 題目注意!!!!! 是update 不是 add!
        self.bit.add(i, val - self.sumRange(i, i))
        

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        return self.bit.query(j) - self.bit.query(i - 1)
        

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```







## Max Tree

```python
    """
    @param A: Given an integer array with no duplicates.
    @return: The root of max tree.
    """
    def maxTree(self, A):
        # write your code here
        # import sys
        # sys.setrecursionlimit(100000 + 20)
        # print(len(A))
        segment_tree = [-1] * (len(A) * 2 + 2)
        print (segment_tree)
        print('hahaha')
        self.build_segment_tree(segment_tree, 1, 0, len(A) - 1, A)
        print(segment_tree) # segment_tree[i]: 
        return self.build_tree(A, 0, len(A) - 1, segment_tree)

    def build_tree(self, A, left, right, segment_tree):
        if left > right:
            return 
        index = self.find_max_index(segment_tree, 1 , 0, len(A) - 1, left, right, A)
        root = TreeNode(A[index])
        root.left = self.build_tree(A, left, index - 1, segment_tree)
        root.right = self.build_tree(A, index + 1, right, segment_tree)
        return root

    def build_segment_tree(self, segment_tree, index, start, end, A):
        # print("index " + str(index))
        # print("start: " + str(start))
        # print("end: " + str(end))
        if start == end:
            segment_tree[index] = start
            return

        mid = (start + end) // 2
        left_node = index * 2
        right_node = index * 2 + 1
        
        # build left and right subtree
        self.build_segment_tree(segment_tree, left_node, start, mid, A)
        self.build_segment_tree(segment_tree, right_node, mid + 1, end, A)
        # assign node value
        if A[segment_tree[left_node]] > A[segment_tree[right_node]]:
            segment_tree[index] = segment_tree[left_node]
        else:
            segment_tree[index] = segment_tree[right_node]

    def find_max_index(self, segment_tree, index, start, end, left, right, A):
        if start == left and end == right: 
            return segment_tree[index]

        mid = (start + end) // 2
        left_node = index * 2
        right_node = index * 2 + 1

        # if query range is on the left
        if right <= mid:
            return self.find_max_index(segment_tree, left_node, start, mid, left, right, A)
        if left >= mid + 1:
            return self.find_max_index(segment_tree, right_node, mid + 1, end, left, right, A)
        left_max_index = self.find_max_index(segment_tree, left_node, start, mid, left, right, A)
        right_max_index = self.find_max_index(segment_tree, right_node, mid + 1, end, left, right, A)
        if A[left_max_index] > A[right_max_index]:
            return left_max_index
        return right_max_index        
        
```