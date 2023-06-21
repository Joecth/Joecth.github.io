---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-5-17
---



[toc]

# Shared-memory Programming -- OpenMP

memory's address the same for all process units

![image-20230613204911655](https://p.ipic.vip/yqkg9g.png)



#### Thread VS Processes

![image-20230613205233935](https://p.ipic.vip/sax3xi.png)

 

![image-20230613205550640](https://p.ipic.vip/mljwld.png)

- `MPI` also works on only 1 single machine, but since it still at least requires 1 time memcopy (not deep into network layer b.c. lib's optimization), so still slower than `pthread`



# OpenMP, w/ compiler's help

- using compiler's way to generate code

![image-20230614175006357](https://p.ipic.vip/aszlg3.png)



- "parallel" is helped by compiler to **gen code**![image-20230614175103178](https://p.ipic.vip/o9t8qs.png)

- `pragma` -- means `compiler directive`
  - `#include <omp.h>`
  - `parallel for`

![image-20230614180005356](https://p.ipic.vip/bo5aay.png)



## Parallel Region Construct

- create threads
- Fork-join model, so, there's **a barrier at the end of a parallel section**
- a thread crashes --> all thread crash!

![image-20230614180902762](https://p.ipic.vip/pgzvly.png)



![image-20230614181915982l](https://p.ipic.vip/nio6ba.png)

- omp_set_num_thread() -- so that don't have to write num_threads in every pragma block
- IF is rarely used in specific case



![image-20230614182028173](https://p.ipic.vip/ttwj3i.png)



## Working-Sharing Construct

![image-20230614182135658](https://p.ipic.vip/86k5x2.png)

![image-20230614182446713](https://p.ipic.vip/3l3nb2.png)



### DO/for Directive

- dup taskk, for `data parallelism`
- 最主要: 了解到shceuling 的選擇
- 然後有order 、Collpase 這些選擇讓load更balance

![image-20230614182741113](https://p.ipic.vip/in110y.png)

#### Schedule

- static 

  - Chunk large --  hard to balance among threads

    Chunk small --  need to ask for thread many times

![image-20230614183623083](https://p.ipic.vip/5ehpdn.png)

![image-20230614183636163](https://p.ipic.vip/0airrf.png)



![image-20230614183723831](https://p.ipic.vip/4mye55.png)

![image-20230614183800116](https://p.ipic.vip/34i69g.png)



![image-20230614183948584](https://p.ipic.vip/wakood.png)

- 兩行 pragma 也可以併到一行去；parallel 、for 有各自的clauses



![image-20230614184718386](https://p.ipic.vip/6crjey.png)



#### Collapse! 

![image-20230614185548583](https://p.ipic.vip/pqpuyb.png)

- 得要 i == 1 not depent on i == 0



### SECTIONS Directive

- no dup
- Diff calls for diff threads
- no schedule 
- usually 3~4 sections 

![image-20230614200900819](https://p.ipic.vip/lls7y2.png)

![image-20230614201017533](https://p.ipic.vip/2l3cba.png)



### SINGLE

- e.g. I/O, need only one thread to write
- others wait, unless `nowait` is added after `single`
- so, don't need to jump out a Parallel region, 
- Also code looks more neat!

![image-20230614201131558](https://p.ipic.vip/vm06i1.png)



## Synchronization Construct

- Similar to SINGLE

![image-20230614201535093](https://p.ipic.vip/lb1i68.png)

- atomic ~= critical
  - 用Java's SyncMethod & SyncStatement 作類比
    - atomic，fine grain 像是MPI的Java的 SyncStatement，告訴它要保護的share variable，有碰到這些var的ref的會去幫做lock、unlock
    - critical, 整段包起來，整個block的statement就是從頭到尾就算沒有sync的問題，也是包起來



![image-20230614202014595](https://p.ipic.vip/iev75d.png)



### ★Example & Comparison!!

![image-20230614202049037](https://p.ipic.vip/ndzm92.png)



## Data Scope Attribute Clauses 

![image-20230614202421417](https://p.ipic.vip/grxj03.png)

- SHARED --  global, gened var in global, and apply the address of those vars in sections



![image-20230614202810013](https://p.ipic.vip/48davx.png)



![image-20230614202923366](https://p.ipic.vip/ptfira.png)



![image-20230614203403943](https://p.ipic.vip/7c56g9.png)

- DEFAULT -- no need to manually write scop attr for so many vars



![image-20230614203451073](https://p.ipic.vip/zrnpgp.png)

- result within block is the "partial sum" of each thread
- ★★ result is a `local PRIVATE var`, b.c. there's `reduction` keyword



## Clause Summary

![image-20230614203910407](https://p.ipic.vip/yyswrn.png)



## Run-Time Lib Routines

- not critical
- 知道是哪個thread num 其實意義不大，因為很多時候是internal scheduling 幫弄了調度工作

![image-20230614204138760](https://p.ipic.vip/98njjs.png)

- omp_in_parallel -- for IF clause 用

