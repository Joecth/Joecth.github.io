---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-07-26
---





# Fenwick Tree

![image-20200715173319551](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrsh3fq61j31920lm47i.jpg)

![image-20200715174108116](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrsp6fuk6j31960mgqeg.jpg)

![image-20200715174924357](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrsxs6yawj30y60ge7cg.jpg)



```python
class FenwickTree(object):
    def __init__(self, n):
        self.sums_ = [0] * (n+1)
        self.n = n
        
    def update(self, i, delta):
        while i < len(self.sums_):
            self.sums_[i] += delta
            i += self._lowbit(i)
    
    def query(self, i):
        sum_ = 0
        while i > 0:
            sum_ += self.sums_[i]
            i -= self._lowbit(i)
        return sum_
    
    def _lowbit(self, x):
        return x & -x
    

class NumArray:
    def __init__(self, nums: List[int]):
        self.nums = nums
        self.tree = FenwickTree(len(nums))
        for i in range(len(nums)):
            self.tree.update(i+1, nums[i])

    def update(self, i: int, val: int) -> None:
        self.tree.update(i+1, val - self.nums[i])
        self.nums[i] = val

    def sumRange(self, i: int, j: int) -> int:
        return self.tree.query(j + 1) - self.tree.query(i)
```



## 315.Count of Smaller Numbers After Self

![image-20200718023519761](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggujdp1o3cj319y0n2k4o.jpg)



![image-20200718023609770](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggujeh5tfej31ae0ms12c.jpg)

