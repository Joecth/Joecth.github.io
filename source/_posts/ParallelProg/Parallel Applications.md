---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-6-19
---



[toc]

# Alg: Embarrassingly Computations

![image-20230614205211835](https://p.ipic.vip/csaeeo.png)



### Image Transformations

- Low-level image operations

![image-20230614205406250](https://p.ipic.vip/h6pbm0.png)



![image-20230614205510336](https://p.ipic.vip/v93s98.png)

- 縱切黑白比較平衡
- 鄰居數會affect communication

![image-20230614210714720](https://p.ipic.vip/knofs2.png)

### Monde Carlo Methods

- Sampling data points,  time samplings are independent

![image-20230614211437118](https://p.ipic.vip/2hhy1y.png)

![image-20230614211737368](https://p.ipic.vip/06us4l.png)

![image-20230614211813740](https://p.ipic.vip/nvz855.png)



### Mandelbrot Set







# General Issues: LB & Termination

## LB (Load Balance)

- centralize or 
- distributed 很難 termination

![image-20230614212600210](https://p.ipic.vip/eop4he.png)



### Why Static bad?

![image-20230614220951383](https://p.ipic.vip/146cy6.png)

![image-20230614221120487](https://p.ipic.vip/oby280.png)



### Dynamic LB, w/ MPI e.g.

- P.S. OpenMP has

![image-20230614221213219](https://p.ipic.vip/5u3k31.png)



#### Centralized Work Pool

![image-20230614221533768](https://p.ipic.vip/58hthe.png)



![image-20230614221712638](https://p.ipic.vip/1uki3p.png)



![image-20230614222147601](https://p.ipic.vip/w5l5ke.png)



#### Decentralized Work Pool

- many masters, with *hierarchical structure*![image-20230615102619627](https://p.ipic.vip/7wmmfa.png)



#### Fully Distributed Work Pool

- though balanced, yet more communications

![image-20230615102827852](https://p.ipic.vip/l6pdem.png)

##### How to balance in Fully Distrubuted Work Pool?

<img src="https://p.ipic.vip/a2p6b2.png" alt="image-20230615102949785" style="zoom: 87%;" />

- Reason for Light loadded system -- for Sender-initiated method
  - Because even the sender has higher load, its load is still pretty "light"
  - vice versa 
- If no global view, how does a thread know it's a heavy or light?
- A term: work stealing 很常被討論

##### E.g. Shortes Path in a Weighted Diagraph

![image-20230615103708587](https://p.ipic.vip/j0m6p0.png)



★平行時候採用的不是Dijkastra, 而是Moore's 

![image-20230615103807875](https://p.ipic.vip/1dj5ac.png)

- Dependency only btw Iterations, NOT btw nodes
- Independent nodes

###### Sequential Proc

1. ![image-20230615110016415](https://p.ipic.vip/pna3pc.png)
2. ![image-20230615110108163](https://p.ipic.vip/g0a112.png)

3. ![image-20230615110139431](https://p.ipic.vip/r0i28o.png)



###### Parallel Proc

![image-20230615110320944](https://p.ipic.vip/hfzbj7.png)

- Step4 has buffer size issue (depent on outgoing nbrs, but MPI requires buffer size well communicated beforehand) to communicate in advance 

- Master assign tasks



![image-20230615111103268](https://p.ipic.vip/7kufdx.png)



![image-20230615111157912](https://p.ipic.vip/e47qnr.png)

- 每個人只需要知道自己目前的node



## Termination Detection

- 重點是分散式時的terminate需要留意!

![image-20230615111859029](https://p.ipic.vip/84ulct.png)

![image-20230615111927488](https://p.ipic.vip/2dfzgp.png)



#### Dual-Pass Ring

![image-20230615112125900](https://p.ipic.vip/il91pd.png)

- 避免往前送了後，但recv已被activate「white」的問題



1. ![image-20230615112458015](https://p.ipic.vip/nz4ni5.png)
2. ![image-20230615112512660](https://p.ipic.vip/a740jc.png)
3. ![image-20230615112543018](https://p.ipic.vip/rid8ji.png)



Example

​	![image-20230615112630803](https://p.ipic.vip/3uag02.png)



# Alg: D&C Computations

- recursively divided a problem into sub-problems that are of the sam eform as the larger problem

### Sorting Algs.

in Parallel.

![image-20230615113019339](https://p.ipic.vip/mz978l.png)



##### BucketSort

![image-20230615114142364](https://p.ipic.vip/nb7vtt.png)

![image-20230615115012915](https://p.ipic.vip/oxcpkm.png)

- Can sampling beforehand to know the distribution!
- partition not yet parallelized



![image-20230615114532487](https://p.ipic.vip/bqhcdn.png)

- solved the partition not parallelized issue!
- 分配的平行仰賴於不看值



##### MergeSort it's Optimization

![image-20230615114920490](https://p.ipic.vip/wjs54z.png)

- bottleneck bounded at the last merging process, with all elems



###### Bitonic MergeSort

![image-20230615153711909](https://p.ipic.vip/z4rrg1.png)

![image-20230615153729891](https://p.ipic.vip/nhm1x8.png)

![image-20230615153915925](https://p.ipic.vip/moyclk.png)

![image-20230615154130420](https://p.ipic.vip/hl8pgt.png)



##### QSort

tree **not balanced**... so... **not suitable for parallelism**

![image-20230615115212226](https://p.ipic.vip/yuapmb.png)



##### RankSort

- Very easy for Parallelism 
- 只需要加一行OpemMP 就平行了！
- data dependency is None!

![image-20230615154546768](https://p.ipic.vip/68xg2u.png)

###### LgN TimeComplexity!

![image-20230615154802371](https://p.ipic.vip/04rs7l.png)



##### CountingSort

- 是跟range有關的… 所以時間複雜度得再想下，下面這是個ideal的case而已
- 看每個值重覆出現多少次

![image-20230615155048145](https://p.ipic.vip/vac5vz.png)



##### Summary

![image-20230615155343588](https://p.ipic.vip/btmasv.png)



### N-Body Simulation

- Newtonian laws of Physics



# Alg: Pipelined Computations

![image-20230615155845442](https://p.ipic.vip/ofq9kv.png)



### Types

Pipelined approach can provide increased speed under three types oif computations

1. If **more than 1 instance ** of the complete problem is to be executed; or **CPU's instructions**

2. If a **ingle instance** has a series of data items must be processed,. each requiring **multiple oerations**
3. 像是第2個的加強版；If **information to start the next process can be passed forward** before the process has completed all its internal operations



![image-20230615165326563](https://p.ipic.vip/xyejcx.png)

​		![image-20230615165647254](https://p.ipic.vip/7useax.png)	

​	![image-20230615170142574](https://p.ipic.vip/wshroy.png)



### Adding Numbers (Type1)

![image-20230615170313469](https://p.ipic.vip/4pc7cc.png)

![image-20230615170340129](https://p.ipic.vip/yclv0e.png)



### Sorting (Type2)

##### Insertion Sort



### Linear Equation Solver (Type3)



# Alg: Synchronous Computations

![image-20230615170819710](https://p.ipic.vip/gm5t9n.png)

- e.g. Odd-Even Sort

![image-20230615170951174](https://p.ipic.vip/s3qm88.png)

![image-20230615171049453](https://p.ipic.vip/2568ls.png)

![image-20230615171127497](https://p.ipic.vip/vite9s.png)

![image-20230615171255882](https://p.ipic.vip/2y5023.png)

- P0、P1 互等… 死了



### PreSum

![image-20230615171355250](https://p.ipic.vip/0k5rsl.png)

![image-20230615171508521](https://p.ipic.vip/thr9a6.png)

![image-20230615171532354](https://p.ipic.vip/z6u5s6.png)

![image-20230615171553368](https://p.ipic.vip/efydyb.png)



### System of Linear Equation

...