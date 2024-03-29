---
layout: post
categories: Languages
tag: []
date: 2020-10-22
Author: Joe
---

#### Hard, to solve: 
    - 301.remove-invalidparantheses TLE for enumeration w/ backtracking, 
    - 140.word-break-ii a)MLE w/ case "a"*n & b)Trie method (Trie not faster for single query, but for multi query in 472.)
    - SegmentTree
    - BinaryIndexedTree
    - Letter Combinatino of A Phone Number

|                         | Python3                                                      | Cpp                                                          | JS                                                           | Java                                                         | Scala                                   |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------------------------- |
| **dict**                |                                                              | include <unordered_map>, //Similar to defaultdict() in python3 |                                                              |                                                              | import scala.collection.mutable.HashMap |
|                         | d = {}                                                       | unordered_map<Node*> d;                                      | d = {}; new Map();                                           | Map<Node, Node> visited = new HashMap();                     | var lookup = new HashMap[Int, Int]()    |
|                         | d[k] = v;                                                    | d[k] = v;                                                    | d[k]=v; map.**set**(0,1)                                     | visited.**put**(k, v)                                        | lookup.put(nums(i), i)                  |
|                         | d[k]; d.get(k,0)                                             | d[k];                                                        | d[k]; map.get(k)                                             | visited.get(cur_O)                                           |                                         |
|                         | k not in d                                                   | (d.find(k)==d.end())                                         | !(nbr_O.val in visited)                                      | !visited.containsKey(nbr_O)                                  |                                         |
|                         | k in d                                                       | (d.find(k)!=d.end())                                         | nbr_O.val in visited; (map.**has**(sum-k))                   | visited.**containsKey**(nbr_O)                               | lookup.contains(diff)                   |
|                         | del d[k]                                                     | d.**erase**(key)                                             | this.d.**delete**(key);                                      | this.d.**remove**(key);                                      |                                         |
|                         | len(d)                                                       | size()                                                       | d.**size**                                                   | d.size()                                                     |                                         |
|                         |                                                              | d.empty()  //true; false                                     |                                                              |                                                              |                                         |
| NotOK                   |                                                              |                                                              | {k:v} in Initialization                                      |                                                              |                                         |
| Special APIs            |                                                              | reserve(capacity)                                            |                                                              |                                                              |                                         |
| Iterate                 | for k in d.keys():                                           | for (auto k: d)                                              | for (var key in dict){}                                      | for(char c:  ransomNote.toCharArray()) {                     | for (i <- 0 until n)                    |
| **Ordered_dict**        | self.od.popitem(last=False)<br />https://www.pythonf.cn/read/89970<br />https://gist.github.com/joequery/12332f410a05e6c7c949 |                                                              | d = new Map(); APIs↑↑ //ref: https://stackoverflow.com/questions/2798893/ordered-hash-in-javascript | LinkedHashMap<Integer, Integer> d;<br />this.d = new LinkedHashMap<>(); |                                         |
|                         | first = next(iter(self.od))                                  |                                                              |                                                              | Integer first = this.d.keySet().iterator().next();           |                                         |
| **queue**               | from collections import deque                                | include  <deque>                                             |                                                              |                                                              |                                         |
|                         | deque();                                                     | deque<Node*>                                                 | let Q = [];                                                  | Queue<Node> Q = new LinkedList(); //LinkedList<Node> Q = new LinkedList<Node>(); //雙向 |                                         |
|                         | Q.append(cur); v=Q.popleft()                                 | Q.push_back(cur); v=Q.front(),Q.pop_front()                  | Q.push(cur); v=Q.shift(); // O(n)                            | Q.add(nbr_O);//Q.push(nbr_O); Q.poll();                      |                                         |
|                         | while d                                                      | while (!Q.empty())                                           | while (Q.length != 0)                                        | while (!Q.isEmpty())                                         |                                         |
| **deque**               |                                                              |                                                              |                                                              | ArrayDeque雙向實現了DEque的interface                         |                                         |
| **vector**              |                                                              | include <vector>                                             |                                                              |                                                              |                                         |
|                         | self.res = []                                                | vector<vector<int>> result;                                  | res = [];                                                    | List<List<Integer>> res = new ArrayList<List<Integer>>();    |                                         |
|                         | item.append(nums[idx])                                       | item.push_back(nums[idx]);                                   | item.push(nums[idx]);                                        | item.add(nums[idx]);                                         |                                         |
|                         | item.pop()                                                   | item.pop_back();                                             | item.pop();                                                  | item.remove(item.size()-1);                                  |                                         |
|                         |                                                              |                                                              | l.length; // 注意！跟dict不同，這邊是 length                 |                                                              |                                         |
|                         |                                                              | for                                                          | for (***let*** i=0; i<l.length; i++)                         |                                                              |                                         |
| **Array**               | l = [False] * 3                                              | vector<bool> dp(3, false);                                   | const dp = new Array(3).fill(false);                         | List<Boolean> dp = new ArrayList(Collections.nCopies(s.length(), false)); | return Array(lookup(diff), i)           |
|                         |                                                              |                                                              |                                                              | int[] nums;, nums.length                                     |                                         |
| **Doublely LinkedList** |                                                              | **std::list**<br />**List** stores elements at non contiguous memory location i.e. it internally uses a doubly linked list i.e. |                                                              |                                                              |                                         |
|                         |                                                              | back(); front();                                             |                                                              |                                                              |                                         |
|                         |                                                              | pop_back(); pop_front();                                     |                                                              |                                                              |                                         |
|                         |                                                              | db_ll.erase(d[key])                                          |                                                              |                                                              |                                         |
| **APIs**                | from collections import Counter<br />Counter(l)              | X                                                            | _.countBy(l)                                                 | X                                                            |                                         |
|                         | max                                                          | max                                                          | Math.max                                                     | Math.max                                                     |                                         |
| **String**              | len(s)                                                       | s.length()                                                   | s.length                                                     | s.length()                                                   |                                         |
|                         | s[i]                                                         | s[i]                                                         | s[i]                                                         | s.charAt(i)                                                  |                                         |
|                         | s[3:5]                                                       | token = s.substr(start, i-start+1);                          | s.slice(3, 5);                                               | s.substring(start, i+1);                                     |                                         |
| **Set**                 | ws = set(wD)                                                 | unordered_set<string> ws;<br/>        <br/>        for (auto s: wordDict){<br/>            ws.insert(s);<br/>        } | let ws = new Set(wD)                                         | HashSet<String> ws = new HashSet(wordDict);                  |                                         |
|                         | token in ws                                                  | ws.find(token) != ws.end()                                   | ws.has(token)                                                | ws.contains(token)                                           |                                         |
| **MAX**                 | float('-inf')                                                | INT_MIN;                                                     | -Number.MAX_VALUE;                                           | Integer.MIN_VALUE;                                           |                                         |
| **2D Init**             | [[0 for j in range(n)] for i in range(m)]                    |                                                              |                                                              | [...Array(3)].map(x=>Array(5).fill(0))                       |                                         |
| **1D sort**             | l.sort(reverse=True)                                         |                                                              |                                                              |                                                              |                                         |
| **Node**                | if not head                                                  | if (!head); if (nullputr == head)                            | if (!root); if (head == null)                                | if (head == null)                                            |                                         |



## Special issues:

- Java & js
- <img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gegmwb46qaj312a0u0qb1.jpg" alt="image-20200504191823941" style="zoom:67%;" />





## Python3 __lt__

```python
TreeNode.__lt__ = lambda me, other: me.val < other.val
```

e.g. in LC-1305

```python
TreeNode.__lt__ = lambda me, other: me.val < other.val	# for the "sort!"
class Solution:
    def getAllElements(self, root1: TreeNode, root2: TreeNode) -> List[int]:
        return self.my_20220125(root1, root2)   # AC
        return self.my_20220125_B(root1, root2) # AC
    def my_20220125_B(self, root1, root2):
        all_nodes1 = self.in_order(root1)
        all_nodes2 = self.in_order(root2)
        i = j = 0
        m, n = len(all_nodes1), len(all_nodes2)
        res = []
        while i < m or j < n:
            a = b = sys.maxsize
            if i < m:
                a = all_nodes1[i].val
            if j < n:
                b = all_nodes2[j].val
            
            if a <= b:
                res.append(a)
                i += 1
            else:
                res.append(b)
                j += 1
        return res
                
    def in_order(self, root):
        if not root:
            return []
        
        stack = []
        cur = root
        while cur:
            stack.append(cur)
            cur = cur.left
        
        res = []
        while stack:
            now = stack.pop()
            res.append(now)
            cur = now.right
            while cur:
                stack.append(cur)
                cur = cur.left
        return res
        
    def my_20220125(self, root1, root2):
        all_nodes1 = self.get_all_nodes(root1)
        all_nodes2 = self.get_all_nodes(root2)
        # print(all_nodes1, all_nodes2)
        all_nodes1.extend(all_nodes2)
        all_nodes1.sort()
        return [n.val for n in all_nodes1]
        res = []
        return sorted([node.val for node in all_nodes1])

    def get_all_nodes(self, root):
        if not root:
            return []
        cur = root
        stack = [cur]
        res = []
        while stack:
            now = stack.pop()
            res.append(now)
            for child in [now.right, now.left]:
                if child:
                    stack.append(child)
        return res
        
```



### Lintcode 391

```python
Interval.__lt__ = lambda self, other: self.end < other.end 
class Solution:
    """
    @param airplanes: An interval array
    @return: Count of airplanes are in the sky.
    """
    def countOfAirplanes(self, airplanes):
        # write your code here
        # return self.my_20211111(airplanes)  # AC
        # return self.my_20211111_B(airplanes)    # AC
        # return self.my_20211111_C(airplanes)    # AC
        return self.my_20220802(airplanes)

    def my_20220802(self, airplanes):
        # airplanes.sort(key=lambda x: x[0])
        airplanes.sort(key=lambda x: x.start)
        n = len(airplanes)
        landing_times = []
        cnt = 0
        for i in range(n):
            # if not landing_times or landing_times[-1] > airplanes[i].end: # BUGGG!
            if not landing_times or landing_times[0] > airplanes[i].start: # FIX!
                # landing_times.append(airplanes[i].end)
                heapq.heappush(landing_times, airplanes[i].end)
            else:
                heapq.heappush(landing_times, airplanes[i].end)
                heapq.heappop(landing_times)
            cnt = max(cnt, len(landing_times))
        return cnt

```

