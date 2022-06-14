---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2022-02-28
---





# SortedList



## 632. Smallest Range Covering Elements from K Lists

- You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

  We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` **or** `a < c` if `b - a == d - c`.

   

  **Example 1:**

  ```
  Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
  Output: [20,24]
  Explanation: 
  List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
  List 2: [0, 9, 12, 20], 20 is in range [20,24].
  List 3: [5, 18, 22, 30], 22 is in range [20,24].
  ```

  **Example 2:**

  ```
  Input: nums = [[1,2,3],[1,2,3],[1,2,3]]
  Output: [1,1]
  ```

   

  **Constraints:**

  - `nums.length == k`

  - `1 <= k <= 3500`

  - `1 <= nums[i].length <= 50`

  - `-105 <= nums[i][j] <= 105`

  - `nums[i]` is sorted in **non-decreasing** order.

    

#### My Code

```python
from sortedcontainers import SortedList
class Solution:
    def smallestRange(self, nums: List[List[int]]) -> List[int]:
        # return self.my_TLE(nums):
        return self.my_20220226(nums)
    
    def my_20220226(self, nums):
        n = len(nums)
        cand = SortedList([])
        # cand = []
        for i in range(n):
            cand.add((nums[i][-1], i))
        
        ans = sys.maxsize
        res = []
        while cand:
            # cand.sort()
            # print(cand)
            rng = cand[-1][0] - cand[0][0]
            # ans = min(ans, rng)
            if rng <= ans:  # = for [3,3]  vs [1,1]
                res = [cand[0][0], cand[-1][0]]
                ans = rng
            
            _, idx = cand.pop()
            nums[idx].pop()
            # print(res)   
            if not nums[idx]: break
            # cand.append((nums[idx][-1], idx))
            cand.add((nums[idx][-1], idx))
        return res
    
    def my_TLE(self, nums):
        n = len(nums)
        # cand = SortedList([])
        cand = []
        for i in range(n):
            cand.append((nums[i][-1], i))
        
        ans = sys.maxsize
        res = []
        while cand:
            cand.sort()
            # print(cand)
            rng = cand[-1][0] - cand[0][0]
            # ans = min(ans, rng)
            if rng <= ans:  # = for [3,3]  vs [1,1]
                res = [cand[0][0], cand[-1][0]]
                ans = rng
            
            _, idx = cand.pop()
            nums[idx].pop()
            # print(res)   
            if not nums[idx]: break
            # if nums[idx]:
            cand.append((nums[idx][-1], idx))
            # cand.add((nums[idx][-1], idx))
        return res
```





## 1675. Minimize Deviation in Array

You are given an array `nums` of `n` positive integers.

You can perform two types of operations on any element of the array any number of times:

- If the element is even, divide it by 2,
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the last element, and the array will be `[1,2,3,2].`
- If the element is odd, multiply it by 2.
  - For example, if the array is `[1,2,3,4]`, then you can do this operation on the first element, and the array will be `[2,2,3,4].`

The **deviation** of the array is the **maximum difference** between any two elements in the array.

Return *the **minimum deviation** the array can have after performing some number of operations.*

 

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 1
Explanation: You can transform the array to [1,2,3,2], then to [2,2,3,2], then the deviation will be 3 - 2 = 1.
```

**Example 2:**

```
Input: nums = [4,1,5,20,3]
Output: 3
Explanation: You can transform the array after two operations to [4,2,5,5,3], then the deviation will be 5 - 2 = 3.
```

**Example 3:**

```
Input: nums = [2,10,8]
Output: 3
```

 

**Constraints:**

- `n == nums.length`
- `2 <= n <= 105`
- `1 <= nums[i] <= 109`



### My Code

```python
from sortedcontainers import SortedList
class Solution:
    def minimumDeviation(self, nums: List[int]) -> int:
        # return self.my_TLE(nums)
        return self.my_20220223(nums)
    
        
    def my_20220223(self, nums):
        nums = SortedList(list(map(lambda x: 2 * x if x % 2 == 1 else x, nums)))
        n = len(nums)
        
        ans = sys.maxsize
        while True:
            # nums.sort()
            rng = nums[-1] - nums[0]
            ans = min(ans, rng)
            if nums[-1] % 2 == 1:
                break
            else:
                tmp = nums.pop()
                tmp //= 2
                nums.add(tmp)
                # nums[-1] //= 2
        return ans
        
    def my_TLE(self, nums):
        nums = list(map(lambda x: 2 * x if x % 2 == 1 else x, nums))
        n = len(nums)
        
        ans = sys.maxsize
        while True:
            nums.sort()
            rng = nums[-1] - nums[0]
            ans = min(ans, rng)
            if nums[-1] % 2 == 1:
                break
            else:
                nums[-1] //= 2
            # print(rng, nums)
        return ans
                
        # # print(nums)
        # nums.sort()
        # while nums[-1] % 2 == 0:
        #     hi = nums.pop()
        #     nums += [hi // 2]
        #     nums = sorted(set(nums))
        # return nums[-1] - nums[0]
        # # while 
```