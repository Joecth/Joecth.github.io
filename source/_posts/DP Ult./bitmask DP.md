---
layout: post
categories: DP Ult.
tag: [] 
date: 2019-10-08
---





## LC 1066



帶權二分圖最優匹配 -- 是匈牙利算法的進階版，叫 KM 算法，但超綱！…

但這裡用 Dijkastra 仍可, 轉換成「以狀態為結點的最短距離」

- 任意點到到給定的起點的
- 全邊不能為負，需 >= 0　（？！）
- BFS + pq
- 每次都是離起點最小的距離的點，所以就很貪心　



m worker.size()

n bikes.size()

initial state: 00000



State: 00110 表示已處理完前兩個工人配了2、3號bikes

k is the number of 1 bits in state

the min cost of the first k workers assigned with the 1-bit bikes

至於誰配到了哪輛車，我們不care，這就是狀態！



state 就是一個node 

$10110: dp[00110] + dist[2][0]$

the min cost of the first 3 workers assigned with 0/2/3-th bikes



$01110: dp[00110] + dist[2][0]$

$00111: dp[00110] + dist[2][4]$



```python
import heapq
class Solution:
    def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> int:
    #     return self.my_TLE(workers, bikes)
        # return self.my_20221221_FAILED(workers, bikes)
        # return self.my_20221221_AC(workers, bikes)
        return self.my_20221222(workers, bikes)

    def my_20221222(self, workers, bikes):
        n, m = len(workers), len(bikes)
        dist = [[0] * m for _ in range(n)]
        for i in range(n):
            x, y = workers[i]
            for j in range(m):
                x2, y2 = bikes[j]
                dist[i][j] = abs(x - x2) + abs(y - y2)
        [print(row) for row in dist]

        min_heap = [(0, 0)]
        visited = {0: 0}
        while min_heap:
            cost, now_state = heapq.heappop(min_heap)
            # i = state.bin_counts()
            i = now_state.bit_count()
            # if i == m:  # BUG
            if i == n:
                return cost

            for j in range(m):
                if (now_state >> j) & 1 == 1:
                    continue
                
                next_state = (now_state | (1 << j))
                
                if visited.get(now_state) + dist[i][j] < visited.get(next_state, sys.maxsize):
                    heapq.heappush(min_heap, (cost + dist[i][j], next_state))
                    visited[next_state] = cost + dist[i][j]
                """ Alternative Block for Dijkastra!
                if next_state not in visited:
                    # print(i, j)
                    heapq.heappush(min_heap, (cost + dist[i][j], next_state))
                    visited[next_state] = cost + dist[i][j]

                if visited[now_state] + dist[i][j] < visited[next_state]:
                    heapq.heappush(min_heap, (cost + dist[i][j], next_state))
                    visited[next_state] = cost + dist[i][j]
                """

        return -1
    """
    Runtime Error
    IndexError: list index out of range
        heapq.heappush(min_heap, (cost + dist[i][j], next_state))
    Line 35 in my_20221222 (Solution.py)
        return self.my_20221222(workers, bikes)
    Line 7 in assignBikes (Solution.py)
        ret = Solution().assignBikes(param_1, param_2)
    Line 182 in _driver (Solution.py)
        _driver()
    Line 193 in <module> (Solution.py)
    Last Executed Input
    [[0,0],[1,0],[2,0],[3,0],[4,0]]
    [[0,999],[1,999],[2,999],[3,999],[4,999],[5,999]]
    """




    def my_20221221_FAILED(self, workers, bikes): 
        # FAILED at 
        """
        Input
        workers =
        [[239,904],[191,103],[260,117],[86,78],[747,62]]
        bikes =
        [[660,8],[431,772],[78,576],[894,481],[451,730],[155,28]]
        Output
        2016
        Expected
        1886        跟 wisdompeak 的比較了後，是加入到 visited 的時間點的差異引起的問題，拉到popleft()後再做就OK了 Fix在下面那個AC的Function！　或是，也才想起來，Dijkastra　的 visited　如果要放在入隊時處理，那就是不能這樣擋，而是要用 hashmap 映射到個 cost 作比較!
        """
        n, m = len(workers), len(bikes)
        dist = [[0] * m for _ in range(n)]
        for i in range(n):
            x, y = workers[i]
            for j in range(m):
                x2, y2 = bikes[j]
                dist[i][j] = abs(x - x2) + abs(y - y2)

        min_heap = [(0, 0)]   # cost, state
        # visited = set([0])
        visited = set([])
        while min_heap:
            cost, state = heapq.heappop(min_heap)
            # if state in visited:
            #     continue
            # visited.add(state)

            i = state.bit_count()
            # print(f'now worker: {i}, now state: {state}')
            if i == n:
                return cost
            for j in range(m):
                # print(i, j)
                if (state >> j) & 1 == 1:   # This bike already taken
                    continue
                next_state = state | (1 << j)
                if next_state not in visited:
                    # min_heap.append((next_state, cost + dist[i][j]))
                    heapq.heappush(min_heap, (cost + dist[i][j], next_state))
                    visited.add(next_state)

            # print('min_heap; ',  min_heap)
            # print([(cost, format(state, f'0{m}b')) for cost, state in min_heap])
            # print('visited: ', visited)
            # print([format(i, f'0{m}b') for i in visited])
        return -1


    def my_20221221_AC(self, workers, bikes):
        n, m = len(workers), len(bikes)
        dist = [[0] * m for _ in range(n)]
        for i in range(n):
            x, y = workers[i]
            for j in range(m):
                x2, y2 = bikes[j]
                dist[i][j] = abs(x - x2) + abs(y - y2)

        # dp = [[0] * m for _ in range(n)]
        min_heap = [(0, 0)]   # cost, state
        # visited = set([0])
        visited = set([])
        while min_heap:
            cost, state = heapq.heappop(min_heap)
            if state in visited:
                continue
            visited.add(state)

            i = state.bit_count()
            print(f'now worker: {i}, now state: {state}')
            if i == n:
                return cost
            for j in range(m):
                # print(i, j)
                if (state >> j) & 1 == 1:   # This bike already taken
                    continue
                next_state = state | (1 << j)
                if next_state not in visited:
                    # min_heap.append((next_state, cost + dist[i][j]))
                    heapq.heappush(min_heap, (cost + dist[i][j], next_state))
                    # visited.add(next_state)

            # print('min_heap; ',  min_heap)
            # print([(cost, format(state, f'0{m}b')) for cost, state in min_heap])
            # print('visited: ', visited)
            # print([format(i, f'0{m}b') for i in visited])
        return -1
"""
[[239,904],[191,103],[260,117],[86,78],[747,62]]
[[660,8],[431,772],[78,576],[894,481],[451,730],[155,28]]
"""

    # def my_TLE(self, workers, bikes):
    #     n, m = len(workers), len(bikes)
    #     ans = [sys.maxsize]
    #     self.backtrack(workers, bikes, 0, set([]), 0, ans)
    #     return ans[0]
    
    # def backtrack(self, workers, bikes, worker_idx, used_bikes, accu_dist, ans):
    #     if worker_idx >= len(workers):
    #         ans[0] = min(ans[0], accu_dist)
    #         return
        
    #     for i in range(len(bikes)):
    #         if i not in used_bikes:
    #             used_bikes.add(i)
    #             now_dist = self.getDist(workers[worker_idx], bikes[i])
    #             accu_dist += now_dist
    #             self.backtrack(workers, bikes, worker_idx+1, used_bikes, accu_dist, ans)
    #             accu_dist -= now_dist
    #             used_bikes.remove(i)

    # def getDist(self, w, b):
    #     return sum(abs(w[idx] - b[idx]) for idx in [0, 1])
```



