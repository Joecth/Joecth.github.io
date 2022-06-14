---
layout: post
categories: BayesianML
tag: [] 
date: 2018-11-02

---



## Two Sample t-test

```mermaid
graph LR
	A[Calculate t] --> B[Calculate P]
	C[Calculate ] --> B[Calculate P]
```



### Optimization

1. moving mean; instead of re-calculate the mean every time a new sample comes in
2. Moving std; instead of re-calculate the std every time a new sample comes in

