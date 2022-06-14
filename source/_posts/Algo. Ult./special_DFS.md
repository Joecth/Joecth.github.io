---
layout: post
categories: AlgoUlt.
tag: [] 
date: 2020-06-03
---



# Special DFS



p.s. DFS with specific boundary check during recursion

e.g. like subset, comb, or perm and so on







## 320 Generalized Abbreviation

A word's **generalized abbreviation** can be constructed by taking any number of **non-overlapping** and **non-adjacent** substrings and replacing them with their respective lengths.

- For example,

   

  ```
  "abcde"
  ```

   

  can be abbreviated into:

  - `"a3e"` (`"bcd"` turned into `"3"`)
  - `"1bcd1"` (`"a"` and `"e"` both turned into `"1"`)
  - `"5"` (`"abcde"` turned into `"5"`)
  - `"abcde"` (no substrings replaced)

- However, these abbreviations are

   

  invalid

  :

  - `"23"` (`"ab"` turned into `"2"` and `"cde"` turned into `"3"`) is invalid as the substrings chosen are adjacent.
  - `"22de"` (`"ab"` turned into `"2"` and `"bc"` turned into `"2"`) is invalid as the substring chosen overlap.

Given a string `word`, return *a list of all the possible **generalized abbreviations** of* `word`. Return the answer in **any order**.

 

**Example 1:**

```
Input: word = "word"
Output: ["4","3d","2r1","2rd","1o2","1o1d","1or1","1ord","w3","w2d","w1r1","w1rd","wo2","wo1d","wor1","word"]
```

**Example 2:**

```
Input: word = "a"
Output: ["1","a"]
```

 

**Constraints:**

- `1 <= word.length <= 15`
- `word` consists of only lowercase English letters.

```python
from collections import deque
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        res = []
        self.my_20200604(word, 0, 0, [], res)
        return res
        # return self.my_1(word)
        # return self.sol(word)
        # return self.sol2(word)
        
    # [1, 0]  --> 1 o
    #         --> 1 + 1 = 2
    # [w, 0]  --> w o
    #         --> w 1
    
    
    def my_20200604(self, word, idx, cnt, item, res):
        if idx == len(word):
            if cnt > 0:
                item.append(str(cnt))
            res.append(''.join(item))
            if cnt > 0:
                item.pop()
            return

        self.my_20220604(word, idx + 1, cnt + 1, item, res)
        
        # item.append(word[idx])
        # self.my_20220604(word, idx + 1, 0, item + word[idx], res)
        # str_cnt = str(cnt)
        if cnt > 0:
            self.my_20220604(word, idx + 1, 0, item + [str(cnt)] + [word[idx]], res)
        else:
            self.my_20220604(word, idx + 1, 0, item + [word[idx]], res)
            
            
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        subsets = [""]
        
        for i in range(len(word)):
            # at every level, we take the previous strings then either 
            # (1). append the character
            # (2). abbreviate the character
            lvl_subsets = []
            for subset in subsets:
                # (1). append character
                lvl_subsets.append(subset + word[i])
                
                # (2). abbreviation
                # (2)(a)
                # if last char is an integer, += 1
                if subset and subset[-1].isdigit():
                    lvl_subsets.append(subset[:len(subset) - 1] + str(int(subset[-1]) + 1))
                # (2)(b) 
                # otherwise, add the number 1 if the previous character is NOT a number
                else:
                    lvl_subsets.append(subset + "1")
            subsets = lvl_subsets
        
        return subsets
      
      
class Solution:
    def generateAbbreviations(self, word: str) -> List[str]:
        queue = deque([([], 0, 0)]) # tuple of currentWord, index, count
        
        res = []
        
        while queue:
            currWord, index, count = queue.popleft()
            
            if index == len(word): # index has reached the end of word - we are finished processing this tuple
                if count != 0:
                    currWord.append(str(count))
                res.append("".join(currWord))
                
            else:
                # branch 1
                # don't add current letter from input and increase count by 1 (so we can turn it into a number later)
                queue.append((currWord, index + 1, count + 1))
            
                # branch 2
                newWord = list(currWord)
                if (count != 0): # if there is a count, consume it and add it to our newWord
                    newWord.append(str(count))
                
                newWord.append(word[index]) # add current letter
                queue.append((newWord, index + 1, 0))
        
        return res
      

```





## 22 Generate Parentheses

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

 

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```

 

**Constraints:**

- `1 <= n <= 8`





```python
# 2 ==> 4 symbols
# (()), ()()
# ((((, ((()

class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        # self.res = []
        # self.helper(n, n, n, "")
        # return self.res
        res = []
        self.dfs_20220604(n, 0, 0, [], res)
        return res

    def dfs_20220604(self, n, l, r, item, res):
        if l == n and r == n:
            res.append(''.join(item[:]))
            return
        
        if l < r:
            return
        
        if l < n:
            item.append('(')
            self.dfs_20220604(n, l + 1, r, item, res)
            item.pop()

        if r < n:
            item.append(')')
            self.dfs_20220604(n, l, r + 1, item, res)
            item.pop()              
        
```
