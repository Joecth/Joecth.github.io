---
layout: post
categories: Kafka
tag: []
date: 2020-10-23
---



# Kafka Intro



- Distributed
- pub/sub
  - Diff topics according to diff messages' types
- MQ



Event Steraming



![image-20230503165108117](https://p.ipic.vip/mfg6e4.png)



Cache、消峰、解耦、異步通訊

![image-20230503170257560](https://p.ipic.vip/20uv24.png)



![image-20230503170437019](https://p.ipic.vip/s5tjxb.png)





# 基礎架構

發送時是發到某個topic

- `topic `
- `分區(partition)` for 分佈式，作法是一個topic被分為多個partitions，就可以分到多台電腦的資源了;  一台的時候受限的是帶寬--12.5MBytes/sec
- 消費的人也拆開了分為多個，共同在一個group內作併發 (默認自己一個人是一個組，然後會擔保不會重覆)
- 有用`副本(replica)`作容錯

![image-20230503171543880](https://p.ipic.vip/ju1a7n.png)

![image-20230503171635183](https://p.ipic.vip/5bjpav.png)

![image-20230503171840722](https://p.ipic.vip/pndo8q.png)

![image-20230503172039395](https://p.ipic.vip/ztuxib.png)

★只有涉及到副本的時候才會有「主從」問題；要是說分區０跟分區１兩個是不互影響的獨立的不會說「主從」



# 搭集群

msk 幫做完的事：在每台機器上會有quorumpeermain ，然後一般是可以用jps看jvm跑的服務



kafkaServer.out、server.log可看到錯誤

![image-20230503210412545](https://p.ipic.vip/kn2xtr.png)



改分區跟刪topic細節

![image-20230503211707348](https://p.ipic.vip/tiuhdf.png)

![image-20230503211828677](https://p.ipic.vip/iecvkr.png)

　

ref: [2.消息队列的功能介绍_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Wv4y1m7m6?p=3&vd_source=a446d08c42a016c121a1c8007fc3ce42)
