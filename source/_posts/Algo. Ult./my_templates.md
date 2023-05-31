---
layout: post
categories: AlgoUlt.
tag: []
date: 2022-01-31
Author: Jo
---



## Quick-Select

```python
        return self.congcong9p_20220718(nums, 0, len(nums)-1, k-1)
    
    def congcong9p_20220718(self, nums, left, right, k):
        if left == right:
            return nums[left]
        
        n = len(nums)
        # lo, hi =  0, n - 2
        pivot = nums[right]
        lo, hi = left, right
        while lo <= hi:
            # mid = lo + (hi - lo) // 2
            while lo <= hi and nums[lo] > pivot:
                lo += 1
            while lo <= hi and nums[hi] < pivot:
                hi -= 1
            
            if lo <= hi:
                nums[lo], nums[hi] = nums[hi], nums[lo]
                lo += 1
                hi -= 1
        # print(nums, lo, hi)
        if k <= hi:
            return self.congcong9p_20220718(nums, left, hi, k)
        if k >= lo:
            return self.congcong9p_20220718(nums, lo, right, k)
        return nums[k]
```



```python
        return self.my_20220719(nums, k)
    
    def my_20220719(self, nums, k):
        n = len(nums)
        p_idx = -1    # tells us which position is correct now
        lo, hi = 0, n - 1
        while p_idx != k - 1:
        # while lo < hi:
            p_idx = self.partition_20220719(nums, lo, hi) # LOMU
            # print(nums, p_idx)
            if k - 1 < p_idx:
                hi = p_idx - 1
            elif k - 1 > p_idx:
                lo = p_idx + 1
            
        return nums[k - 1]
        
        
    def partition_20220719(self, nums, lo, hi):
        i = lo - 1  # i: last pos, whose val > pivot
        pivot = nums[hi]
        for j in range(lo, hi + 1): # j: trvsing index
            if nums[j] > pivot:
                i += 1
                nums[i], nums[j] = nums[j], nums[i]
        nums[i+1], nums[hi] = nums[hi], nums[i+1]
        return i + 1    # i+1 大於 pivot的人的個數
```



3 1 5 7 4

​      4

  



##### More:

[Lomuto VS Hoare](https://www.freecodecamp.org/news/quickselect-algorithm-explained-with-examples/)

[LC215](https://leetcode.com/problems/k-closest-points-to-origin/discuss/578663/quick-select-same-template-to-solve-215-on)



### Special: Partition in List, LC-86

```python
class Solution:
    def partition(self, head, x):
        return self.my_20220722(head, x)
    
    def my_20220722(self, head, x):
        i = left_head = ListNode(0)
        j = right_head = ListNode(0)
        cur = head
        while cur:
            if cur.val < x:
                # now = i
                i.next = cur
                i = i.next
            else:
                # now = j
                j.next = cur
                j = j.next
            # now.next = cur    # BUGGG
            # now = now.next

            cur = cur.next

        i.next = right_head.next
        j.next = None
        return left_head.next
```



## Sieve of Eratosthenes

```python
from math import isqrt

class Solution:
    def countPrimes(self, n: int) -> int:
        # n -= 1
        if n <= 1:
            return 0
        is_prime = [True] * n
        is_prime[0] = is_prime[1] = False
        """
        2 3 4 5 6..  16 17  35
                X X XXX
                            5.9 < 7
              5 or 6 7 8 9 
                            5
        """
        # for i in range(2, n):#isqrt(n + 1) + 1):
        for i in range(2, isqrt(n) + 1):    # OPTIMIZATION!
            if is_prime[i]:
                # for x in range(2, n):     # TLE
                    # if x*i > n-1:
                    #     break
                    # is_prime[x*i] = False    
                    
                """ OPTIMIZATION BELOW: """
                for x in range(i*i, n, i):       # OPTIMIZATION!
                    is_prime[x] = False

        # print(is_prime)
        return sum(is_prime)
            
        
    
```



## Hungary 

### Max Invitation

```python
class Solution:
    def maximumInvitations(self, grid: List[List[int]]) -> int:
        # return self.my_20220719(grid)   # with EF (bfs), ref: https://www.youtube.com/watch?v=OViaWp9Q-Oc
        return self.my_20220719_FF(grid)  # ref: https://leetcode.com/problems/maximum-number-of-accepted-invitations/discuss/1978859/Python-or-Hungarian-Algorithm-easy-to-understand-%3A)
    
    def my_20220719_FF(self, grid):
        m, n = len(grid), len(grid[0])
        matches = {}
        for boy in range(m):
            self.dfs_20220719(grid, boy, set([]), matches)
        return len(matches)
            
    def dfs_20220719(self, grid, boy, visited, matches):
        for girl in range(len(grid[0])):
            if grid[boy][girl] and girl not in visited:
                visited.add(girl)
                
                if girl not in matches or self.dfs_20220719(grid, matches[girl], visited, matches):
                    matches[girl] = boy
                    return True
        return False
```





## SkipList 

### 792. Number of Matching Subsequences

```python
        return self.my_20220720(S, words)
        return self.my_20220724(S, words)
    
    def my_20220724(self, s, words):
        s = '#' + s
        
        next_s_char = [[-1] * 26 for _ in range(len(s))]
        '''TODO!'''
        for i in range(len(s) - 2, -1, -1):
            next_s_char[i] = next_s_char[i + 1].copy()
            pos = ord(s[i + 1]) - ord('a')
            next_s_char[i][pos] = i + 1
        
        ans = 0
        for word in words:
            if self.is_subseq_20220724(word, s, next_s_char):
                ans += 1
        return ans
    
    def is_subseq_20220724(self, word, s, next_s_char):
        
        i = j = 0
        while i < len(s) and j < len(word):
            nxt_pos = next_s_char[i][ord(word[j]) - ord('a')]
            if nxt_pos == -1:
                return False
            i = nxt_pos
            j += 1  # j needs to be completely matched!
        
        # if j == len(word):    # MUST be!
        return True
    def my_20220720(self, s, words):
        mapping = defaultdict(list)
        for i in range(len(s)):
            mapping[s[i]].append(i)
        # print(mapping)
        cnt = 0
        # pos = [-1] * len(words)
        m, n = len(s), len(words)
        # for i in range(n):
        for i in range(n):
            word = words[i]
            # prev_pos = pos[i]
            prev_pos = -1
            # for j in range(len(word)):
            j = 0
            while j < len(word):
                searching_arr = mapping[word[j]]
                # TO KNOW: in searching_arr, if there elem's value > prev_pos(left hanside char)
                idx = self.upper_bound(searching_arr, prev_pos)
                # print(searching_arr, i, j, idx, prev_pos)
                # if idx == len(word[j]): # BUGGG no val is larger than me
                if idx == len(searching_arr): # FIX
                    break
                # prev_pos = idx    # BUGGG!!
                prev_pos = searching_arr[idx]    # BUGGG!!
                j += 1
            # print(i, j)
            if j == len(word):
                cnt += 1
        return cnt
    
    def upper_bound(self, arr, target):
        lo, hi = 0, len(arr)
        while lo < hi:
            mid = lo + (hi - lo) // 2
            if arr[mid] > target:
                hi = mid
            else:
                lo = mid + 1
        return lo
```



### 1055. Shortest Way to Form String

```python
    def shortestWay(self, source, target):
        """
        :type source: str
        :type target: str
        :rtype: int
        """
        if not source: return -1
        # return self.my_sol(source, target)
        # return self.sol_sisi(source, target)
        return self.my_20220721(source, target)
    
    def my_20220721(self, source, target):
        source = '#' + source
        m, n = len(source), len(target)
        next_s_char = [[-1] * 26 for _ in range(len(source))]
        for i in range(len(source) - 2, -1, -1):
            next_s_char[i] = next_s_char[i + 1].copy()
            pos = ord(source[i + 1]) - ord('a')
            next_s_char[i][pos] = i + 1
        
        i = 0
        # ans = 0   # BUG
        ans = 1
        # for j in range(len(target)):  # BUGG!
        j = 0
        while j < len(target):
            idx = ord(target[j]) - ord('a')
            next_pos = next_s_char[i][idx]
            # print(j, i, idx, next_pos)

            if next_pos == -1:  # needs loop again!
                """ CRITICAL!! """
                # print(next_pos, i)
                if i == 0:
                    return -1
                
                ans += 1
                i = 0
            else:
                i = next_pos
                j += 1
        return ans
```



## Ultimate B-search

### 658. K Closest

```python
        # return self.my_20220720(arr, k, x)    # AC
        return self.my_20220720_B(arr, k, x)    # AC
    
    def my_20220720_B(self, arr, k, x): # TODO: discuss OOXXXX
        n = len(arr)
        if k >= n:
            return arr
        
        lo, hi = 0, n - k   # starting pos of the k-leng subarray
        while lo < hi:
            mid = lo + (hi - lo) // 2
            if arr[mid + k] - x >= x - arr[mid]:
                hi = mid
            else:
                lo = mid + 1
        return arr[lo:lo + k]
            
            
    
    def my_20220720(self, arr, k, x):
        n = len(arr)
        if k >= n:
            return arr
        
        lo, hi = 0, n - k   # starting pos of the k-leng subarray
        while lo + 1 < hi:
            print(lo, hi)
            mid = lo + (hi - lo) // 2
            print(mid)
            # if abs(arr[mid] - x) <= abs(arr[mid+k] - x):    # BUGGG!
            # if x - arr[mid] <= arr[mid + k] - x:            # CORRECT FIX!
            if arr[mid + k] - x >= x - arr[mid]:
                hi = mid
            else:
                lo = mid
        print(lo, hi)
        
        head = abs(arr[lo] - x)
        tail = abs(arr[lo+k] - x)
        if head <= tail:
            return arr[lo:lo+k]
        return arr[hi:hi+k]
```



## Target 出現次數 --> 超大array

- Partition w/ 100 as the partition_size, then GroupBy





## K distance apart

```python
class Solution:
    def rearrangeString(self, s: str, k: int) -> str:
        if k <= 1 or len(s) <= 1:
            return s
        # return self.sol_w_arr(s, k) # still FAILED
        # return self.solution_0921_A(s, k) # 112ms
        # return self.solution_0921_B(s, k)   # 48ms
        return self.my_20220726(s, k)
    
    def my_20220726(self, s, k):
        n = len(s)
        max_heap = []
        [heapq.heappush(max_heap, (-v, k)) for k, v in Counter(s).items()]
        # if -max_heap[0][0] > n // k:
        #     return ''
        count = Counter(s)
        maxf = -max_heap[0][0]
        if (maxf - 1) * k + list(count.values()).count(maxf) > len(s):
            return ""
        
        
        res = [''] * n
        for i in range(0, n, k):
            using = []
            for j in range(k):
                if max_heap:
                    cnt, char = heapq.heappop(max_heap)
                    cnt = abs(cnt)
                    using.append([cnt - 1, char])
                    res[i + j] = char
                else:
                    break
                
            for new_cnt, char in using:
                if new_cnt > 0:
                    heapq.heappush(max_heap, (-new_cnt, char))
        
        return ''.join(res)
```



## Kth subarray sum

```python
class Solution:
    def kthSmallestSubarraySum(self, nums: List[int], k: int) -> int:
        # return self.congcong(nums, k)
        return self.my_20220802(nums, k)

    def my_20220802(self, nums, k):
        nums.sort()
        # print(nums)
        n = len(nums)
        lo, hi = min(nums), sum(nums)
        while lo + 1 < hi:  # binary answer!
            mid = lo + (hi - lo) // 2
            # print(lo, hi, mid)
            # print(lo, hi, mid)
            if self.getSmallerOrEqualCount(nums, mid) >= k - 1:
                hi = mid
            else:
                lo = mid
        # print(lo, hi)
        if self.getSmallerOrEqualCount(nums, lo) == k - 1:
            return lo
        return hi
    
    def getSmallerOrEqualCount(self, nums, target):
        n = len(nums)
        tot = n * (n + 1) // 2
        now = 0
        cnt = 0
        i = 0
        fin = False
        for j in range(n):
            now += nums[j]
            while now > target:
                # cnt += (n - 1 - (j - 1))
                cnt += n - j
                now -= nums[i]
                i += 1
            #     if i > j:     # BUGGGGG!!!!! 不可不可!!
            #         fin = True
            # if fin:
            #     break
            
        # print(cnt)
        return tot - cnt # return tot - #(sub array sum > than target)
```



## Dijkastra or Kruskal

### 1631. Path With Minimum Effort

```python
from collections import deque
import heapq

class UF:
    def __init__(self, n):
        self.parents = [i for i in range(n)]
    def find(self, x):
        if x != self.parents[x]:
            self.parents[x] = self.find(self.parents[x])
        return self.parents[x]
    def merge(self, x, y):
        px, py = self.find(x), self.find(y)
        if px != py:
            self.parents[py] = px
    def is_connected(self, x, y):
        return self.find(x) == self.find(y)

DIRECTIONS = [(-1, 0), (0, 1), (1, 0), (0, -1)]
class Solution:
    def minimumEffortPath(self, A: List[List[int]]) -> int:
        # return self.my_20220207(A)    # AC, sol MST
        return self.my_20220803(A)
    
    def my_20220803(self, A):
        m, n = len(A), len(A[0])
        END = (m - 1, n - 1)
        min_heap = [(0, 0, 0)]
        visited = {(0, 0): 0}
        while min_heap:
            # print(min_heap)
            effort, i, j = heapq.heappop(min_heap)
            if (i, j) == END:
                return visited[END]
            for di, dj in DIRECTIONS:
                ni, nj = i + di, j + dj
                """ BUGGG!!!
                if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in visited:
                    effort = abs(A[ni][nj] - A[i][j])
                    visited[(ni, nj)] = max(effort, visited[(i, j)])
                    heapq.heappush(min_heap, (max(effort, visited[(i, j)]), ni, nj))
                    """
                if 0 <= ni < m and 0 <= nj < n:
                    next_effort = abs(A[ni][nj] - A[i][j])
                    # if next_effort > effort:    # BUG, No need
                    #     path_max_effort = max(next_effort, effort)
                    path_max_effort = max(next_effort, effort)
                    if (ni, nj) not in visited:
                        visited[(ni, nj)] = path_max_effort
                    else: 
                        if visited[(ni, nj)] > path_max_effort:  # 曾經發生的路上的最大如果大於effort的話；因為我們要的是 min(max)
                            visited[(ni, nj)] = path_max_effort
                        else:
                            continue
                    heapq.heappush(min_heap, (path_max_effort, ni, nj))            
        return -1    
    
    def my_20220803_WRONG(self, A):
        m, n = len(A), len(A[0])
        END = (m - 1, n - 1)
        min_heap = [(0, 0, 0)]
        visited = {(0, 0): 0}
        while min_heap:
            # print(min_heap)
            effort, i, j = heapq.heappop(min_heap)
            if (i, j) == END:
                return visited[END]
            for di, dj in DIRECTIONS:
                ni, nj = i + di, j + dj
                """ BUGGG!!!
                if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in visited:
                    effort = abs(A[ni][nj] - A[i][j])
                    visited[(ni, nj)] = max(effort, visited[(i, j)])
                    heapq.heappush(min_heap, (max(effort, visited[(i, j)]), ni, nj))
                    """
                if 0 <= ni < m and 0 <= nj < n:
                    next_effort = abs(A[ni][nj] - A[i][j])
                    if (ni, nj) not in visited:
                        visited[(ni, nj)] = max(next_effort, effort)
                    else: 
                        if visited[(ni, nj)] > next_effort:  # 曾經發生的路上的最大如果大於effort的話；因為我們要的是 min(max)
                            visited[(ni, nj)] = next_effort
                        else:
                            continue
                    heapq.heappush(min_heap, (next_effort, ni, nj))            
        return -1
        
    
    def my_20220207(self, A):
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        m, n = len(A), len(A[0])
        # heap = [(-A[0][0], 0, 0), (-A[m - 1][n - 1], m - 1, n - 1)]   # no need
        heap = []
        # res = sys.maxsize 不需要
        for i in range(m):
            for j in range(n):
                if i < m - 1:
                    heapq.heappush(heap, (abs(A[i][j] - A[i + 1][j]), i, j, i + 1, j))
                if j < n - 1:
                    heapq.heappush(heap, (abs(A[i][j] - A[i][j + 1]), i, j, i, j + 1))
        # print(len(heap))
        # print(sorted(heap))
        
        uf = UF(m * n)
        seen = set([])
        src = 0
        dst = (m - 1) * n + (n - 1)
        res = sys.maxsize
        while heap:
            val, i1, j1, i2, j2 = heapq.heappop(heap)
            # seen.add((i1, j1))
            # seen.add((i2, j2))
            a, b = i1 * n + j1, i2 * n + j2
            uf.merge(a, b)
            # print(val, i1, j1, i2, j2)
            
            # now = i * n + j
            # for di, dj in dire:
            #     ni, nj = i + di, j + dj
            #     # if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in seen:
            #     if 0 <= ni < m and 0 <= nj < n and (ni, nj) in seen:
            #         nbr = ni * n + nj
            #         uf.merge(now, nbr)
            #         # seen.add((ni, nj))    BUG! shouldn't be here
            # print(uf.parents)
            if uf.is_connected(src, dst):
                return val            
        return 0
        
```





## Memo

### 329. Longest Increasing Path in a Matrix

Given an `m x n` integers `matrix`, return *the length of the longest increasing path in* `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).

 

**Example 1:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h4xctvhrvdj206q06qdfr.jpg)

```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

**Example 2:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h4xctw3izaj2071071t8n.jpg)

```
Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
Output: 4
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
```

**Example 3:**

```
Input: matrix = [[1]]
Output: 1
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `0 <= matrix[i][j] <= 231 - 1`

```python
DIR = [[-1, 0], [0, 1], [1, 0], [0, -1]]
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        memo = [[-1] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                self.dfs(matrix, i, j, memo)
        return max(max(row) for row in memo)
        
    def dfs(self, matrix, i, j, memo):
        if not (0 <= i < len(matrix)) or not (0 <= j < len(matrix[0])):
            return 0
        if memo[i][j] > -1:
            return memo[i][j]
        
        longest = 0
        for di, dj in DIR:
            ni, nj = i + di, j + dj
            
            if 0 <= ni < len(matrix) and 0 <= nj < len(matrix[0]) and matrix[ni][nj] > matrix[i][j]:
                longest = max(self.dfs(matrix, ni, nj, memo), longest)

        memo[i][j] = longest + 1
        return memo[i][j]
    '''
    0   0   2
            
    2   2   1(1, 2)
    3   4   2
    '''
```





## Shortest Path

1293.



743.

### Dijkastra V.S. A*

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        import heapq
        
        graph = defaultdict(dict)
        for i,j,w in times:
            graph[i][j] = w
            
        dist = [float("inf")]*(n+1)
        dist[k] = 0
        que = [(0,k)] 
        visited = [False]*(n+1)
        
        while que:
            d, node = heapq.heappop(que)
            
            if visited[node]:
                continue

            visited[node] = True
            
            for child in graph[node]:
                if graph[node][child] + d < dist[child]:
                    dist[child] = graph[node][child] + d
                    heapq.heappush( que, (dist[child],child) )
                    
        ret = max(dist[1:])
        return ret if ret!=float("inf") else -1
```



## Rearrange String k Dist

### 358. Rearrange String k Distance Apart

```python
import heapq
from collections import Counter
class Solution:
    def rearrangeString(self, s: str, k: int) -> str:
        if k <= 1 or len(s) <= 1:
            return s
        # return self.sol_w_arr(s, k) # still FAILED
        # return self.solution_0921_A(s, k) # 112ms
        # return self.solution_0921_B(s, k)   # 48ms
        return self.my_20220726(s, k)
    
    def my_20220726(self, s, k):
        n = len(s)
        max_heap = []
        [heapq.heappush(max_heap, (-v, k)) for k, v in Counter(s).items()]
        # if -max_heap[0][0] > n // k:
        #     return ''
        count = Counter(s)
        maxf = -max_heap[0][0]
        if (maxf - 1) * k + list(count.values()).count(maxf) > len(s):	# 最多次就是這個，不然就是 len(tasks), ref to LC621.
            return ""
        
        
        res = [''] * n
        for i in range(0, n, k):
            using = []
            for j in range(k):
                if max_heap:
                    cnt, char = heapq.heappop(max_heap)
                    cnt = abs(cnt)
                    using.append([cnt - 1, char])
                    res[i + j] = char
                else:
                    break
                
            for new_cnt, char in using:
                if new_cnt > 0:
                    heapq.heappush(max_heap, (-new_cnt, char))
        
        return ''.join(res)
```



## Deserialization

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        que = deque([root])
        res = ''
        while que:
            now = que.popleft()
            res += str(now.val) if now else '#'
            res += ' '
            if not now:
                continue

            que.append(now.left)
            que.append(now.right)
        # print(res)
        return res
    
    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        # root = TreeNode
        vals = data.split(' ')
        nodes = [TreeNode(val) if val != '#' else None for val in vals]
        # [print(n) for n in nodes]
        
        slow, fast = 0, 1
        # print('START!')
        while fast < len(nodes):
            now = nodes[slow]
            if now:
                now.left = nodes[fast]
                if fast + 1 < len(nodes):
                    now.right = nodes[fast + 1]
                fast += 2
            # print(now, now.left, now.right)
            slow += 1
            # fast += 2 # BUGG!
        return nodes[0]
            
# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```



## Longest Cycle in a Graph

### 2360. Longest Cycle in a Graph

```python
class Solution:
    def longestCycle(self, edges: List[int]) -> int:
        return self.shangan_lc304(edges)
        
    def shangan_lc304(self, edges):
        n = len(edges)
        
        # graph = {}
        # for i in range(n):
        #     graph[i] = []
        # for i in range(n):
        #     graph[i].append(edges[i])
            
        time_stamp = [0] * n
        res = -1
        cur = 1       # FIX
        for i in range(n):  # as starting point!
            # cur =     # BUG
            if time_stamp[i] > 0:
                continue
                
            j = i
            start = cur   # FIX
            while j >= 0:
                # start = cur   # BUGG
                if time_stamp[j] > 0:   # FOUND a LOOP!
                    if time_stamp[j] >= start:
                        res = max(res, cur - time_stamp[j])
                        print(cur - time_stamp[j])
                    break
                time_stamp[j] = cur
                j = edges[j]
                cur += 1
        return res
```





## Tail DP

### 2214. Minimum Health to Beat Game 

##### P.S. Best solution to this problem is Greedy instead of this DP. However, this DP brings valuable ideas

```python
class Solution:
    def minimumHealth(self, damage: List[int], armor: int) -> int:
        n = len(damage)
        use = [0] * (n + 1)
        no_use = [0] * (n + 1)
        
        use[n] = 1
        no_use[n] = 1
        
        """
        use[i]: 第i天開始的時候，那天「會用」armor的情況下，允許的最少Health
        no_use[i]: 第i天開始的時候，那天「不會用」armor的情況下，允許的最少Health
        """
        for i in range(n - 1, -1, -1):
            # use_on_i_day = use[i + 1] + max(damage[i] - armor, 0) # BUG
            use_on_i_day = no_use[i + 1] + max(damage[i] - armor, 0)
            use_before_i_day = use[i + 1] + damage[i]
            use[i] = min(use_on_i_day, use_before_i_day)
            no_use[i] = no_use[i + 1] + damage[i]
        return use[0]
```





## State DP

### 2222. Number of Ways to Select Buildings

##### P.S. can also be solved with L/R partition method

```python
from collections import defaultdict
class Solution:
    def numberOfWays(self, s: str) -> int:
        # return self.stateDP_20220815(s) # AC!
        return self.greedy_20220815(s)  # AC!
    
    def greedy_20220815(self, s):
        n = len(s)
        left_ones = [0] * n
        left_zeros = [0] * n
        right_ones = [0] * n
        # cnt = 0
        for i in range(1, n):
            left_ones[i] = left_ones[i - 1] + (s[i - 1] == '1')
            left_zeros[i] = left_zeros[i - 1] + (s[i - 1] == '0')
        right_zeros = [0] * n
        for i in range(n - 2, -1, -1):
            right_ones[i] = right_ones[i + 1] + (s[i + 1] == '1')
            right_zeros[i] = right_zeros[i + 1] + (s[i + 1] == '0')
            
        ans_ones = 0
        ans_zeros = 0
        for i in range(1, n - 1):
            if s[i] == '1':
                ans_ones += left_zeros[i] * right_zeros[i]
            else:
                ans_zeros += left_ones[i] * right_ones[i]
        return ans_ones + ans_zeros
                
    
    def stateDP_20220815(self, s):
        n = len(s)
        dp = defaultdict(int)
        for i in range(n):
            if s[i] == '0':
                dp['0'] += 1
                dp['10'] += dp['1']
                dp['010'] += dp['01']
            else:
                dp['1'] += 1
                dp['01'] += dp['0']
                dp['101'] += dp['10']
        return dp['010'] + dp['101']
        # state DP https://leetcode.com/problems/number-of-ways-to-select-buildings/discuss/1907109/PythonDP-easy-to-understand
        
```



### 1048. Longest String Chain

```python
        return self.my_20220803(sorted(words, key=lambda w: len(w)))
    
    def my_20220803(self, words):
        n = len(words)
        # que = deque([(words[0], 1)])
        seen = {}

        for i in range(len(words)):
            word = words[i]
            for j in range(len(word)):
                # prev_state = word[:i] + word[i+1:]        # BUGGG!
                prev_state = word[:j] + word[j+1:]        # FIX
                # print(word, prev_state)
                if prev_state in seen:
                    # seen[word] = seen[prev_state] + 1     # BUGGG!
                    seen[word] = max(seen.get(word, 0), seen[prev_state] + 1)     # FIX
            if word not in seen:
                seen[word] = 1
        # print(seen)
        return max(seen.values())
```





## Weird DP

### 552. Student Attendance Record II





## Tree Parent Path

### 2385. Amount of Time for Binary Tree to Be Infected

```python
from collections import deque
class Solution:
    def amountOfTime(self, root: Optional[TreeNode], start: int) -> int:
        return self.my_20220821(root, start)
    
    def my_20220821(self, root, start):
        parents = {root:None}
        ret = [None]
        self.buildParentAndGetStart(root, start, parents, ret)
        start_node = ret[0]
        # print(start_node.val)
        
        que = deque([start_node])
        visited = set([start_node])
        # visited[start_node]
        steps = 0
        while que:
            size = len(que)
            next_que = deque([])
            for _ in range(size):
                now = que.popleft()
                
                # 1 path for children
                for child in [now.left, now.right]:
                    # if child: # BUG!
                    if child and child not in visited: # FIX!
                        next_que.append(child)
                        visited.add(child)
                
                # 2 path for parent
                par = parents[now]
                if par and par not in visited:
                    next_que.append(par)
                    visited.add(par)
            
            # [print(x.val) for x in next_que]   #for debug
            que = next_que
            steps += 1
        return steps - 1
        
    def buildParentAndGetStart(self, root, start, parents, ret):
        if not root:
            return 
        
        if root.val == start:
            ret[0] = root
        
        for child in [root.left, root.right]:
            if child:
                parents[child] = root
                self.buildParentAndGetStart(child, start, parents, ret)
```





## BT or Special Graph

### 2242. Maximum Score of a Node Sequence

```python
# import heapq
class Solution:
    def maximumScore(self, scores: List[int], edges: List[List[int]]) -> int:
        # return self.my_TLE(scores, edges)
        return self.my_20220830(scores, edges)  # AC!
    
    def my_20220830(self, scores, edges):
        graph = {}
        n = len(scores)
        for i in range(n):
            graph[i] = []
        for a, b in edges:
            graph[a].append((scores[b], b))  # TODO: optimize with heap!
            graph[b].append((scores[a], a))
        
        for i in range(n):
            graph[i].sort(reverse=True)
        
        # res = 0   # BAD
        res = -1   # FIX
        for u, v in edges:
            # for u_nbr_score, u_nbr in graph[u]:   # TLE!
            for i in range(min(3, len(graph[u]))):  # FIX!!!
                u_nbr_score, u_nbr = graph[u][i]
                # for v_nbr_score, v_nbr in graph[v]:   # TLE!
                for j in range(min(3, len(graph[v]))):  # FIX!!!
                    v_nbr_score, v_nbr = graph[v][j]
                    if v != u_nbr and u != v_nbr and u_nbr != v_nbr:
                        # print(u, v, u_nbr, v_nbr)
                        now = scores[u] + scores[v] + scores[u_nbr] + scores[v_nbr]
                        res = max(now, res)
        return res

    
    
    def my_TLE(self, scores, edges):
        '''
        1 build graph
        2 start from every node with for loop, then apply know-down strategy
        '''
        graph = {}
        n = len(scores)
        for i in range(n):
            graph[i] = []
        for a, b in edges:
            graph[a].append(b)
            graph[b].append(a)
        
        n = len(scores)
        ans = -1
        for i in range(n):
            now = [-1]
            # self.dfs(graph, i, [], now)   # BUG!, should still need visited
            # self.dfs(graph, scores, i, set(), now)    # WRONG! bottom-up not able to solve this；因為雖然可以把分數往上傳，但是傳上去時，無法知道過來的是不是長度為4這時才傳上來的
            # print(i, now)
            self.bt(graph, scores, i, set([i]), scores[i], now)
            ans = max(ans, now[0])
        return ans
```



## Bottom-up FAILED

### 2242. Maximum Score of a Node Sequence ↑



## Servers

### 1606. Find Servers That Handled Most Number of Requests

```python
        # return self.sol_ye15(k, arrival, load)    # SAME as Mine, which helps me to debug
        return self.my_20220830(k, arrival, load)   # AC
    
    def my_20220830(self, k, arrival, load):
        n = len(arrival)    
        
        # ith request --> record it's ending time
        # time = 1  # no need
        idles = SortedList([i for i in range(k)])   # idxes
        busy = []   # min_heap; (fin_time, idx)
        req = [0] * k
        for i in range(n):
            while busy and busy[0][0] <= arrival[i]:
                _, idx = heapq.heappop(busy)
                # heapq.heappush(idles, idx)    # BUG
                idles.add(idx)  # FIX!
            # print(busy, idles)
            if not idles:
                continue    
            # if idles:
            idx = idles.bisect_left(i % k) #% len(idles)
            # if idx == n:  # BUG!!
            if idx == len(idles):
                idx = 0
            """ CRITICAL BUG: idx 跟 i都不是真正在用的server；　在用的是 idles[idx]!!!
            # heapq.heappush(busy, (arrival[i] + load[i], i))   # BUG
            # heapq.heappush(busy, (arrival[i] + load[i], idx))   # STILL BUG!
            # idles.remove(idles[idx])    # CRITICAL!
            # req[idx] += 1 # BUGG!
            """
            """ FIX below! """
            server = idles[idx]
            heapq.heappush(busy, (arrival[i] + load[i], server))   # FIX
            req[server] += 1 # FIXX!
            
            idles.pop(idx)    # CRITICAL!
        # print(req)
        max_count = max(req)
        # print(max_count)
        return [i for i in range(k) if max_count == req[i]]
```



## Merge 2 Sorted Intervals

```python
class Solution:
    """
    @param list1: one of the given list
    @param list2: another list
    @return: the new sorted list of interval
    """
    def mergeTwoInterval(self, list1, list2):
        # write your code here
        result = []
        if list1 is None or list2 is None:
            return result
        if len(list1) == 0:
            return list2
        if len(list2) == 0:
            return list1
            
        i, j = 0, 0
        while i < len(list1) and j < len(list2):
            if list1[i].start <= list2[j].start:
                self.add_to_list(list1[i], result)
                i += 1
            else:
                self.add_to_list(list2[j], result)
                j += 1
        
        while j < len(list2):
            self.add_to_list(list2[j], result)
            j += 1
        
        while i < len(list1):
            self.add_to_list(list1[i], result)
            i += 1
        
        return result
    
    def add_to_list(self, interval, result):
        if len(result) == 0:
            result.append(interval)
            return
            
        if interval.start <= result[-1].end:
            result[-1].end = max(result[-1].end, interval.end)
        else:
            result.append(interval)
```



## My Inheritance 

```python
class RLEIterator:
    def __init__(self, encoding: List[int]):
        # self.obj = RLEIterator_TLE(encoding)
        self.obj = RLEIterator_FIX(encoding)
        
    def next(self, n: int) -> int:
        return self.obj.next(n)
        
class RLEIterator_FIX:
    def __init__(self, encoding: List[int]):
        self.encoding = encoding
        self.it = 0
        

    def next(self, n: int) -> int:
        remaining = n
        for i in range(self.it, len(self.encoding), 2):
            times, elem = self.encoding[i], self.encoding[i + 1]
            # remaining -= min(times, remaining)
            # if remaining >= times:   # BUGGG
            if remaining > times:   # FIX
                remaining -= times
                self.encoding[i] = 0 
                # return elem
            else:
                self.encoding[i] = times - remaining
                self.it = i
                return elem
        return -1
        
            
        # self.idx += n
        # ret = -1
        # if self.idx < len(self.arr):
        #     ret = self.arr[self.idx]
        # return ret
        
class RLEIterator_TLE:
    def __init__(self, encoding: List[int]):
        self.arr = []
        for i in range(0, len(encoding), 2):
            times, elem = encoding[i], encoding[i + 1]
            self.arr.extend([elem] * times)
        # print(self.arr)
        self.idx = -1

    def next(self, n: int) -> int:
        self.idx += n
        ret = -1
        if self.idx < len(self.arr):
            ret = self.arr[self.idx]
        return ret
```



## TLE Hash, solved by Rabin-Karp

```python
class Solution:
    def differByOne(self, dict: List[str]) -> bool:
        n, m = len(dict), len(dict[0])
        hashes = [0] * n
        MOD = 10**11 + 7
        
        for i in range(n):
            for j in range(m):
                hashes[i] = (26 * hashes[i] + (ord(dict[i][j]) - ord('a'))) % MOD
        
        base = 1
        for j in range(m - 1, -1, -1):        
            seen = set()
            for i in range(n):
                new_h = (hashes[i] - base * (ord(dict[i][j]) - ord('a'))) % MOD
                if new_h in seen:
                    return True
                seen.add(new_h)
            base = 26 * base % MOD
        return False
      	# https://leetcode.com/problems/strings-differ-by-one-character/discuss/801825/Python-Clean-set-%2B-string-hashing-solution-from-O(NM2)-to-O(NM)
```



## Using Defaultdict as an Array alternative

### 2400. Number of Ways to Reach a Position After Exactly k Steps

*p.s. compare with Lintcode 1827* 

```python
MOD = 10 **9 + 7
from collections import deque, defaultdict
import math

class Solution:
    def numberOfWays(self, startPos: int, endPos: int, k: int) -> int:
        """ 
        dp[kdx] is a defaultdict --> -10, -9, -1, 0, 1, 2
        dp[kdx][j]
        
                    核心就是一个「Pair!」
        dp[kdx] --> tuple             (pos, 方案数)  <== 不行！會無法 lookup!
                --> defaultdict(int)[pos] --> 方案数
        """
        dp = {} # 
        FAR = startPos + k
        FAR_NEG = startPos - k
        for kdx in range(k + 1):
            dp[kdx] = defaultdict(int)  # pos --> to 方案數
        dp[0][startPos] = 1 # dp i j: with i steps out of k steps, at j position
        for k_id in range(1, k + 1):
            # for j in range(FAR_NEG, FAR+1):
            start = startPos - k_id
            end = startPos + k_id
            for j in range(start, end + 1):
                if j + 1 <= FAR:
                    dp[k_id][j] += dp[k_id - 1][j + 1]
                    dp[k_id][j] %= MOD
                if j - 1 >= FAR_NEG:
                    dp[k_id][j] += dp[k_id - 1][j - 1]
                    dp[k_id][j] %= MOD
        return dp[k][endPos] % MOD
```



## Iterative SegTree to replace MonoDeque

```python
class SEG:
    def __init__(self, n):
        self.tree = [0] * n * 2
        self.n = n
        
    def build(self, arr):
        # for i in range(self.n-1, -1, -1): # BUG
        for i in range(self.n, len(self.tree)):
            self.tree[i] = arr[i - self.n]
            
        for i in range(self.n-1, 0, -1): # FIX
            self.tree[i] = max(self.tree[i << 1], self.tree[i << 1 | 1])
            
    
    def query(self, left, right):
        l, r = left + self.n, right + self.n
        ret_max = -sys.maxsize
        while l < r:
            if l & 1 == 1:
                ret_max = max(ret_max, self.tree[l])
                # l ++ 1    # TYPO
                l += 1
            l >>= 1
            if r & 1 == 1:
                r -= 1
                ret_max = max(ret_max, self.tree[r])
            r >>= 1
        return ret_max
       
    def update(self, idx, val):
        i = idx + self.n
        self.tree[i] = val
        # i = idx   # BUGG!
        # while i > 0:  # BUG
        while i > 1:
            i >>= 1
            self.tree[i] = max(self.tree[i << 1], self.tree[i << 1 | 1])
        
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums:
            return nums
        if k == 1:
            return nums
    
        return self.my_20220921(nums, k)    # AC with SegTree!  
    
    def my_20220921(self, nums_orig, k):
        # print(len(nums_orig), k)
        debug = False	# set to True also AC!
        if debug:
            a = len(nums_orig)
            power = math.ceil(math.log(a, 2))
            n = 1 << power
            nums = nums_orig + [0] * (n - len(nums_orig))
            # print(nums, power, n, n - len(nums))
        else:
            n = len(nums_orig)
            nums = nums_orig

        seg = SEG(n)
        # for i in range(n):    # TLE
        #     seg.update(i, nums[i])
        # print(f'seg tree: {seg.tree}')
            
        seg.build(nums)         # FIX, optimize the build speed

        res = []
        end = min(len(nums_orig), n)
        for r in range(k, end+1):   # end: open end
            now = seg.query(r-k, r)
            res.append(now)
        return res
```



## 可撤銷 DSU

### 2092. Find All People With Secret



ref: https://developer.aliyun.com/article/848276

ref: https://ac-mikoto.top/2021/12/16/%E5%8F%AF%E6%92%A4%E9%94%80%E5%B9%B6%E6%9F%A5%E9%9B%86/





## Max E

```python
帶權無向圖，在「拿掉某個節點的情況後」，求圖的邊的最大權重值為何？

輸入是 
N nodes
(node1, node2, weight)


class Solution:
  	def __init__(self, edges):
      	# build graph
      	for 
      	  graph = {}        
        # query 時就是 edges <---- 
        cache = []
        self.findMax()
        
        
        edges = sorted(edges) by weight, a, b
        		[(8, 0, 1)]
          		[(8, 0, 2)]
        		↓	

    def findMax(self, node):
		self.graph 
        For I in range(N):
        # graph[a].append((b, w))

        For I inr ange(N):
        	if i != node
            For nbr in graph[i]:
              if nbr != node
              	# knock-down strategy        
        O(V + E)
        
        2nd version, with sorted edges
        
        ret = 0
        for w, a, b in edges:
          if a != node and b!= node:
            ret = w
           	break
      	return ret
      	O(E)  	-->	O(LgE)
        		--> O(V)
        
        3 cache
        return cache[]
            
Follow-up

```

https://codeshare.io/6pKd1Y



## Bisect in 2D

### 1272. Remove Interval

```python
from bisect import bisect_right
class Solution:
    def removeInterval(self, intervals: List[List[int]], toBeRemoved: List[int]) -> List[List[int]]:
        
        """
        s, e = toBeRemoved
        1 找最靠近的interval whose endtime > s
        2 找最靠近的interval whose starttime < e
        3 上述　1、２的閉區間就都是要刪掉的！
        O   O   O   X   X   X   O   O
                    left        right
        """
        s, e = toBeRemoved
        left = bisect_right(intervals, s,  key=lambda x: x[1])  # 
        right = bisect_right(intervals, e,  key=lambda x: x[0])
        # print(left, right)
        n = len(intervals)
        res = []
        for i in range(n):
            if i < left or i >= right:  # no overlap!
                res.append(intervals[i])
            else:   # has overlap
                if intervals[i][0] < s:
                    res.append([intervals[i][0], s])
                if intervals[i][1] > e:
                    res.append([e, intervals[i][1]])
                # res.append()
        return res
```





## Skyline

心法：用array幫助取得「前個時間點」的 aggregate 的結果！而不是跟後面的步驟絞在一起做

```python
class Solution:
    def getSkyline(self, buildings: List[List[int]]) -> List[List[int]]:        
        # return self.my_20221002(buildings)    # AC!
        # return self.my_20221003_BUG(buildings)  # BUG for Case2, 切邊會多紀錄
        return self.my_20221003_FIX(buildings)  # AC!

    def my_20221003_FIX(self, buildings):
        n = len(buildings)
        diffs = SortedDict([])
        for l, r, h in buildings:
            if l not in diffs:
                diffs[l] = []
            if r not in diffs:
                diffs[r] = []
            diffs[l].append((h, 1))
            diffs[r].append((h, -1))
        
        # print(diffs)
        # arr = []
        ticks = []
        rolling_max = []
        hs = SortedList([0])
        for time, items in diffs.items():
            for h, status in items:
                if not rolling_max or status == 1:
                    hs.add(h)
                else:
                    hs.remove(h)

            ticks.append(time)
            rolling_max.append(hs[-1])
        # print(ticks)
        # print(rolling_max)
        
        res = []
        for i in range(len(ticks)):
            time, high = ticks[i], rolling_max[i]
            if i == 0:
                res.append([time, high])
            else:
                if high != rolling_max[i - 1]:
                    res.append([time, high])
        return res    
    
    def my_20221003_BUG(self, buildings):
        n = len(buildings)
        diffs = SortedDict([])
        for l, r, h in buildings:
            if l not in diffs:
                diffs[l] = []
            if r not in diffs:
                diffs[r] = []
            diffs[l].append((h, 1))
            diffs[r].append((h, -1))
        
        # print(diffs)
        # arr = []
        ticks = []
        rolling_max = []
        hs = SortedList([0])
        for time, items in diffs.items():
            # arr.append((time, *items))
            for h, status in items:
                ticks.append(time)
                if not rolling_max:
                    hs.add(h)
                    rolling_max.append(h)
                else:
                    if status == 1:
                        hs.add(h)
                        rolling_max.append(max(rolling_max[-1], h))
                    else:
                        hs.remove(h)
                        # rolling_max.append(rolling_max[-1])   # BUG
                        rolling_max.append(hs[-1])   # FIX
                        # rolling_max.append()
        # print(ticks)
        # print(rolling_max)
        
        res = []
        for i in range(len(ticks)):
            time, high = ticks[i], rolling_max[i]
            if i == 0:
                res.append([time, high])
            else:
                if high != rolling_max[i - 1]:
                    res.append([time, high])
        return res
        """
        BUG for Ex2 -- 切邊會多紀錄！
        """
        
                
    
    def my_20221002(self, buildings):
        n = len(buildings)
        diffs = SortedDict()    # time -> heights
        for l, r, h in buildings:
            if l not in diffs:
                # diffs[l] = SortedList()
                diffs[l] = []
            if r not in diffs:
                # diffs[r] = SortedList()
                diffs[r] = []
            # diffs[l].add(-h)   # 進來時, 要先考慮高的；出去時，先考慮低的
            # diffs[r].add(h)
            diffs[l].append(-h)   # 進來時, 要先考慮高的；出去時，先考慮低的
            diffs[r].append(h)
            
        # print(diffs)
        bs = SortedList([0])
        res = []
        for time, heights in diffs.items():
            prev_high = bs[-1]
            # while bs[-1][0] < 
            
            for h in heights:       # h 的iteration不需要有序！因為是把這個時間的都全走完！
                # print(time, h)
                if h < 0:
                    # bs.add((h, time))
                    # bs.add(h) # BUGGG!
                    bs.add(-h) # FIX
                else:
                    bs.remove(h)
            # print(prev_high, bs)
            if bs[-1] != prev_high:
                # res.append([time, h])     # BUG
                res.append([time, bs[-1]])  # FIX!!
        return res
            
```





## Cyclic Sort

https://leetcode.com/discuss/study-guide/1902662/cyclic-sort-very-important-and-less-known-pattern





## Comparator 

### 692. Top K Frequent Words 

```python
from collections import Counter
import heapq

# heapq.__lt__ = lambda me, other: (me[0] < other[0]) or ((me[0] == other[0]) and (me[1] > other[1]))
# heapq.__lt__ = lambda me, other: me[1] > other[1]     # 要定義的地方是「類」！！！！

class Pair:
    def __init__(self, freq, word):
        self.freq, self.word = freq, word
    def __lt__(self, other):
        return self.freq < other.freq or self.freq == other.freq and self.word > other.word

class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        counter = Counter(words)
        heap = []
        for word, freq in counter.items():
            heapq.heappush(heap, Pair(freq, word))  # freq: small to large; word: large to small
            # heapq.heappush(heap, ())
            if len(heap) > k:
                heapq.heappop(heap)
        # print(heap)
        if not heap:
            return []
        ans = [''] * k
        for i in range(k - 1, -1, -1):
            # freq, word = heapq.heappop(heap)
            pair = heapq.heappop(heap)
            # print(freq, word)
            # freq = heapq.heappop(heap)
            ans[i] = pair.word
        return ans
```





## Bit OPs

### 1239. Maximum Length of a Concatenated String with Unique Characters

```python
class Solution:
    def maxLength(self, arr: List[str]) -> int:
        arr_masks = []
        for s in arr:
            mask = 0
            uniq = True
            for c in s:
                shift = ord(c) - ord('a')
                if (1 << shift) & mask == 0:
                    mask |= (1 << shift)
                else:
                    uniq = False
                    break
            if uniq:
                arr_masks.append(mask)
        # print(list(map(lambda x: format(x, '032b'), arr_masks)))
        
        res = [0]
        mask = 0
        def dfs(idx, mask, item):
            if idx == len(arr_masks):
                # print(mask)
                res[0] = max(mask.bit_count(), res[0])
                return
            
            if arr_masks[idx] & mask == 0:
                # dfs(idx + 1, arr_masks[idx] & mask, item + [idx])   # TAKE BUGGG!
                dfs(idx + 1, arr_masks[idx] | mask, item + [idx])   # TAKE  FIXX!
            dfs(idx + 1, mask, item)               # No TAKE
                
        dfs(0, 0, [])
        
        return res[0]
            
                
        """ ========================= """
        n = len(arr)        # ALSO AC, ref to lee215
        # res = set([])
        # dp = [set(arr[0])]
        dp = [set([])]
        for i in range(n):
            s = arr[i]
            set_s = set(s)
            if len(s) != len(set_s):
                continue
            for item in dp:
                if set_s & item:
                    continue
                dp.append(item | set_s) # FIXXX!
                # item |= set_s # BUGGG!
            # dp.append(set_s)  # BUGG
        # print(dp)
        return max(len(item) for item in dp)
```



## 等差 Seq. DP

### 446. Arighmetic Slices II - Subsequence

```python
from collections import defaultdict
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [[0] * n for _ in range(n)]    # dp[i][j]:以 i, j两位置的值為結尾的等差數列
        '''
        dp[i][j] : 找所有前面的 k
        dp[i][j] = 
        '''
        lookup = defaultdict(list)
        for i in range(n):
            lookup[nums[i]].append(i)
        res = 0
        for i in range(2, n):       # 尚未解決長度大於 3 的
            for j in range(0, i):
                # diff = nums[i] - nums[j]
                target = nums[j] - (nums[i] - nums[j])
                # 要想前找存在的k, 使 diff == nums[j] - nums[k]
                if target in lookup:
                    for k in lookup[target]:
                        if k < j:
                            # dp[i][j] += 1
                            dp[i][j] = dp[i][j] + dp[j][k] + 1
                            # dp[i][j] += (dp[j][k] + 1)
                    res += dp[i][j]
        return res
        
        
```



# Lee215

## [Sum of total strength of Wizards](https://youtu.be/IPf4GWc00ro?t=1806)

#### Sum of All Subarr Sum... etc



## Built-in Agg w/ Customization

```python
    return max(src.splitlines(), key=lambda item: len(item))
```



## To Improve

 2398 - lee215





L6

SVM 

overfitting, underfitting

linear regression, resnet, mobilenet, 

decision tree, 什么时候overfit

resnet

最近在解的问题



attention -- transformer ，segmentation, ML 

reinforcement learning



open question, 他們針對這個職位要問的

cllection, sanitycheck , biuld model, reason for this model , loss , performance , input/output

how to train

overfit : solution 



JD



15 -- to be the best of employer

 earn trucst

 dive deep

how to solve

STAR 來解！





忙 





# BQ

ask for inclusive



suewliao@deloitte.com.tw





# Vue

```python
arr = []
for line in sys.stdin:
    # print(line, end="")
    arr.append(line.strip().split('|'))

"""
issue: not single word mapping...
    many --> many
    many --> 1
Notes:
1 seems all Rosetta in msg must be provided mapping in subsequence pairs
"""
class Solution:
    def getTranslatedSentence(self, s, lookup):
        n = len(s)
        i = 0
        res = []
        while i < n:
            w = ''
            if s[i] == ' ':
                res.append(' ')
                i += 1
                continue
            while i < n and w.lstrip() not in lookup:
                w += s[i]
                i += 1
            res.append(lookup[w])
            # i += 1
        return ''.join(res)
        
        
        # s_list = s.split()    # not ok for case3!
        # n = len(s_list)
        # i = 0
        # res = []
        # while i < n:
        #     w = ''
        #     while i < n and w.lstrip() not in lookup:
        #         w += ' ' + s_list[i]
        #         i += 1
        #     res.append(lookup[w.lstrip()])
        #     # i += 1
        # return ' '.join(res)

lookup = {}
for i in range(1, len(arr)):
    fr, to = arr[i]
    lookup[fr] = to
    
# print(arr)
msg = arr[0][0]
# print(lookup)
# print(msg)
print(Solution().getTranslatedSentence(msg, lookup))
```



```python
import sys
# import numpy as np
# import pandas as pd
# from sklearn import ...

arr = []
for line in sys.stdin:
    # print(line, end="")
    arr.append(line.strip())
assert(len(arr) == 1)


class Solution:
    def countNumberEmersions(self, s):
        n = len(s)
        cnt = 0
        level = 0
        prev = 0
        for i in range(n):
            # if i > 0 and 
            if s[i] == 'U':
                level += 1
            else:
                level -= 1
            
            if level == 0:
                if prev < 0:
                    cnt += 1
            prev = level
        return cnt

s = arr[0]
UD = set(['U', 'D'])
assert([x in UD for x in s])
print(Solution().countNumberEmersions(s))
```
