---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-6-14
---



[toc]

# Shared-memory Programming -- Pthread

memory's address the same for all process units

![image-20230613204911655](https://p.ipic.vip/yqkg9g.png)



#### Thread VS Processes

![image-20230613205233935](https://p.ipic.vip/sax3xi.png)

 

![image-20230613205550640](https://p.ipic.vip/mljwld.png)

- `MPI` also works on only 1 single machine, but since it still at least requires 1 time memcopy (not deep into network layer b.c. lib's optimization), so still slower than `pthread`



# Pthread

- low-level APIs

![image-20230613205740988](https://p.ipic.vip/ylaa0z.png)

### Creation & Join 

- communicate via `global` or `return`
- Join --  get back
- attr -- e.g. bind thread to a specific core

![image-20230613205900839](https://p.ipic.vip/gc9s9z.png)



![image-20230613210526466](https://p.ipic.vip/wkxgd7.png)



- not yet using global variable for access and for sync the accesses
- back to main thread -- with join
- no common communication btw `main thread` and `worker`, diff tasks, which is different from MPI (with same one code)

- ![image-20230613210825653](https://p.ipic.vip/eispqw.png)

  - 二選一，必call

    

## Sync. problems & Tools

#### Lock

when many thread access a single memory

<img src="https://p.ipic.vip/anjrj1.png" alt="image-20230613211632884" style="zoom:67%;" />



![image-20230613211801639](https://p.ipic.vip/dsmfqm.png)

- multicore 或 可以access 到shared memory就會有這種問題



![image-20230613212019334](https://p.ipic.vip/tt707d.png)

- to ensure, for this block, only one thread can modify the critical section one time

![image-20230613212147907](https://p.ipic.vip/7907js.png)

- Mutex -- a token to solve  `critical section` for this problem



![image-20230613212420533](https://p.ipic.vip/czdvg1.png)

- must be a global variable for thread to share the lock to prevent sync issue





![image-20230613212614632](https://p.ipic.vip/2b9ug7.png)

![image-20230613215210184](https://p.ipic.vip/w5w9fc.png)

- sacrifice one item to distinguish empty vs full



#### Shared Memory

![image-20230613215410271](https://p.ipic.vip/mcuxo7.png)

- in/out no sync problem as only modified by one side at one time



<img src="https://p.ipic.vip/86qjiy.png" alt="image-20230613215543076"  />



#### Condiction Variables (CV) -- Event Driven

- signal

![image-20230613215931198](https://p.ipic.vip/zwoomx.png)



![image-20230613220948392](https://p.ipic.vip/eyk4cc.png)

<img src="https://p.ipic.vip/21kpqn.png" alt="image-20230613221108729" style="zoom:77%;" />

![image-20230613221826754](https://p.ipic.vip/e8rryg.png)

- needs lock token
- take_action() 放進去 critical section也ok能保證他執行時 x== 0



#### Thread Pools

![image-20230613222005541](https://p.ipic.vip/bcmev4.png)

![image-20230613222157683](https://p.ipic.vip/irg1wm.png)

![image-20230613222252838](https://p.ipic.vip/u9lrtp.png)

![image-20230613222359328](https://p.ipic.vip/kgmhg2.png)

- execute after de-que, and then back to sleep (pthread_cond_wait)

- implemented with 
  - Condition Variable
  - Thread
  - Lock (Mutex)



### Other tool -- Semaphore

![image-20230613223202556](https://p.ipic.vip/7l3qwk.png)



![image-20230613223300567](https://p.ipic.vip/7x2i9u.png)



- `sem_post()` means `signal()`



![image-20230613223522224](https://p.ipic.vip/7f5gzj.png)



### Other tool -- Monitor

- a class by default wrapped in "critical section"!
  - class already implement the lock & unlock 
- user don't need to manual write lock & unlock

![image-20230613223654203](https://p.ipic.vip/mi32el.png)



Example of Monitor in Java

![image-20230613223907205](https://p.ipic.vip/8hmi3h.png)



![image-20230613224017776](https://p.ipic.vip/ov1rdk.png)



![image-20230613224316842](https://p.ipic.vip/4019ie.png)



![image-20230613224416696](https://p.ipic.vip/t97cd5.png)





# OpenMP, w/ compiler's help

- using compiler's way to generate code

![image-20230614175006357](/Users/joe/Library/Application Support/typora-user-images/image-20230614175006357.png)



- "parallel" is helped by compiler to **gen code**![image-20230614175103178](https://p.ipic.vip/o9t8qs.png)

- `pragma` -- means `compiler directive`
  - `#include <omp.h>`
  - `parallel for`

![image-20230614180005356](https://p.ipic.vip/bo5aay.png)



## Parallel Region Construct



## Working-Sharing Construct



## Sync. Construct



## Data Scope Attribute Clauses 



## Run-Time Lib Routines



