---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-6-13
---



[toc]

# Parallel Prog Analysis



### Speedup & Efficiency

- communnication1!! where overhead and bottleneck from
  - ![image-20230613013303011](https://p.ipic.vip/yqgq97.png)

#### Max Speedup

![image-20230613105540459](https://p.ipic.vip/0qacr0.png)

![image-20230613105641557](https://p.ipic.vip/9dmjtm.png)

- f: a factor

![image-20230613105801451](https://p.ipic.vip/912sjn.png)





### Strong Scalability VS Weak Scalability

#### Strong Scaling

- Ideal

- to know "how many machines I should use to solve this problem?"

![image-20230613110239466](https://p.ipic.vip/vken5x.png)

- 花費在communication的 time ↑，as nodes ↑, so hard to get a linear scaleup

- 畫不出，因為「resource is too few」

- as data partition↓  external communication↑



#### Weak Scaling

e.g., 2 nodes, but the arrays size to solve becomes 2 times

相對*容易得到*這樣的結果 -- 因為computation也多了，不只有communication的overhead多

![image-20230613110425965](https://p.ipic.vip/74wl5d.png)



#### vs

![image-20230613113259366](https://p.ipic.vip/ik96q7.png)



### Time Complexity & Cost optimality

should also take into `communication cost`

![image-20230613114309137](https://p.ipic.vip/n20xf5.png)



#### e.g. 1

![image-20230613114633053](https://p.ipic.vip/pfld9e.png)



#### e.g. 2

![image-20230613114704506](https://p.ipic.vip/252uvg.png)



![image-20230613114855791](https://p.ipic.vip/qkmdkd.png)



### Cost-Optimal Alg.

![image-20230613115039542](https://p.ipic.vip/zblaps.png)