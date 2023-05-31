---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-07-26
---





# Bi-BFS



### LC1197 Minimum Knight Moves

In an **infinite** chess board with coordinates from `-infinity` to `+infinity`, you have a **knight** at square `[0, 0]`.

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h2ufl4y2kzj20ba0ba3yu.jpg)

Return *the minimum number of steps needed to move the knight to the square* `[x, y]`. It is guaranteed the answer exists.

 

**Example 1:**

```
Input: x = 2, y = 1
Output: 1
Explanation: [0, 0] → [2, 1]
```

**Example 2:**

```
Input: x = 5, y = 5
Output: 4
Explanation: [0, 0] → [2, 1] → [4, 2] → [3, 4] → [5, 5]
```

 

**Constraints:**

- `-300 <= x, y <= 300`
- `0 <= |x| + |y| <= 300`



#### Code

```python
DIRECTIONS = [
                [1, 2], [1, -2], [-1, 2], [-1, -2],
                [2, 1], [2, -1], [-2, 1], [-2, -1],
               ]
from collections import deque
class Solution:
    def minKnightMoves(self, x: int, y: int) -> int:
        # return self.my_20220602(x, y) # AC
        return self.my_20220602B(x, y)  # AC
    
    def my_20220602B(self, x, y):
        que = deque([(0, 0)])
        # visited = {(0, 0): 0}
        visited = set([(0, 0)])
        back_que = deque([(x, y)])
        back_visited = set([(x, y)])
        steps = 0
        while que:
            if len(que) > len(back_que):
                que, back_que = back_que, que
                visited, back_visited = back_visited, visited
                
            len_que = len(que)
            for _ in range(len_que):
                i, j = que.popleft()
                # if (i, j) == (x, y):
                #     return steps
                if (i, j) in back_visited:
                    return steps
                for di, dj in DIRECTIONS:
                    ni, nj = i + di, j + dj
                    # if 0 <= ni < 
                    if (ni, nj) not in visited:
                        que.append((ni, nj))
                        visited.add((ni, nj))
            steps += 1
        return -1
    
    def my_20220602(self, x, y):
        que = deque([(0, 0)])
        visited = set([(0, 0)])
        # visited[(0, 0)] =
        steps = 0
        while que:
            len_que = len(que)
            for _ in range(len_que):
                i, j = que.popleft()
                if (i, j) == (x, y):
                    return steps
                for di, dj in DIRECTIONS:
                    ni, nj = i + di, j + dj
                    # if 0 <= ni < 
                    if (ni, nj) not in visited:
                        que.append((ni, nj))
                        visited.add((ni, nj))   # important!
            steps += 1
        return -1
```





### LC1345 Jump Game IV

Beside word ladder II, there's interesting pattern of Bi-BFS in LC1345

Instead of just using 'steps as a set', I prefer to use a dictionay to remember the steps of each index. 

And I fix it to AC. 

##### Special Take away:

> ```python
>         if len(Q) > len(back_Q):
>             Q, back_Q = back_Q, Q
>             visited, visited_back = visited_back, visited
> ```



#### Code

