---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-09-25
---



## 208 Trie I

```python
import collections
class TrieNode:
    def __init__(self):
        self.children = collections.defaultdict(TrieNode)
        self.is_end = False
        

class Trie:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()
        

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        cur = self.root
        for c in word:
            cur = cur.children[c]
        cur.is_end = True
        return None

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        cur = self.root
        for c in word:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return cur.is_end

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        cur = self.root
        for c in prefix:
            if c not in cur.children:
                return False
            cur = cur.children[c]
        return True
      



## 1804 Trie II



​```python
lass TrieNode:
    def __init__(self):
        self.children = [None] * 26
        # self.is_end = False
        self.word_count = 0
        self.child_count = 0

class Trie:

    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        # root = self.root
        cur = self.root
        idx = 0
        while idx < len(word):
            pos = ord(word[idx]) - ord('a')
            if not cur.children[pos]:
                cur.children[pos] = TrieNode()
            cur.child_count += 1    # ！！！！ REMEMBER!!!
            cur = cur.children[pos]
            idx += 1
        # cur = cur
        cur.word_count += 1
        cur.child_count += 1
        return None

    def countWordsEqualTo(self, word: str) -> int:
        cur = self.root
        idx = 0
        while idx < len(word):
            pos = ord(word[idx]) - ord('a')
            if not cur.children[pos]:
                return 0
            cur = cur.children[pos]
            idx += 1
        return cur.word_count

    def countWordsStartingWith(self, prefix: str) -> int:
        idx = 0
        cur = self.root
        while idx < len(prefix):
            pos = ord(prefix[idx]) - ord('a')
            if not cur.children[pos]:
                return 0
            cur = cur.children[pos]
            idx += 1
        return cur.child_count

    def erase(self, word: str) -> None:
        cur = self.root
        idx = 0
        while idx < len(word):
            pos = ord(word[idx]) - ord('a')
            if not cur.children[pos]:
                return None
            cur.child_count -= 1
            cur = cur.children[pos]
            idx += 1
        cur.word_count -= 1
        cur.child_count -= 1
        return None



## 559 Trie Service

Build tries from a list of <word, freq> pairs. Save top 10 for each node.

### Example

**Example1**

## 1268. Search Suggestions System 
```
Input:  
 <"abc", 2>
 <"ac", 4>
 <"ab", 9>
Output:<a[9,4,2]<b[9,2]<c[2]<>>c[4]<>>> 
Explanation:
			Root
             / 
           a(9,4,2)
          /    \
        b(9,2) c(4)
       /
     c(2)
```

**Example2**

```
Input:  
<"a", 10>
<"c", 41>
<"b", 50>
<"abc", 5>
Output: <a[10,5]<b[5]<c[5]<>>>b[50]<>c[41]<>>
```

​```python
Serialize and deserialize a trie (prefix tree, search on internet for more details).

You can specify your own serialization algorithm, the online judge only cares about whether you can successfully deserialize the output from your own serialize function.

You only need to implement these two functions serialize and deserialize. We will run the following code snippet

str = serialize(old_trie)
// str can be any string used to represent this tree
new_trie = deserialize(str)
// The new tree should be identical to the old one
Example
Example 1

Input: <a<b<e<>>c<>d<f<>>>>
Output: <a<b<e<>>c<>d<f<>>>>
Explanation:
The trie is look like this.
     root
      /
     a
   / | \
  b  c  d
 /       \
e         f
Example 2

Input: <a<>>
Output: <a<>>
Notice
You don't have to serialize like the test data, you can design your own format.
```



## 527 Trie Serialization

Serialize and deserialize a trie (prefix tree, search on internet for more details).

You can specify your own serialization algorithm, the online judge only cares about whether you can successfully deserialize the output from your own serialize function.

You only need to implement these two functions `serialize` and `deserialize`. We will run the following code snippet

```cpp
str = serialize(old_trie)
// str can be any string used to represent this tree
new_trie = deserialize(str)
// The new tree should be identical to the old one
```

### Example

**Example 1**

```plain
Input: <a<b<e<>>c<>d<f<>>>>
Output: <a<b<e<>>c<>d<f<>>>>
Explanation:
The trie is look like this.
     root
      /
     a
   / | \
  b  c  d
 /       \
e         f
```

