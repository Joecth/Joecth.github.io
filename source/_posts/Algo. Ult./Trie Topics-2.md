---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2019-09-25
---



## 333 · Identifying Strings

Given n character strings containing only lower case letters, find the minimum prefix strings that can identify each string.
That is, the minimum prefix string Ap which identifies string A will not be a prefix string of other n-1 character strings.

#### Sample

```shell
Input:["aaa","bbc","bcd"]
Output:["a","bb","bc"]
Explanation:"a" is only the profix of "aaa".
"bb" is only the profix of "bbc".
"bc" is only the profix of "bcd".
```

#### My Sol

```python 
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
        self.word_cnt = 0
        self.word_list = []

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def add(self, word):
        cur = self.root
        for i in range(len(word)):
            if word[i] not in cur.children:
                cur.children[word[i]] = TrieNode()
            cur = cur.children[word[i]]
            cur.word_cnt += 1
        cur.is_end = True
    
    def get(self, word):
        cur = self.root
        n = len(word)
        prefix = ''
        for i in range(n):
            prefix += word[i]
            if cur.children[word[i]].word_cnt == 1:
                return prefix
            cur = cur.children[word[i]]
        return word


from typing import (
    List,
)

class Solution:
    """
    @param string_array: a string array
    @return: return every strings'short peifix
    """
    def short_perfix(self, string_array: List[str]) -> List[str]:
        # write your code here
        # return self.my_20220222(string_array) # w/ Hash
        return self.my_20220223(string_array)   # w/ Trie

    def my_20220223(self, string_array):
        n = len(string_array)
        trie = Trie()
        for i in range(n):
            trie.add(string_array[i])
        
        res = []
        for i in range(n):
            tmp = trie.get(string_array[i])
            res.append(tmp)
        return res


    def my_20220222(self, string_array):
        n = len(string_array)
        prefix2cnt = {}
        # prefix2word = {}
        for i in range(n):
            word = string_array[i]
            for j in range(len(word)):
                token = word[:j+1]
                prefix2cnt[token] = prefix2cnt.get(token, 0) + 1
                # prefix2word

        # processed_word = {}
        res = []
        for i in range(n):
            word = string_array[i]
            # for j in range(len(word)):
            j = 0
            while j < len(word):
                token = word[:j+1]
                if prefix2cnt[token] > 1:
                    j += 1
                    continue
                else:
                    res.append(token)
                    break
            if j == len(word):      # !!! CAUTIOUS!
                res.append(word)
                
        return res

```
