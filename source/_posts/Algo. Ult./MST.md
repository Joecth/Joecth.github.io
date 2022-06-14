---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2022-02-28
---





# MST

- `Kruskal` 

- `Dijkastra` (kind of Prim)





## 1102. Path With Maximum Minimum Value

> Given an `m x n` integer matrix `grid`, return *the maximum **score** of a path starting at* `(0, 0)` *and ending at* `(m - 1, n - 1)` moving in the 4 cardinal directions.
>
> The **score** of a path is the minimum value in that path.
>
> - For example, the score of the path `8 → 4 → 5 → 9` is `4`.



**Example 1:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h0qkkbfdkaj206s06tmx2.jpg)

```
Input: grid = [[5,4,5],[1,2,6],[7,4,6]]
Output: 4
Explanation: The path with the maximum score is highlighted in yellow. 
```

**Example 2:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h0qkkciyy2j206s06tmx2.jpg)

```
Input: grid = [[2,2,1,2,2,2],[1,2,2,2,1,2]]
Output: 2
```

**Example 3:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h0qkkcxptvj20b80dhdg4.jpg)

```
Input: grid = [[3,4,6,3,4],[0,2,1,1,7],[8,8,3,2,7],[3,2,4,9,8],[4,1,2,0,0],[4,6,5,4,3]]
Output: 3
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `0 <= grid[i][j] <= 109`





### My Code

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
    
class Solution:
    def maximumMinimumPath(self, A: List[List[int]]) -> int:
        # return self.sol(A)
        return self.my_20220206(A)    # AC
        return self.my_20220207(A)    # AC, sol MST
    
    def my_20220207(self, A):
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        m, n = len(A), len(A[0])
        # max_heap = [(-A[0][0], 0, 0), (-A[m - 1][n - 1], m - 1, n - 1)]   # no need
        max_heap = []
        # res = sys.maxsize 不需要
        for i in range(m):
            for j in range(n):
                heapq.heappush(max_heap, (-A[i][j], i, j))
                
        uf = UF(m * n)
        seen = set([])
        src = 0
        dst = (m - 1) * n + (n - 1)
        res = sys.maxsize
        while max_heap:
            val, i, j = heapq.heappop(max_heap)
            seen.add((i, j))
            # print(-val, i, j)
            # if uf.is_connected(src, dst): # BUGG! shouldn't be here
            #     # return -val
            now = i * n + j
            for di, dj in dire:
                ni, nj = i + di, j + dj
                # if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in seen:
                if 0 <= ni < m and 0 <= nj < n and (ni, nj) in seen:
                    nbr = ni * n + nj
                    uf.merge(now, nbr)
                    # seen.add((ni, nj))    BUG! shouldn't be here
            # print(uf.parents)
            if uf.is_connected(src, dst):
                return -val            
        return -1
        
    
    def my_20220206(self, A):
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        max_heap = [(-A[0][0], 0, 0)]
        seen = set([(0, 0)])
        m, n = len(A), len(A[0])
        res = sys.maxsize
        while max_heap:
            val, i, j = heapq.heappop(max_heap)
            res = min(res, -val)
            if i == m - 1 and j == n - 1:
                return res
            for di, dj in dire:
                ni, nj = i + di, j + dj
                if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in seen:
                    # heapq.heappush(max_heap, (max(val, -A[ni][nj]), ni, nj))  # 
                    heapq.heappush(max_heap, (-A[ni][nj], ni, nj))
                    seen.add((ni, nj))
        return -1
            
        
        
    def sol(self, A):
        dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        R, C = len(A), len(A[0])
        
        maxHeap = [(-A[0][0], 0, 0)]
        seen = [[0 for _ in range(C)] for _ in range(R)]
        while maxHeap:
            val, x, y = heapq.heappop(maxHeap)
            # seen[x][y] = 1 # got TLE
            if x == R - 1 and y == C - 1: return -val
            for dx, dy in dire:
                nx, ny = x + dx, y + dy
                if 0 <= nx < R and 0 <= ny < C and not seen[nx][ny]:
                    seen[nx][ny] = 1 # passed
                    heapq.heappush(maxHeap, (max(val, -A[nx][ny]), nx, ny))
        return -1        
#         m, n = len(grid), len(grid[0])
        
#         Q = deque([])
```