![image-20200718023824394](https://tva1.sinaimg.cn/large/e6c9d24egy1h382bwtejxj20sy0q6myy.jpg)

![image-20200718023848244](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggujh9hnupj30xy0kygz9.jpg)



# Segment Tree

### Tutorial 1:

- 更直觀且容易理解，但就是代碼長
- binary tree, balanced, so O(logN); not necessary perfect

![image-20200717004335838](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtajfaf6jj315q0k2ajx.jpg)



![image-20200717004840098](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtaogby29j31760l4al7.jpg)



![image-20200717005918569](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtazhqyolj31380i210v.jpg)

#### Examples

![image-20200717010205089](https://tva1.sinaimg.cn/large/e6c9d24egy1h382cwznscj216m0k8q5v.jpg)





![image-20200717011114867](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtbc3bhtbj31720ke13z.jpg)

![image-20200717011201336](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtbcsjngrj31840mgwq4.jpg)



![image-20200717011546317](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggtbh09kqsj317s0mu7gt.jpg)



### Tutorial 2:

https://leetcode.com/problems/range-sum-query-mutable/discuss/646774/Segment-tree-recursive-iterative-Binary-index-iterative-explained

https://www.youtube.com/watch?time_continue=2&v=Oq2E2yGadnU&feature=emb_logo



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382c3mf4uj20z80nc76k.jpg" alt="image-20200731220447017" style="zoom:67%;" />

![image-20200731220717640](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghaib0wwh8j30wc0luaj8.jpg)

## 9P SegTree

![image-20210310122717309](https://tva1.sinaimg.cn/large/e6c9d24egy1h382c643jyj20ha06pweq.jpg)

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382ce6bntj20uk0eemxm.jpg" alt="image-20210311105831208" style="zoom:50%;" />



#### Update

###### 

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382cgjns2j20k00nugmx.jpg" alt="image-20210311111747916" style="zoom:50%;" />

#### Query

0-3

<img src="https://tva1.sinaimg.cn/large/008eGmZEgy1goft1ww37bj30m60l60x2.jpg" alt="image-20210311112917349" style="zoom:33%;" />

###### 2-3

<img src="https://tva1.sinaimg.cn/large/008eGmZEgy1goft01qfv6j30jq0i6q75.jpg" alt="image-20210311112729932" style="zoom: 33%;" />

##### 

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382cl74iwj20z20h2q48.jpg" alt="image-20210311113131649" style="zoom: 50%;" />

有可能可以「按需拆」



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382d5izkhj21gk0paae8.jpg" alt="image-20210311113433681" style="zoom: 33%;" />

<img src="https://tva1.sinaimg.cn/large/008eGmZEgy1goft7rju6xj30cq0bumyb.jpg" alt="image-20210311113455510" style="zoom: 50%;" />



- 空間：N + Nx(1 + 2 + 4 + ... N/4 + N/2 + N)



## Array version Fr Youtube

ref; https://youtu.be/e_bK-dgPvfM?t=555

**Pro: able to see the array as a tree after ` build_tree**







# Case

## Range Sum Query w/ F-Tree & S-TRee

```python
class FenwickTree(object):
    def __init__(self, n):
        self.sums_ = [0] * (n+1)
        self.n = n
        
    def update(self, i, delta):
        while i < len(self.sums_):
            self.sums_[i] += delta
            i += self._lowbit(i)
    
    def query(self, i):
        sum_ = 0
        while i > 0:
            sum_ += self.sums_[i]
            i -= self._lowbit(i)
        return sum_
    
    def _lowbit(self, x):
        return x & -x
    
# Confirm: index 0 Not Used, right?! 
class NumArray:
    def __init__(self, nums: List[int]):
        self.nums = nums
        self.tree = FenwickTree(len(nums))
        for i in range(len(nums)):
            self.tree.update(i+1, nums[i])

    def update(self, i: int, val: int) -> None:
        self.tree.update(i+1, val - self.nums[i])
        self.nums[i] = val

    def sumRange(self, i: int, j: int) -> int:
        return self.tree.query(j + 1) - self.tree.query(i)


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
```

```python
class SegmentTreeNode:
    def __init__(self, start, end, val, left=None, right=None):
        self.start = start
        self.end = end
        self.mid = start + (end - start) // 2
        self.val = val
        self.left = left
        self.right = right

class SegTree:
    def __init__(self, nums):
        self.nums = nums
        if self.nums:
            self.root = self._buildTree(0, len(nums) - 1)

    def update(self, i, val):
        self._updateTree(self.root, i, val)

    def sumRange(self, i, j):
        return self._sumRange(self.root, i, j)

    def _buildTree(self, start, end):
        if start == end: return SegmentTreeNode(start, end, self.nums[start])
        mid = start + (end - start) // 2
        left = self._buildTree(start, mid)
        right = self._buildTree(mid + 1, end)
        return SegmentTreeNode(start, end, left.val + right.val, left, right)

    def _updateTree(self, root, i, val):
        if root.start == i and root.end == i:
            root.val = val
            return
        if i <= root.mid:
            self._updateTree(root.left, i, val)
        else:
            self._updateTree(root.right, i, val)
        root.val = root.left.val + root.right.val

    def _sumRange(self, root, i, j):
        if root.start == i and root.end == j:
            return root.val
        if j <= root.mid:
            return self._sumRange(root.left, i, j)
        elif i > root.mid:
            return self._sumRange(root.right, i, j)
        else:
            return self._sumRange(root.left, i, root.mid) + self._sumRange(root.right, root.mid + 1, j)


class NumArray(object):
    # def __init__(self, nums: List[int]):
    def __init__(self, nums):
        self.tree = SegTree(nums)

    def update(self, i, val):
        self.tree.update(i, val)

    def sumRange(self, i, j):
        return self.tree.sumRange(i, j)
    # ref：https://leetcode-cn.com/problems/range-sum-query-mutable/solution/xian-duan-shu-by-a4613565/

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)

```



### Combined:

```python
class SegmentTreeNode(object):
    def __init__(self, start, end, val, left=None, right=None):
        self.start = start
        self.end = end
        self.mid = start + (end - start)//2
        self.val = val
        self.left = left
        self.right = right

class SegmentTree(object):
    def __init__(self, nums):
        self.nums = nums
        if self.nums:
            self.root = self._buildTree(0, len(nums) - 1)
            
    def update(self, i, val):
        self._update(self.root, i, val)
        
    def query(self, i, j):
        return self._query(self.root, i, j)
    
    def _buildTree(self, start, end):
        if start == end:
            return SegmentTreeNode(start, end, self.nums[start])
        mid = start + (end - start) // 2
        left = self._buildTree(start, mid)
        right = self._buildTree(mid + 1, end)
        return SegmentTreeNode(start, end, left.val + right.val, left, right)
    
    def _update(self, root, i, val):
        if root.start == i and root.end == i:
            root.val = val
            return 
        if i <= root.mid:
            self._update(root.left, i, val)
        else:
            self._update(root.right, i, val)
        root.val = root.left.val + root.right.val
        
    def _query(self, root, i, j):
        # print(root.start, root.end, root.mid, i, j)
        if root.start == i and root.end == j:
            return root.val
        if j <= root.mid:
            return self._query(root.left, i, j)
        elif i > root.mid:
            return self._query(root.right, i, j)
        else:
            return self._query(root.left , i, root.mid) + self._query(root.right, root.mid + 1, j)

class FenwickTree(object):
    def __init__(self, n):
        self.sums_ = [0] * (n+1)
        
    def update(self, i, delta):
        while i < len(self.sums_):
            # print('in while ', i, delta)
            self.sums_[i] += delta
            i += self._lowbit(i)
            # print('after update: {}'.format(self.sums_))
        
    def query(self, i):
        res = 0
        while i > 0:
            res += self.sums_[i]
            i -= self._lowbit(i)
        return res
    def _lowbit(self, x):
        return x & (-x)

class NumArray:
    def __init__(self, nums: List[int]):
        """ F-Tree """
        self.cur_nums = nums
        self.tree = FenwickTree(len(nums))
        for i in range(len(nums)):
            self.tree.update(i+1, nums[i])
        # print("INIT: ", self.tree.sums_)
        """ S-Tree """
        self.seg_tree = SegmentTree(nums)
        
    def update(self, i: int, val: int) -> None:
        """ F-Tree """
        self.tree.update(i + 1, val - self.cur_nums[i])
        self.cur_nums[i] = val
        
        """ S-Tree """
        self.seg_tree.update(i, val)
        
    def sumRange(self, i: int, j: int) -> int:
        """ F-Tree """
        # print(self.tree.query(j + 1), self.tree.query(i))
        return self.tree.query(j+1) - self.tree.query(i)
    
        """ S-Tree """
        return self.seg_tree.query(i, j)

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```



# MST

Usually, turn edge list into adj. list

- adj matrix: |V| x |V|
- adj list: |V| + |E|
- edge list: |E|

## Prim

![image-20200715184644350](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrulfnwylj30gk05ujt1.jpg)



![image-20200715184636907](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrulc6ffvj30im0b00w3.jpg)



Greedy!



![image-20200715185254214](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrus16rd6j31dk0ognax.jpg)



![image-20200715185312545](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggrusiuvkpj31iu0q41gm.jpg)



## Kruskal

![image-20200715230409210](https://tva1.sinaimg.cn/large/e6c9d24egy1h382cpdw9fj215c0fywg3.jpg)

![image-20200715230645534](https://tva1.sinaimg.cn/large/e6c9d24egy1h382csugkgj21b20h0juo.jpg)



# Union Find



![image-20200722100950385](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggziztev1mj30tu0fktew.jpg)



![image-20200722101052300](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggzj0tmac1j30pw0hitdb.jpg)

- path compression是從 find 出發的，平攤下來是反阿卡曼近乎O(1)



![image-20200722101240864](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggzj2pqk5dj30zo0gg7at.jpg)

- 把low rank tree 合併到high rank tree上去, rank是平均長度的概念，這樣在做path compression時的over head比較少
- merge by rank 也就是 link by rank



![image-20200722102700630](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggzjhmrcp4j30l00ie79r.jpg)

![image-20200722102725831](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggzji4hnntj30oc0hkq8o.jpg)

```python
class UF_w_rank(object):
    def __init__(self, n):
        self._parents = [idx for idx in range(n+1)]
        self._rank = [1 for idx in range(n+1)]
    def find(self, u):
        if u != self._parents[u]:
            self._parents[u] = self.find(self._parents[u])
        return self._parents[u]
    def union(self, u, v):
        pu, pv = self.find(u), self.find(v)
        if pu == pv:
            return False
        # self._parents[pv] = pu
        # return True
        if self._rank[u] > self._rank[v]:
            self._parents[pv] = pu
        elif self._rank[u] < self._rank[v]:
            self._parents[pu] = pv
        else:
            self._parents[pv] = pu
            self._rank[pu] += 1
            self._rank[pv] += 1
        return True
    
class UF(object):
    def __init__(self, n):
        self._parents = [idx for idx in range(n+1)]
        # self._rank
    def find(self, u):
        if u != self._parents[u]:
            self._parents[u] = self.find(self._parents[u])
        return self._parents[u]
    def union(self, u, v):
        pu, pv = self.find(u), self.find(v)
        if pu == pv:
            return False
        self._parents[pv] = pu
        return True
        
class Solution(object):
    def areSentencesSimilarTwo(self, words1, words2, pairs):
        """
        :type words1: List[str]
        :type words2: List[str]
        :type pairs: List[List[str]]
        :rtype: bool
        """
        m, n = len(words1), len(words2)
        if m != n or not pairs:
            return False
        uf = UF(len(pairs) * 2)
        # d = collections.defaultdict(int)
        d = {}
        for w1, w2 in pairs:
            # w1--> u
            # w2--> v
            # if w1 not in d:
            #     d[w1] = len(d)
            u = d[w1] = d.get(w1, len(d))
            v = d[w2] = d.get(w2, len(d))
            # print(w1,u,w2,v)
            uf.union(u, v)
            
        for i in range(m):
            w1, w2 = words1[i], words2[i]
            if w1 == w2:    # may both not in d, but are same
                continue
            # w1--> u
            # w2--> v
            if w1 not in d or w2 not in d:
                return False
            u, v = d[w1], d[w2]
            if uf.find(u) != uf.find(v):
                return False
        return True
        
        
    def BF():       # Not finished yet
        m, n = len(words1), len(words2)
        if m != n or not pairs:
            return False
        d = collections.defaultdict(set)
        for u, v in range(pairs):
            d[u].add(v)
            d[v].add(u)
            
        for i in range(m):
            if not self.is_similar(words1[i], words2[i], d):
                return False
        return True
    
    def is_similar(self, w1, w2, d):
        if w1 not in d and w2 not in d:
            return False
        if w1 in d:
            return self.is_similar( w2)
            # ==> many expansion!
        if w2 in d and w1 in d[w2]:
            return True
```

```python
""" LC 323 """
import collections
class DSU:
    def __init__(self, nums):
        # self.parents = [i for i in range(len(nums))]    # TODO: mapping, here I assume it continous first
        self.parents = {}
        for n in nums:
            self.parents[n] = n # 動態加呢 ?!　有這樣的題？
        self.ranks = {}
        for n in nums:
            self.ranks[n] = 1
            
    def find(self, x):
        if self.parents[x] != x:
            self.parents[x] = self.find(self.parents[x])    # set my grandpa as my parent
        return self.parents[x]
    
    def union(self, x, y):
        # print(x, y)
        # px, py = self.parents[x], self.parents[y] # BUG... XD
                                                  # CAUGHT by CASE↓
                                                    # 4
                                                    # [[1,2],[0,1],[0,2]]
        px, py = self.find(x), self.find(y)
        if px != py:
            # self.parents[y] = px      # BAD... CAUGHT by CASE↓; 怎麼記case或形象好？
                                            # FAILED at: 
                                                # 5
                                                # [[0,1],[2,1],[2,0]]; I return 2
                                                    # REASON:
                                                    # 0 <- 1        parents:    0  0   2
                                                    # 2 <- 0 <- 1   parents:    0
                                            # THOUGH PASS at:
                                                # 5
                                                # [[1,2],[0,1],[0,2]]; I return 3
            self.parents[py] = px       
            return True
        return False
    
    def union_w_rank(self, x, y):
        px, py = self.find(x), self.find(y)
        if px != py:
            rx, ry = self.ranks[x], self.ranks[y]
            if rx > ry:
                self.parents[py] = px
            elif rx < ry:
                self.parents[px] = py
            else:
                self.parents[px] = py
                self.ranks[py] += 1
            return True
        return False    

class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        # return self.my_20210505(n, edges)   # AC     問題：dsu裡面的東西有沒有必要是全部的members?    
        # return self.my_20210505_B(n, edges)   # AC；　↑　不必然, 只塞了邊的點，孤立的點就沒放進去DSU
        # return self.my_20210505_C(n, edges) # AC;    正的算，看最後有幾個代表，ps,一開始有把所有點都先弄出來
        # return self.my_20210505_D(n, edges) # AC     正的算，但一開始只把edge上看到的端點放進去，後面在找代表的點的時候要是發現沒在 uf裡，就知道它是個孤立點
        # return self.my_20210505_E(n, edges) # DFS AC
        return self.my_20210505_F(n, edges) # BFS   AC

    def my_20210505_E(self, n, edges):
        visited = set([])
        outs = collections.defaultdict(list)
        for i in range(len(edges)):
            e0, e1 = edges[i]
            outs[e0].append(e1)
            outs[e1].append(e0)
        
        # print(outs)
        count = 0
        for i in range(n):
            if i not in visited:
                self.my_20210505_dfs(i, 0, outs, visited)
                # visited.add(i)
                count += 1
            # print(i, count)
        return count
    
    def my_20210505_dfs(self, now, idx, outs, visited):
        visited.add(now)
        for nbr in outs[now]:
            # if now not in visited:    BUG
            if nbr not in visited:
                self.my_20210505_dfs(nbr, idx + 1, outs, visited)    
    
    def my_20210505_F(self, n, edges):
        visited = set([])
        outs = collections.defaultdict(list)
        for i in range(len(edges)):
            e0, e1 = edges[i]
            outs[e0].append(e1)
            outs[e1].append(e0)
        
        # print(outs)
        count = 0
        for i in range(n):
            if i not in visited:
                # DO BFS
                Q = collections.deque([])
                Q.append(i)
                visited.add(i)
                while Q:
                    now = Q.popleft()
                    for nbr in outs[now]:
                        if nbr not in visited:  # Don't forget!!!
                            Q.append(nbr)
                            visited.add(nbr)
                # END BFS
                count += 1
        return count
    
        
    
    def my_20210505_D(self, n, edges):
        all_nodes = set([])       # 這不是真正的all_nodes了，只是 all_edge_nodes
        for i in range(len(edges)):
            fr, to = edges[i]
            all_nodes.add(fr)
            all_nodes.add(to)

        uf = DSU(all_nodes)    # always have to put all members into DSU first?!
        for i in range(len(edges)):
            # print(i)
            fr, to = edges[i]
            uf.union(fr, to)   # should seperate this LOGICS! NOT Here            
            # uf.union_w_rank(fr, to)   # should seperate this LOGICS! NOT Here
                
        res = set()
        for i in range(n):
            try:
                res.add(uf.find(i))
            except:
                res.add(i)
        return len(res)        
    
    def my_20210505_C(self, n, edges):
        all_nodes = []
        [all_nodes.append(i) for i in range(n)]
        
        uf = DSU(all_nodes)    # always have to put all members into DSU first?!
        for i in range(len(edges)):
            # print(i)
            fr, to = edges[i]
            uf.union(fr, to)   # should seperate this LOGICS! NOT Here            
            # uf.union_w_rank(fr, to)   # should seperate this LOGICS! NOT Here
                
        res = set()
        for i in range(n):
            res.add(uf.find(i))
        return len(res)

    def my_20210505_B(self, n, edges):
        all_nodes = set([])       # 這不是真正的all_nodes了，只是 all_edge_nodes
        for i in range(len(edges)):
            fr, to = edges[i]
            all_nodes.add(fr)
            all_nodes.add(to)

        dsu = DSU(all_nodes)    # always have to put all members into DSU first?!
        union_count = 0
        for i in range(len(edges)):
            # print(i)
            fr, to = edges[i]
            if dsu.union_w_rank(fr, to):
                union_count += 1
        return n - union_count
        # return len(all_nodes) - union_count
        
    
    def my_20210505(self, n, edges):
        # max_num = 0
        # for i in range(n):
        #     fr, to = edges[i]
        #     max_num = max(max_num, fr, to)
        # all_nodes = []
        # [all_nodes.append(i) for i in range(max_num)]
        
        # all_nodes = set([])       # BUG, if any one node is not list in edges
        # for i in range(len(edges)):
        #     fr, to = edges[i]
        #     all_nodes.add(fr)
        #     all_nodes.add(to)
        all_nodes = []
        [all_nodes.append(i) for i in range(n)] # 一定這樣先把all_nodes生出來先嗎？
        # print(all_nodes)
        # assume there's not repeated connections in edges
        # return len(all_nodes) - n     # WRONG, 要是邊在已連著的同個島的兩點了，就不該再被減去，不然就多減了
        
        dsu = DSU(all_nodes)    # always have to put all members into DSU first?!
        union_count = 0
        for i in range(len(edges)):
            # print(i)
            fr, to = edges[i]
            if dsu.union_w_rank(fr, to):   # should seperate this LOGICS! NOT Here
                union_count += 1      # 不是在這裡作這個判斷的
                                    # 不不不，其實可以過；但要是 union像剛剛那樣寫了，
                                    # 可能要自圓的話就得下面再作一次所有人的find的判斷 (?!) --> DISCUSS
        # print('union_count: ', union_count)
        return len(all_nodes) - union_count
        
            
        
```



