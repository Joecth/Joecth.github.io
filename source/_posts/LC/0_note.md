---
layout: post
categories: Languages
tag: []
date: 2023-04-22
Author: Jo
---




[toc]

# Cross Language Table

|                               | Python3★                                                                                                                 | Cpp                                                                                                                                         | JS                                                                                                    | Java                                                                                                |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **dict**                |                                                                                                                               | include <unordered_map>, //Similar to defaultdict() in python3                                                                              |                                                                                                       |                                                                                                     |
|                               | d = {}                                                                                                                        | unordered_map<Node*> d;                                                                                                                     | d = {}; new Map();                                                                                    | Map<Node, Node> visited = new HashMap();                                                            |
|                               | d[k] = v;                                                                                                                     | d[k] = v;                                                                                                                                   | d[k]=v; map.**set**(0,1)                                                                        | visited.**put**(k, v)                                                                         |
|                               | d[k]; d.get(k,0)                                                                                                              | d[k];                                                                                                                                       | d[k]; map.get(k)                                                                                      | visited.get(cur_O)                                                                                  |
|                               | k not in d                                                                                                                    | (d.find(k)==d.end())                                                                                                                        | !(nbr_O.val in visited)                                                                               | !visited.containsKey(nbr_O)                                                                         |
|                               | k in d                                                                                                                        | (d.find(k)!=d.end())                                                                                                                        | nbr_O.val in visited; (map.**has**(sum-k))                                                      | visited.**containsKey**(nbr_O)                                                                |
|                               | del d[k]                                                                                                                      | d.**erase**(key)                                                                                                                      | this.d.**delete**(key);                                                                         | this.d.**remove**(key);                                                                       |
|                               | len(d)                                                                                                                        | size()                                                                                                                                      | d.**size**                                                                                      | d.size()                                                                                            |
|                               |                                                                                                                               | d.empty()  //true; false                                                                                                                    |                                                                                                       |                                                                                                     |
| NotOK                         |                                                                                                                               |                                                                                                                                             | {k:v} in Initialization                                                                               |                                                                                                     |
| Special APIs                  |                                                                                                                               | reserve(capacity)                                                                                                                           |                                                                                                       |                                                                                                     |
| Iterate                       | for k in d.keys():                                                                                                            | for (auto k: d)                                                                                                                             | for (var key in dict){}                                                                               | for(char c:  ransomNote.toCharArray()) {                                                            |
| **Ordered_dict**        | self.od.popitem(last=False)<br />https://www.pythonf.cn/read/89970<br />https://gist.github.com/joequery/12332f410a05e6c7c949 |                                                                                                                                             | d = new Map(); APIs↑↑ //ref: https://stackoverflow.com/questions/2798893/ordered-hash-in-javascript | LinkedHashMap<Integer, Integer> d;<br />this.d = new LinkedHashMap<>();                             |
|                               | first = next(iter(self.od))                                                                                                   |                                                                                                                                             |                                                                                                       | Integer first = this.d.keySet().iterator().next();                                                  |
| **queue**               | from collections import deque                                                                                                 | include`<deque>`                                                                                                                          |                                                                                                       |                                                                                                     |
|                               | deque();                                                                                                                      | deque<Node*>                                                                                                                                | let Q = [];                                                                                           | Queue`<Node>` Q = new LinkedList(); //LinkedList`<Node>` Q = new LinkedList`<Node>`(); //雙向 |
|                               | Q.append(cur); v=Q.popleft()                                                                                                  | Q.push_back(cur); v=Q.front(),Q.pop_front()                                                                                                 | Q.push(cur); v=Q.shift(); // O(n)                                                                     | Q.add(nbr_O);//Q.push(nbr_O); Q.poll();                                                             |
|                               | while d                                                                                                                       | while (!Q.empty())                                                                                                                          | while (Q.length != 0)                                                                                 | while (!Q.isEmpty())                                                                                |
| **deque**               |                                                                                                                               |                                                                                                                                             |                                                                                                       | ArrayDeque雙向實現了DEque的interface                                                                |
| **pq** | heapq.heappush() | | |  |
| **vector**              |                                                                                                                               | include`<vector>`                                                                                                                         |                                                                                                       |                                                                                                     |
|                               | self.res = []                                                                                                                 | vector<vector`<int>`> result;                                                                                                             | res = [];                                                                                             | List<List`<Integer>`> res = new ArrayList<List`<Integer>`>();                                   |
|                               | item.append(nums[idx])                                                                                                        | item.push_back(nums[idx]);                                                                                                                  | item.push(nums[idx]);                                                                                 | item.add(nums[idx]);                                                                                |
|                               | item.pop()                                                                                                                    | item.pop_back();                                                                                                                            | item.pop();                                                                                           | item.remove(item.size()-1);                                                                         |
|                               |                                                                                                                               |                                                                                                                                             | l.length; // 注意！跟dict不同，這邊是 length                                                          |                                                                                                     |
|                               |                                                                                                                               | for                                                                                                                                         | for (***let*** i=0; i<l.length; i++)                                                          |                                                                                                     |
|                               | l = [False] * 3                                                                                                               | vector`<bool>` dp(3, false);                                                                                                              | const dp = new Array(3).fill(false);                                                                  | List`<Boolean>` dp = new ArrayList(Collections.nCopies(s.length(), false));                       |
|                               |                                                                                                                               |                                                                                                                                             |                                                                                                       | int[] nums;, nums.length                                                                            |
| **Doublely LinkedList** |                                                                                                                               | **std::list**<br />**List** stores elements at non contiguous memory location i.e. it internally uses a doubly linked list i.e. |                                                                                                       |                                                                                                     |
|                               |                                                                                                                               | back(); front();                                                                                                                            |                                                                                                       |                                                                                                     |
|                               |                                                                                                                               | pop_back(); pop_front();                                                                                                                    |                                                                                                       |                                                                                                     |
|                               |                                                                                                                               | db_ll.erase(d[key])                                                                                                                         |                                                                                                       |                                                                                                     |
| **APIs**                | from collections import Counter<br />Counter(l)                                                                               | X                                                                                                                                           | _.countBy(l)                                                                                          | X                                                                                                   |
|                               | max                                                                                                                           | max                                                                                                                                         | Math.max                                                                                              | Math.max                                                                                            |
| **String**              | len(s)                                                                                                                        | s.length()                                                                                                                                  | s.length                                                                                              | s.length()                                                                                          |
|                               | s[i]                                                                                                                          | s[i]                                                                                                                                        | s[i]                                                                                                  | s.charAt(i)                                                                                         |
|                               | s[3:5]                                                                                                                        | token = s.substr(start, i-start+1);                                                                                                         | s.slice(3, 5);                                                                                        | s.substring(start, i+1);                                                                            |
| **Set**                 | ws = set(wD)                                                                                                                  | unordered_set`<string>` ws;``        ``        for (auto s: wordDict){``            ws.insert(s);``        }  | let ws = new Set(wD)                                                                                  | HashSet`<String>` ws = new HashSet(wordDict);                                                     |
|                               | token in ws                                                                                                                   | ws.find(token) != ws.end()                                                                                                                  | ws.has(token)                                                                                         | ws.contains(token)                                                                                  |
| **MAX**                 | float('-inf')                                                                                                                 | INT_MIN;                                                                                                                                    | -Number.MAX_VALUE;                                                                                    | Integer.MIN_VALUE;                                                                                  |
| **2D Init**             | [[0 for j in range(n)] for i in range(m)]                                                                                     |                                                                                                                                             |                                                                                                       | [...Array(3)].map(x=>Array(5).fill(0))                                                              |
| **1D sort**             | l.sort(reverse=True)                                                                                                          |                                                                                                                                             |                                                                                                       |                                                                                                     |
| **Node**                | if not head                                                                                                                   | if (!head); if (nullputr == head)                                                                                                           | if (!root); if (head == null)                                                                         | if (head == null)                                                                                   |

## Special issues:

- Java & js



## C++ Syntax Review

- APIS:

  - isalnum(s[i])
  
  - ```cpp
    if (board[i][j] == '.') continue;
    
    // int val = int(board[i][j]); 		// WRONG
    int val = int(board[i][j] - '0');	
    
    // if (used & (1 << val) > 0) return false;	// WRONG
    if ((used & (1 << val)) > 0) return false;
  
  - INT_MIN, isalnum(str\[i\]), swap(matrix[i][j], matrix[j][i]); reverse(matrix[i].begin(), matrix[i].end());
  - int end = upper_bound(nums.begin(), nums.end(), target - x) - nums.begin();
  - const size_t n;
  - auto max_num = *max_element(candies.begin(), candies.end())
  
- OVERFLOW:

  - res += ((long long)pow(2, end_cnt) % M);  **FIX** in ↓

    ```cpp
    vector<int> p(n, 1);
    for (int i = 1; i < n; i++){
      p[i] = p[i - 1] * 2  % M;
    }
    
    res += p[end_cnt];
    ```
  
- Hash

  - unordered_map<int, int> lookup;
  
  - unordered_map<char, char> m = { {'(', ')'}, {'[', ']'}, {'{', '}'} }; // 注意用的是逗號不是冒號!
  
  - if (lookup.find(target - nums[i]) != lookup.end()){
  
  - for (auto item: m){
  
    ​		res.push_back(item.second);  // 注意！是second, 不是 second()

    }
  
  - private:
  
    ​		unordered_set`<int>` s;
  
  - s.erase(key);
  
  - unordered_set`<string>` lookup(wordDict.begin(), wordDict.end());
  
  - unordered_set<pair<int, int>, #SHOULD PROVIDE HASH HERE...> visited; // so, change to use
  
  - Map
  
    - ```cpp
          vector<vector<pair<int, int>>>updates;
      int idx = upper_bound(updates[index].begin(), updates[index].end(), make_pair(snap_id,INT_MAX)) - updates[index].begin();
      ```
  
    - for (auto &[k, v]: m){
  
    - map<char, int, less<char>> m;
  
    - ※ 可怕的bug! 會讓 map裡的key沒被traverse到 
  
      ```cpp
      // if (seen.find(diff) == seen.end()) {
      if (seen.count(diff) == 0) {
      // res += min(m[diff], m[key]); // BUGGGG!!!! 這會自己幫map新加key為diff的elements … 然後就會打亂本來有的key的traverse的節奏；像這邊，就是6最後沒被traverse到…
      ```
  
      
  
- DP

  - vector`<int>` dp = {1, 1, 2};
  - vector`<int>` dp(n + 1);
  - vector<vector`<int>`> dp(n, vector`<int>`(2));
  
- Bits

  - uint32_t x = n;
  - ```cpp
    int num = 5;
    bool a = false;
    cout << typeid(a).name() << endl;
    std::bitset<8> binary(num);  // 將數字轉換為8位元的二進制
    std::cout << "Binary representation of " << num << ": " << binary << std::endl;
    // printf("%s", binary.to_string());
    for (int i = 0; i < binary.size(); i++){
        if (binary.test(i))
            printf("i: %d,  \n", i);
    }
    
    string s = binary.to_string();
    for (int i = 0; i < s.size(); i++){
        if (s[i] == '1')
            printf("i: %d,  \n", i);
    }  
    
    return 0;
    
    ---------------------------------------------
    Hello world!b
    Binary representation of 5: 00000101
    i: 0,  
    i: 2,  
    i: 5,  
    i: 7,  
    
    int minFlips(int a, int b, int c) {
      bitset<32> bin_a(a), bin_b(b), bin_c(c);
      size_t cnt = 0;
      for (int i = 0; i < 32; i++){
        if ((bin_a.test(i) | bin_b.test(i)) != bin_c.test(i)){
          if (bin_c.test(i) == 1) cnt += 1;
          else cnt += bin_a.test(i) + bin_b.test(i);
        }
      }
      return cnt;
    }
    ```
  
- Matrix

  - if (matrix.size() == 0 || matrix[0].size() == 0) return {};
  - int m = matrix.size(), n = matrix[0].size();
  - int DIRECTIONS\[4]\[2] = { {0, 1}, {1, 0}, {0, -1}, {-1, 0} };
  - seen.insert(i * n + j);
  - seen.find(ni * n + nj) != seen.end()
  - if (grid[i][j] == '1' && (visited.find(i * n + j) == visited.end())){
  - PII now = q.front(); q.pop();

    int i = now.first, j = now.second;

    // printf("%d, %d\n", r, c);

    for (auto dir:DIRECTIONS){

    ```cpp
    // int di = dir.first, nj = dir.second;  // WRONG
    
    int di = dir[0], dj = dir[1];   // FIX
    ```
  
- LINEAR -- String, or Arr, with Vector as Stack, prefer to just use vector to make API name easier

  - Vector

    - v.push_back(x)
    - v.back() // 注意，API 不是top(); v.pop_back() // 注意，不是pop()
    - // vector`<int>` right(n) = {1}; // WRONG!

      vector`<int>` left(n, 1);
    - string dfs_and_compare(TreeNode* root, string sub_pat, vector`<bool>` &res){. // Don't Forget `&`!
    - ```cpp
      private:
          vector<int> p;//(3); 宣告不可指定大小
      public:
          ParkingSystem(int big, int medium, int small) {
              p = {big, medium, small};
          }
      ```
    - vector`<int>`& tail = res.back();
      tail[1] = max(e, prev_end);
    - private:
      
      ​    vector<map<int, int>> arr;
      
      public:
      
      ​    SnapshotArray(int length) {
      
      ​        // arr(length);	// WRONG!
      ​		arr.resize(length)  // FIX!
  - Queue

    - while (!q.empty())
    - q.push(nbr)
    - q.front() // 注意，不是 top() ; q.pop()
    - // for(auto &x: q) {    NO trvs queue in CPP!
  - String
  
    - item.pop_back();
    - for (char c: s){
    - to_string(root->val)
    - string pat = "(" + l + " " + to_string(root->val) + " " + r + ")";
    - if (pat == sub_pat){
    
      res[0] = true;
      
      }
    - string tmp = s;  // COPY a string
    - isalnum(s[i]);
    - tolower(s[i]) != tolower(s[j])
    - return s.substr(lo, hi - lo+1);
    - const int bi = i < b.size() ? b[i] - '0' : 0;
    - result.insert(result.begin(), val + '0');
    - // if (lookup.find(s.substr(i, j)) != lookup.end()){    // BUG!! BE CAREFUL!!!
    
      ​	if (lookup.find(s.substr(i, j - i)) != lookup.end()){ // FIX!
    - ```cpp
      class Codec {
      public:
      
          // Encodes a list of strings to a single string.
          string encode(vector<string>& strs) {
          std::string encoded;
          for (const std::string& str : strs) {
            encoded += str + "\n";
          }
          return encoded;
          }
      
          // Decodes a single string to a list of strings.
          vector<string> decode(string s) {
              stringstream ss(s);
              string word;
              vector<string> res;
              // while (ss >> word){
              char delim = '\n';
              while (getline(ss, word, delim)){
                  res.push_back(word);
              }
              return res;
          }
      };
      
      ```
  
- PQ, Top-K & Comparator

  - // printf("%d %d\n", x.first, x.second);
  - private:

        priority_queue`<int>` pq;
        
        int sz;
  - ```cpp
    #define pii pair<int,int>
    #define tii tuple<int,int>
    
    class Solution {
    public:
        static bool cmp(pair<int, int>& m, pair<int, int>& n) {
            return m.second > n.second;
        }  
        vector<int> topKFrequent(vector<int>& nums, int k) {
            // return sol_20230523(nums, k);
            return py_20230523(nums, k);
        }
    
        vector<int> sol_20230523(vector<int>& nums, int k) {    // AC!
            unordered_map<int, int> counter;
            for (auto x: nums) counter[x]++;
    
            int n = nums.size();
    
            // priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
            // auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {
            //     return a.second > b.second; // Sort in descending order of frequency
            // };      
          
            // priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(&cmp)> pq(cmp);
    	      //auto cmp = [](ListNode * left, ListNode * right){
    				//		return left->val > right->val;
    				//};
    				//priority_queue<ListNode *, vector<ListNode*>, decltype(cmp)> heap(cmp);
    
          	priority_queue<pii, vector<pii>, decltype(&cmp)> pq(cmp);
            for (auto x: counter){
                // printf("%d %d\n", x.first, x.second);
                pq.push(x);
                if (pq.size() > k) pq.pop();
            }
            vector<int> res;
            while (!pq.empty()){
                res.push_back(pq.top().first);
                pq.pop();
            }
    
            return res;
        }
    
        vector<int> py_20230523(vector<int>& nums, int k) {     // AC!
            unordered_map<int, int> counter;
            for (auto x: nums) counter[x]++;
    
            int n = nums.size();
    
            // priority_queue<pii> pq;
            priority_queue<tii> pq;
            for (auto x: counter){
                // printf("%d %d\n", x.first, x.second);
                // pq.push(pair(-x.second, x.first));
                // if (pq.size() > k) pq.pop();
                pq.push(tuple(-x.second, x.first));
                if (pq.size() > k) pq.pop();
            }
    
            vector<int> res;
            while (!pq.empty()){
                // res.push_back(pq.top().second);
                res.push_back(get<1>(pq.top()));
                pq.pop();
            }
    
            return res;
        }  
    };
    
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            // write your code here
            int n = lists.size();
            if (n == 0){
                return NULL;
            }
            auto cmp = [](ListNode * left, ListNode * right){
                return left->val > right->val;
            };
            priority_queue<ListNode *, vector<ListNode*>, decltype(cmp)> heap(cmp);
            for (int i = 0; i < n; i++){
                if (lists[i] != NULL){
                    heap.push(lists[i]);
                }
            }
            // ListNode DummyNode(0);
            ListNode * DummyNode = new ListNode(0);
            ListNode * tail = DummyNode;
            while(!heap.empty()){
                ListNode * node = heap.top();
                heap.pop();
                tail->next = node;
                tail = tail->next;
                if (node->next){
                    heap.push(node->next);
                }
            }
            return DummyNode->next;
        }
    ```
  - auto cmp = [](ListNode * left, ListNode * right){
  
    ​		return left->val > right->val;
  
    };
  
    priority_queue<ListNode *, vector<ListNode*>, decltype(cmp)> heap(cmp);
  
- Interval

  - int s = interval[0], e = interval[1];
  
- LinkedList

  - printf("var[%d] = %x\n", i - 1, ptr);
  - printf("var[%d] = %d\n", i - 1, *ptr);
  - if (!head || !head->next) return head;
  - // ListNode* my_20230520_recursive(ListNode* prev, ListNode* head, vector<ListNode*> res) { // ERROR!

    ListNode* my_20230520_recursive(ListNode* prev, ListNode* head, vector<ListNode*> &res) {   // FIX by adding & for `res`
  - // vector<ListNode*> res; //runtime error: reference binding to null pointer of type 'ListNode *' (stl_vector.h)

    vector<ListNode*> res(1);	// FIX
  
- Tree

  - bool ans = dfs(root, LONG_MIN, LONG_MAX);
    bool dfs(TreeNode* root, long lo, long hi){        // FIX!, 別用 INT_MIN, INT_MAX
  
- Graph

  - vector<vector`<int>`> g(numCourses);
  - Node* tmp = new Node(node->val);    // FIX!, dont' forget `new` to create the "pointer"
  - void dfs(vector<vector`<int>`> &graph, int node, unordered_set`<int>` &visited){    // FIX! copy of the graph in every dfs call will lead to TLE!
  - vector<vector`<int>`> graph(n); // WRONG --> vector<vector`<int>`> graph(n, vector`<int>`);
  - `<u>`to be a tree`</u>`, shoud be (edges.size() + 1 != n)



# Best Record

![image-20230602210901526](https://p.ipic.vip/z75ght.png)
