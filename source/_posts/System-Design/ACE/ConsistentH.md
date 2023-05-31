---
layout: post
categories: SystemDesign
tag: []
date: 2020-01-15
---



# Consistent Hashing

## map keys into key spaces ---%--> servers

- hash function best in uniform hash , randomly scattered
- key 要分布去一堆服务器做作理

但是，「即使」一开始可以很好地randomly scattered
- NAIVE 取模 方式的问题 ，当有down(rehash)了时，
  - 大风吹，而且又有非必要的挪动，不符合我们想要的移动最少的想法
  - unnecessary key redistribution



## Consistent Hashing

- N necessary keys
- 算出来的hash顺时针往後找吧！
- 往回找我的工作，往前依附給下台機器





## Uniformly Distributed Hashing Algorithm within whole hash range

e.g. SHA, MD5

這裡的話是hash ring上，也是evenly distribute打散很棒的

Hash算完後再truncate掉也是Uniformly Distributed



為了讓每個server的key量是相當的



## New Problems

但這樣出現的問題會是「不平均間隔」

***Problem on paper --*** 

1 Random position of nodes leads to non-uniform data and load distribution

2 Each node can have different performance characteristic (有可能機器就是不一樣的，我們希望讓強的就是算多一些，而不是受Uniform分佈影響)





## 加上 Virtual Node to Solve 

***Solution on paper --*** 

2^160 nodes on ring

1 Random position of nodes leads to non-uniform data and load distribution 只需要redistribution很小部份data，且每台的量都也是稱職平均的

​	a More virtual nodes, key distributino is more balanced

​	b When add/remove node, evenly distributed data & load among node



2 Each node can have different performance characteristic

​	a Assign different size key range for each node



最後，V-node mapping to servers!



![image-20220916100307808](https://tva1.sinaimg.cn/large/e6c9d24egy1h687v1wssej20uw0rs76p.jpg)





## Appendix

- 管理mapping 表的在一台機器？在所有機器？過半數人？
- 承上 Gossip 協議、Zookeeper
- ![image-20220916101431164](https://tva1.sinaimg.cn/large/e6c9d24egy1h6886ur03bj20q00eon09.jpg)

Gossip protocol

Zookeeper

<-->

