---
layout: post
categories: Contests
tag: []
date: 2022-06-04
Author: Jo
---



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h380m00lvdj21000e4mz2.jpg" alt="image-20220614195114137" style="zoom:67%;" />





# Weekly295 -- 2287~2290

## 2290 Minimum Obstacle Removal to Reach Corner

You are given a **0-indexed** 2D integer array `grid` of size `m x n`. Each cell has one of two values:

- `0` represents an **empty** cell,
- `1` represents an **obstacle** that may be removed.

You can move up, down, left, or right from and to an empty cell.

Return *the **minimum** number of **obstacles** to **remove** so you can move from the upper left corner* `(0, 0)` *to the lower right corner* `(m - 1, n - 1)`.

 

**Example 1:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h2xvr7kkwqj20gt06u0t5.jpg)

```
Input: grid = [[0,1,1],[1,1,0],[1,1,0]]
Output: 2
Explanation: We can remove the obstacles at (0, 1) and (0, 2) to create a path from (0, 0) to (2, 2).
It can be shown that we need to remove at least 2 obstacles, so we return 2.
Note that there may be other ways to remove 2 obstacles to create a path.
```

**Example 2:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h2xvr6hui1j20b906uglo.jpg)

```
Input: grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]]
Output: 0
Explanation: We can move from (0, 0) to (2, 4) without removing any obstacles, so we return 0.
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 105`
- `2 <= m * n <= 105`
- `grid[i][j]` is either `0` **or** `1`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`



```python
DIRE = [(1, 0), (-1, 0), (0, 1), (0, -1)]
import heapq
from collections import deque
class Solution:
    def minimumObstacles(self, grid: List[List[int]]) -> int:
        # return self.my_TLE(grid)
        return self.my_20220605(grid)
                
    def my_20220605(self, grid):
        m, n = len(grid), len(grid[0])
        # que = deque([(0, 0, 0)])    # i, j, removed cnt
        hque = [(0, 0, 0)]
        visited = {}
        visited[(0, 0)] = 0
        
        # back_que = deque([(m - 1, n - 1, 0)])
        # back_visited = {}
        # back_visited[(m - 1, n - 1)] = 0
        while hque:
            # if len(que) > len(back_que):
            #     que, back_que = back_que, que
            #     visited, back_visited = back_visited, visited
                
            # i, j, rm_cnt = que.popleft()
            rm_cnt, i, j = heapq.heappop(hque)
            if (i, j) == (m - 1, n - 1):
                return rm_cnt
            # print(i, j, rm_cnt)
            for di, dj in DIRE:
                ni, nj = i + di, j + dj
                if 0 <= ni < m and 0 <= nj < n: #and (ni, nj) not in visited:
                    # if (ni, nj) in back_visited:
                    #     return visited.get((ni, nj), 0) + back_visited.get((ni, nj), 0)
                    
                    if (ni, nj) not in visited:
                        # que.append((ni, nj, rm_cnt + grid[ni][nj]))
                        heapq.heappush(hque, (rm_cnt + grid[ni][nj], ni, nj))
                        visited[(ni, nj)] = rm_cnt + grid[ni][nj]
                        
                    # if rm_cnt + 1 < visited.get((ni, nj), 0):
                    # if rm_cnt + 1 <= visited.get((ni, nj), 0):
                    #     que.append((ni, nj, rm_cnt + grid[ni][nj]))
                    #     visited[(ni, nj)] = min(rm_cnt + grid[ni][nj], visited.get((ni, nj), 0))
        # return visited[(m - 1, n - 1)]


    def my_TLE(self, grid):
        m, n = len(grid), len(grid[0])
        que = deque([(0, 0, 0)])    # i, j, removed cnt
        visited = {}
        visited[(0, 0)] = 0
        
        back_que = deque([(m - 1, n - 1, 0)])
        back_visited = {}
        back_visited[(m - 1, n - 1)] = 0
        while que:
            # if len(que) > len(back_que):
            #     que, back_que = back_que, que
            #     visited, back_visited = back_visited, visited
                
            i, j, rm_cnt = que.popleft()
            # print(i, j, rm_cnt)
            for di, dj in DIRE:
                ni, nj = i + di, j + dj
                if 0 <= ni < m and 0 <= nj < n: #and (ni, nj) not in visited:
                    # if (ni, nj) in back_visited:
                    #     return visited.get((ni, nj), 0) + back_visited.get((ni, nj), 0)
                    
                    if (ni, nj) not in visited:
                        que.append((ni, nj, rm_cnt + grid[ni][nj]))
                        visited[(ni, nj)] = rm_cnt + grid[ni][nj]
                        
                    # if rm_cnt + 1 < visited.get((ni, nj), 0):
                    if rm_cnt + 1 <= visited.get((ni, nj), 0):
                        que.append((ni, nj, rm_cnt + grid[ni][nj]))
                        visited[(ni, nj)] = min(rm_cnt + grid[ni][nj], visited.get((ni, nj), 0))
        return visited[(m - 1, n - 1)]
            
        
```



## 2289 Steps to Make Array Non-descreasing





ref: https://www.youtube.com/watch?v=NQGduRE1Crk







# Biweekly79 -- 2283~2286



