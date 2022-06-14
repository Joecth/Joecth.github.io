---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2022-02-28
---







# Shortest Path - Dijkastra



## 1293 Shortest Path in a Grid with Obstacles Elimination

Hard

You are given an `m x n` integer matrix `grid` where each cell is either `0` (empty) or `1` (obstacle). You can move up, down, left, or right from and to an empty cell in **one step**.

Return *the minimum number of **steps** to walk from the upper left corner* `(0, 0)` *to the lower right corner* `(m - 1, n - 1)` *given that you can eliminate **at most*** `k` *obstacles*. If it is not possible to find such walk return `-1`.

 

**Example 1:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h2zr9nc3fzj206s0b9q2z.jpg)

```
Input: grid = [[0,0,0],[1,1,0],[0,0,0],[0,1,1],[0,0,0]], k = 1
Output: 6
Explanation: 
The shortest path without eliminating any obstacle is 10.
The shortest path with one obstacle elimination at position (3,2) is 6. Such path is (0,0) -> (0,1) -> (0,2) -> (1,2) -> (2,2) -> (3,2) -> (4,2).
```

**Example 2:**

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h2zr9noia0j206s06tglo.jpg)

```
Input: grid = [[0,1,1],[1,1,1],[1,0,0]], k = 1
Output: -1
Explanation: We need to eliminate at least two obstacles to find such a walk.
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 40`
- `1 <= k <= m * n`
- `grid[i][j]` is either `0` **or** `1`.
- `grid[0][0] == grid[m - 1][n - 1] == 0`

```python
import heapq
DIRE = [(-1, 0), (1, 0), (0, -1), (0, 1)]
class Solution:
    def shortestPath(self, grid: List[List[int]], K: int) -> int:
        # return self.my_20210915_FIX(grid, K)    # 關鍵：同一個位置，不同次的消次數，就該算未見過！！
        # return self.my_20220606_TLE(grid, K)
        # return self.my_20220606_STILL_TLE(grid, K)
        return self.my_20220606(grid, K)
    
    def my_20220606(self, grid, K):
        '''
        Djikastra
        BFS-like solution, but replace que with heap, 
        prioritize "steps"
        '''
        m, n = len(grid), len(grid[0])
        min_heap = [(0, 0, 0)]  # steps that already taken, rm_cnt, i, j
        visited = {}
        visited[(0, 0)] = 0     # (i, j): num of rm_cnt
        # visited[(0, 0, 0)] = 0  
        
        while min_heap:
            # print(min_heap)
            steps, i, j = heapq.heappop(min_heap)
            # TODO: finish check! if i
            if (i, j) == (m - 1, n - 1):
                return steps
            for di, dj in DIRE:
                ni, nj = i + di, j + dj     # ni, nj: next i, next j
                # if 0 <= ni < m and 0 <= nj < n and rm_cnt < K:    # BUGGG
                if 0 <= ni < m and 0 <= nj < n and (ni, nj) not in visited:
                    """ DEBUG!"""
                    rm_next = 1 if grid[ni][nj] == 1 else 0
                    if visited[(i, j)] + rm_next > K:
                        continue
                    heapq.heappush(min_heap, (steps + 1, ni, nj))
                    # visited.add((steps + 1, neg_rm_cnt - rm_next, ni, nj))
                    visited[(ni, nj)] = visited[(i, j)] + rm_next
                    
                # if 0 <= ni < m and 0 <= nj < n and visited[(ni, nj)] > visited[(i, j)] + grid[ni][nj]:
                if 0 <= ni < m and 0 <= nj < n and visited.get((ni, nj), 0) > visited[(i, j)] + grid[ni][nj]:
                    rm_next = 1 if grid[ni][nj] == 1 else 0
                    visited[(ni, nj)] = visited[(i, j)] + rm_next
                    heapq.heappush(min_heap, (steps + 1, ni, nj))
                    # visited.add((steps + 1, neg_rm_cnt - rm_next, ni, nj))
            # print(visited.get((2, 0)))
        return -1
    
    def my_20220606_STILL_TLE(self, grid, K):
        '''
        Djikastra
        BFS-like solution, but replace que with heap, 
        prioritize "steps"
        '''
        m, n = len(grid), len(grid[0])
        min_heap = [(0, 0, 0, 0)]  # steps that already taken, rm_cnt, i, j
        visited = set([])
        # visited[(0, 0, 0)] = 0  
        
        while min_heap:
            # print(min_heap)
            steps, neg_rm_cnt, i, j = heapq.heappop(min_heap)
            # TODO: finish check! if i
            if (i, j) == (m - 1, n - 1):
                return steps
            for di, dj in DIRE:
                ni, nj = i + di, j + dj     # ni, nj: next i, next j
                # if 0 <= ni < m and 0 <= nj < n and rm_cnt < K:    # BUGGG
                if 0 <= ni < m and 0 <= nj < n and -neg_rm_cnt <= K:     # FIX
                    # if (steps + 1, rm_cnt + 1, ni, nj) in visited:    # BUG
                    rm_next = 1 if grid[ni][nj] == 1 else 0
                    if (steps + 1, neg_rm_cnt - rm_next, ni, nj) in visited:
                        continue
                    if (neg_rm_cnt - rm_next) * -1 > K: continue
                    heapq.heappush(min_heap, (steps + 1, neg_rm_cnt - rm_next, ni, nj))
                    visited.add((steps + 1, neg_rm_cnt - rm_next, ni, nj))
        return -1    
    
    def my_20220606_TLE(self, grid, K):
        '''
        Djikastra
        BFS-like solution, but replace que with heap, 
        prioritize "steps"
        '''
        m, n = len(grid), len(grid[0])
        min_heap = [(0, 0, 0, 0)]  # steps that already taken, rm_cnt, i, j
        visited = set([])
        # visited[(0, 0, 0)] = 0  
        
        while min_heap:
            # print(min_heap)
            steps, rm_cnt, i, j = heapq.heappop(min_heap)
            # TODO: finish check! if i
            if (i, j) == (m - 1, n - 1):
                return steps
            for di, dj in DIRE:
                ni, nj = i + di, j + dj     # ni, nj: next i, next j
                # if 0 <= ni < m and 0 <= nj < n and rm_cnt < K:    # BUGGG
                if 0 <= ni < m and 0 <= nj < n and rm_cnt <= K:     # FIX
                    # if (steps + 1, rm_cnt + 1, ni, nj) in visited:    # BUG
                    rm_next = 1 if grid[ni][nj] == 1 else 0
                    if (steps + 1, rm_cnt + rm_next, ni, nj) in visited:
                        continue
                    if rm_cnt + rm_next > K: continue
                    heapq.heappush(min_heap, (steps + 1, rm_cnt + rm_next, ni, nj))
                    visited.add((steps + 1, rm_cnt + rm_next, ni, nj))
        return -1
                    
```
