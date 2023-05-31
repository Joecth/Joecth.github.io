---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2022-09-12
---





## 2407. LIS II 

*hard*

Find the longest subsequence of `nums` that meets the following requirements:

- The subsequence is **strictly increasing** and
- The difference between adjacent elements in the subsequence is **at most** `k`.

Return *the length of the **longest** **subsequence** that meets the requirements.*

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

 

**Example 1:**

```
Input: nums = [4,2,1,4,3,4,5,8,15], k = 3
Output: 5
Explanation:
The longest subsequence that meets the requirements is [1,3,4,5,8].
The subsequence has a length of 5, so we return 5.
Note that the subsequence [1,3,4,5,8,15] does not meet the requirements because 15 - 8 = 7 is larger than 3.
```

**Example 2:**

```
Input: nums = [7,4,5,1,8,12,4,7], k = 5
Output: 4
Explanation:
The longest subsequence that meets the requirements is [4,5,8,12].
The subsequence has a length of 4, so we return 4.
```

**Example 3:**

```
Input: nums = [1,5], k = 1
Output: 1
Explanation:
The longest subsequence that meets the requirements is [1].
The subsequence has a length of 1, so we return 1.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i], k <= 105`



#### My Code

```python
class SegTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * n * 2  # 2 for boundary cases: 0 and n
        
    def query(self, left, right):   # on [left, right)
        l, r = left + self.n, right + self.n
        res = -sys.maxsize
        while l < r:
            if l & 1 == 1:
                res = max(res, self.tree[l])
                l += 1
            
            if r & 1 == 1:
                r -= 1  # because it's an open boundary
                res = max(res, self.tree[r])
            l >>= 1
            r >>= 1
        return res
        
        
    def update(self, i, val):
        idx = i + self.n
        self.tree[idx] = val
        while idx > 0:
            idx >>= 1
            # self.tree[idx] = max([self.tree[idx >> 1], self.tree[idx >> 1 | 1]])  # BUG!
            self.tree[idx] = max([self.tree[idx << 1], self.tree[idx << 1 | 1]])
            
            
class Solution:
    def lengthOfLIS(self, nums: List[int], k: int) -> int:
        # return self.my_20220912_numpymax(nums, k)  # AC! with numpy
        return self.my_20220912_segtree(nums, k)
    
    def my_20220912_segtree(self, nums, k):
        n = len(nums)
        seg = SegTree(max(nums) + 1) # max(nums) + 1 means the length of the dp, where dp[i] means LIS end with value 
        res = 0
        for i in range(n):
            x = nums[i]
            prev_max = seg.query(max(0, x-k), x)  # prev_max = dp[max(0, x-k):x]
            # print(i, nums[i], prev_max, seg.tree)
            seg.update(x, prev_max+1)   # dp[x] = prev_max + 1
            res = max(res, prev_max+1)
        return res
    
    def my_20220912_numpymax(self, nums, k):    # AC! once AC...
        n = len(nums)   
        dp = np.array([0] * (10 ** 5 + 2))
        # dp = np.array([0] * (max(nums) + 1))
        res = 0
        for i in range(n):
            x = nums[i]
            dp[x] = dp[max(0, x-k):x].max() + 1
            res = max(dp[x], res)
        
        return res            
```







Ref: 

1 https://leetcode.com/problems/longest-increasing-subsequence-ii/discuss/2560085/Python-Explanation-with-pictures-Segment-Tree

2 https://codeforces.com/blog/entry/18051

3 https://omeletwithoutegg.github.io/ in 中文