```python
from collections import deque
from sortedcontainers import SortedDict
class Solution:
    def minJumps(self, arr: List[int]) -> int:
        # return self.my_20220114(arr)	# AC
        return self.my_20220114_opt_bibfs(arr)	# AC
    
    def my_20220114_opt_bibfs(self, arr):
        n = len(arr)
        if n <= 1:
            return 0
        mapping = {}
        for i, val in enumerate(arr):
            # mapping[val] = mapping.get(val, []) + [i] # 這行導致不 AC - . -... 無言啦!, 下面fix
            if val in mapping:
                mapping[val].append(i)
            else:
                mapping[val] = [i]
            
        # print(mapping)
        Q = deque([0])
        back_Q = deque([n - 1])
        visited = SortedDict([])
        visited[0] = 0
        # visited[n - 1] = 0
        visited_back = SortedDict([])
        visited_back[n - 1] = 0
        while Q:
            if len(Q) > len(back_Q):
                Q, back_Q = back_Q, Q
                visited, visited_back = visited_back, visited
                
            next_Q = deque([])
            len_Q = len(Q)
            for _ in range(len_Q):
                now = Q.popleft()
                # fly first!
                for nxt in mapping[arr[now]]:
                    # if nxt in back_Q:
                    if nxt in visited_back:
                        # return visited.get(nxt, 0) * 2 + 1    # BUG
                        return visited.get(now, 0) + 1 + visited_back.get(nxt, 0)   # BUG
                    if not (0 <= nxt <= n - 1):
                        continue
                    if nxt not in visited:
                        # visited[nxt] = min(visited[now], visited.get(nxt, n))
                        visited[nxt] = min(visited[now] + 1, visited.get(nxt, n))
                        next_Q.append(nxt)
                
                mapping[arr[now]].clear()
                # then step one or -1                
                for delta in [1, -1]:
                    # if nxt in back_Q:
                    nxt = now + delta
                    if nxt in visited_back:
                        return visited.get(now, 0) + 1 + visited_back.get(nxt, 0) # BUG
                    if not (0 <= nxt <= n - 1):
                        continue
                    visited[nxt] = min(visited[now] + 1, visited.get(nxt, n))
                    if visited[nxt] == visited[now] + 1:
                        next_Q.append(nxt)
            Q = next_Q
        return -1
        # return visited[n - 1]
        #     print(next_Q)
        #     print(visited)
        # print(visited)

    
    
    def my_20220114(self, arr):
        n = len(arr)
        mapping = {}
        for i, val in enumerate(arr):
            # mapping[val] = mapping.get(val, []) + [i] # 這行導致不 AC - . -... 無言啦!, 下面fix
            if val in mapping:
                mapping[val].append(i)
            else:
                mapping[val] = [i]
            
        # print(mapping)
        Q = deque([0])
        visited = SortedDict([])
        visited[0] = 0
        while Q:
            next_Q = deque([])
            len_Q = len(Q)
            for _ in range(len_Q):
                now = Q.popleft()
                if now == n - 1:            # early return, still TLE... 
                    return visited[now]
                # fly first!
                for nxt in mapping[arr[now]]:
                    if not (0 <= nxt <= n - 1):
                        continue
                    if nxt not in visited:
                        # visited[nxt] = min(visited[now], visited.get(nxt, n))
                        visited[nxt] = min(visited[now] + 1, visited.get(nxt, n))
                        next_Q.append(nxt)
                
                mapping[arr[now]].clear()
                # then step one or -1                
                for delta in [1, -1]:
                    nxt = now + delta
                    if not (0 <= nxt <= n - 1):
                        continue
                    visited[nxt] = min(visited[now] + 1, visited.get(nxt, n))
                    if visited[nxt] == visited[now] + 1:
                        next_Q.append(nxt)
            Q = next_Q
        return visited[n - 1]
        #     print(next_Q)
        #     print(visited)
        # print(visited)

```



### LC127 Word Ladder

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:   
      	# return self.my_20220914(beginWord, endWord, set(wordList))    # AC
        return self.my_20220914_bibfs(beginWord, endWord, set(wordList))    # AC!

    def my_20220914_bibfs(self, beginWord, endWord, wordset):
        if endWord not in wordset:  # FIX case2
            return 0
        
        que = deque([beginWord])
        steps = 0
        # visited = set([beginWord])
        visited = {beginWord:1}
        
        end_que = deque([endWord])
        end_visited = {endWord:1}
        while que:
            if len(end_que) < len(que):
                que, end_que = end_que, que
                visited, end_visited = end_visited, visited
            
            size = len(que)
            for _ in range(size):
                now = que.popleft()
                # if now == endWord:
                    # return steps + 1
                    
                nbrs = self.getNeighbors_20220914(now, wordset) # 26*L^2
                for nbr in nbrs:
                    if nbr not in visited:
                        que.append(nbr)
                        # visited.add(nbr)
                        if nbr in end_visited:
                            return end_visited[nbr] + visited[now]
                        visited[nbr] = visited[now] + 1    
            # steps += 1
        return 0
            
    
    def my_20220914(self, beginWord, endWord, wordset):
        # n = len(wordset)
        # wordset.add(endWord)  # BUG! 這題不該加
        que = deque([beginWord])
        steps = 0
        visited = set([beginWord])   # 別忘了呀！！！
        while que:      # N
            size = len(que)
            for _ in range(size):
                now = que.popleft()
                if now == endWord:
                    return steps + 1
                nbrs = self.getNeighbors_20220914(now, wordset) # 26*L^2
                for nbr in nbrs:
                    if nbr not in visited:
                        que.append(nbr)
                        visited.add(nbr)
                # [que.append(nbr) for nbr in nbrs if nbr not in visited]
                
            steps += 1
        return 0

    def getNeighbors_20220914(self, now, wordset):
        nbrs = []
        for j in range(len(now)):   # L
            for k in range(26):     # 26
                token = now[:j] + chr(k + ord('a'))+ now[j+1:]  # L
                if token in wordset:
                    nbrs.append(token)
        return nbrs
```
