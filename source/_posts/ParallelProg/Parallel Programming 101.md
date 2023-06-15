---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-6-11
---



[toc]

Parallel computing is an **evolution of serial computing**



# Intro Idea

Diff btw parallel computing & distributed computing

- parallel computing 
  - centeredvclustered
- Distributed computing
  - across distanced servers
  - resource sharing, e.,g. Hadoop

![image-20230611113005715](https://p.ipic.vip/p5vvj5.png)





![image-20230611113657441](https://p.ipic.vip/vq3v47.png)

![image-20230611114953768](https://p.ipic.vip/40apfw.png)





# Classification of Parallel Computers & Programming models



1. Flynn's classic taxonomy

2. Memory architecture classification
3. Programming model classification



### 1 Flynn's classic taxonomy

- since 1966
- based on : `instruction` & `data`
- <img src="https://p.ipic.vip/w99krn.png" alt="image-20230611115601765" style="zoom:67%;" />



##### SISD

![image-20230611115712729](https://p.ipic.vip/cnytiv.png)



#### SIMD

![image-20230611115814089](https://p.ipic.vip/qepvxa.png)



#### MISD

![image-20230611120058551](https://p.ipic.vip/xf4w10.png)



#### MIMD

![image-20230611120246700](https://p.ipic.vip/iid8f7.png)





### 2 Memory Architecture

Shared meory vs Distributed Memory Computer Architecture



#### Shared Memory

![image-20230611120752829](https://p.ipic.vip/9wn5co.png)

![image-20230611120930051](https://p.ipic.vip/3haoy3.png)



#### Distributed Memory

- cluster  -- 同一個管理者  vs Supercomputer -- 每層sw set
- 

<img src="https://p.ipic.vip/o18ntx.png" alt="image-20230611121643961" style="zoom:67%;" />

![image-20230611122056110](https://p.ipic.vip/49aaoq.png)





### 3 SW, Parallel Programming Model

![image-20230611122655829](https://p.ipic.vip/pxceyf.png)

P.S. "**Not restricted**" means, e.g. OpenMP is "message passing prog", but able for a Shared memory prog machine!



#### Shared Memory Prog

![image-20230611122904463](https://p.ipic.vip/csw96t.png)



#### <img src="https://p.ipic.vip/hc2hv4.png" alt="image-20230611122928940"  />



#### Message Passing Prog

![image-20230611123035755](https://p.ipic.vip/ocw304.png)

####  ![image-20230611123118348](https://p.ipic.vip/1cwoup.png)



- programming model -- SW
- parallel system -- HW architecture

![image-20230611123343220](https://p.ipic.vip/y6obw9.png)



# Supercomputer & Latest Techs

### Supercomputer

<img src="https://p.ipic.vip/ji2gwf.png" alt="image-20230612185350196" style="zoom:50%;" />



- `U` means the height of each cell
- measured in `FLOPS` instead of `MIPS`
- Benchmark -- ranked by the `TOP500` list since 1993
  - according to the `HPL benchmark` result
  - twice/year at ISC & SC conferences
    - LU factorization by `Panel factorization`







#### TOP500 List

<img src="https://p.ipic.vip/oigpsr.png" alt="image-20230612185910128" style="zoom:50%;" />



in https://www.top500.org/lists/2018/06, ↓

![image-20230612190238421](https://p.ipic.vip/2cyzdh.png)



<img src="https://p.ipic.vip/cssnvo.png" alt="image-20230612190356802" style="zoom:50%;" />



<img src="https://p.ipic.vip/dp4iey.png" alt="image-20230612190425459" style="zoom:50%;" />



<img src="https://p.ipic.vip/65yj1n.png" alt="image-20230612190510473" style="zoom:50%;" />

<img src="https://p.ipic.vip/lrwd6z.png" alt="image-20230612190548760" style="zoom:50%;" />

<img src="https://p.ipic.vip/tfly7d.png" alt="image-20230612190821656" style="zoom:50%;" />

<img src="https://p.ipic.vip/5wk12f.png" alt="image-20230612191014562" style="zoom: 50%;" />



### 1 Processor Technology



![image-20230612191226950](https://p.ipic.vip/wt3zm3.png)

- PBSM --  per block shared-memory
- `Globabl Shared memory` for `in-btween blocks`
- Thread Execution Manager -- is the hardware Scheduler



![image-20230612191656520](https://p.ipic.vip/pyv6pu.png)





<img src="https://p.ipic.vip/uejx05.png" alt="image-20230612191901027" style="zoom:50%;" />





### 

### 2 Interconnect & Network Technology

The usual bottleneck

sharedmemory or network

<img src="https://p.ipic.vip/09jgwc.png" alt="image-20230612192112407" style="zoom:50%;" />

- Wait, delay, etc...



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230613004213850.png" alt="image-20230613004213850" style="zoom: 67%;" />

![image-20230613004401691](https://p.ipic.vip/2ov735.png)

![image-20230613004524961](https://p.ipic.vip/sy4r7q.png)

![image-20230613004704255](https://p.ipic.vip/yu028j.png)

![image-20230613004820761](https://p.ipic.vip/zfcu3u.png)

![image-20230613004941763](https://p.ipic.vip/ryz6ji.png)



![image-20230613005130735](https://p.ipic.vip/c9nk20.png)



![image-20230613005205579](https://p.ipic.vip/wwtlfx.png)



![image-20230613005332196](https://p.ipic.vip/7sbkfs.png)



![image-20230613005422539](https://p.ipic.vip/8ciys1.png)



### 3 I/O storage Technology

![image-20230613010621983](https://p.ipic.vip/cvi0ox.png)

![image-20230613010813781](https://p.ipic.vip/bs7rlu.png)



![image-20230613011144095](https://p.ipic.vip/v7qt2b.png)

- NVRAM : non-volatile ram, as SSD, hard-drive, 



## Summary

People has been and will always be able to find a way to keep the growth of computing 

	- Technology: CPU scaling, distributed computing, new processor architecture 
	- Optimization: algorithm, data management, compiler 
	- System design: network topology, file system 

It is more than just computing 

- Networks and 10 become greater concerns 

Does the performance report from supercomputers really meets the needs of applications? 

- People start re-thinking what should be the right objective and benchmark for designing the next generation of supercomputers



ref: [國立清華大學開放式課程OpenCourseWare(NTHU, OCW) - 平行程式](https://ocw.nthu.edu.tw/ocw/index.php?page=course&cid=231)