**Example 2**

```plain
Input: <a<>>
Output: <a<>>
```

### Notice

You don't have to serialize like the test data, you can design your own format.

```python
"""
Definition of TrieNode:
class TrieNode:
    def __init__(self):
        # <key, value>: <Character, TrieNode>
        self.children = collections.OrderedDict()
"""
class Solution:

    '''
    @param root: An object of TrieNode, denote the root of the trie.
    This method will be invoked first, you should design your own algorithm 
    to serialize a trie which denote by a root node to a string which
    can be easily deserialized by your own "deserialize" method later.
    '''
    def serialize(self, root):
        # Write your code here
        if root is None:
            return ""

        data = ""
        for key, value in root.children.items():
            data += key + self.serialize(value)

        return "<%s>" % data


    '''
    @param data: A string serialized by your serialize method.
    This method will be invoked second, the argument data is what exactly
    you serialized at method "serialize", that means the data is not given by
    system, it's given by your own serialize method. So the format of data is
    designed by yourself, and deserialize it here as you serialize it in 
    "serialize" method.
    '''
    def deserialize(self, data):
        # Write your code here
        if data is None or len(data) == 0:
            return None

        root = TrieNode()
        current = root
        path =[]
        for c in data:
            if c == '<':
                path.append(current)
            elif c == '>':
                path.pop()
            else:
                current = TrieNode()
                if len(path) == 0:
                    print c, path
                path[-1].children[c] = current

        return root
```



## 745 Prefix & Suffix Search

```python

import collections
class TrieNode_TLE:
    def __init__(self):
        # self.children = collections.defaultdict(dict())
        # self.children = collections.defaultdict(TrieNode())   # BUGGG
        self.children = collections.defaultdict(TrieNode)   # fixed!
        self.is_end = False
        self.members = []
        
class WordFilter:

    def __init__(self, words: List[str]):
        self.root = TrieNode()
        self.root_suffix = TrieNode()
        for i in range(len(words)):
            # n = len(words[i])
            self.add(words[i], i)
        
    def add(self, word, member_id):
        root = self.root        # TODO: wrapped into API
        for i in range(len(word)):
            # root.members.append(member_id)
            root = root.children[word[i]]
            root.members.append(member_id)
        root.is_end = True
        
        root = self.root_suffix
        rev = word[::-1]
        for i in range(len(rev)):
            # root.members.append(member_id)
            root = root.children[rev[i]]
            root.members.append(member_id)
        root.is_end = True
        
    def starts_with(self, prefix, seq): # seq == -1 means we want suffix
        root = self.root
        if seq == -1:
            root = self.root_suffix
            
        for i in range(len(prefix)):
            if prefix[i] not in root.children:
                return None
            root = root.children[prefix[i]]
        # print(root.children)
        return root.members
    
    def f(self, prefix: str, suffix: str) -> int:
        members = self.starts_with(prefix, 1)
        members_suffix = self.starts_with(suffix[::-1], -1)
        # print(members, members_suffix)
        if not members or not members_suffix:
            return -1
        i, j = len(members) - 1, len(members_suffix) - 1
        while i >= 0 and j >= 0:
            # print(i, j)
            if members[i] == members_suffix[j]:
                return members[i]
            elif members[i] > members_suffix[j]:
                i -= 1
            else:
                j -= 1
        return -1


# Your WordFilter object will be instantiated and called as such:
# obj = WordFilter(words)
# param_1 = obj.f(prefix,suffix)
```



["WordFilter","f","f","f","f","f","f","f","f","f","f"]
[[["cabaabaaaa","ccbcababac","bacaabccba","bcbbcbacaa","abcaccbcaa","accabaccaa","cabcbbbcca","ababccabcb","caccbbcbab","bccbacbcba"]],["bccbacbcba","a"],["ab","abcaccbcaa"],["a","aa"],["cabaaba","abaaaa"],["cacc","accbbcbab"],["ccbcab","bac"],["bac","cba"],["ac","accabaccaa"],["bcbb","aa"],["ccbca","cbcababac"]]