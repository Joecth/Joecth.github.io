---
layout: post
categories: LC
date: 2019-10-05
tag: [Michell, F, TODO] 



---



```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        if not str:
            return True
        
        i, j = 0, len(s) - 1
        
        while i < j:
            while i < j and not s[i].isalnum():
                i += 1
            while i < j and not s[j].isalnum():
                j -= 1
            
            if s[i].lower() != s[j].lower():
                return False
            i += 1
            j -= 1
            
        return True
            
```

P.S.
".," -- > True