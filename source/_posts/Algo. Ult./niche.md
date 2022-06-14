---
layout: post
categories: AlgoUlt.
tag: []
date: 2022-01-31
Author: Jo
---



## Gospers-Hack

- [Gospers-Hack](https://github.com/wisdompeak/LeetCode/blob/master/Template/Bit_manipulation/Gospers-Hack.cpp)

```cpp
// Iterate all the m-bit state where there are k 1-bits.

int state = (1 << k) - 1;            
while (state < (1 << m))
{
    doSomething(state);

    int c = state & - state;
    int r = state + c;
    state = (((r ^ state) >> 2) / c) | r;
}
```



e.g. LC 2151
https://blog.csdn.net/qq_30108019/article/details/122694407

https://www.youtube.com/watch?v=bAzfkApHEnI

https://www.prosequence.tech/LeetCode/2151.%20Maximum%20Good%20People%20Based%20on%20Statements



