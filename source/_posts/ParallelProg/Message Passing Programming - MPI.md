---
layout: post
categories: ParallelProg
tag: [] 
date: 2022-6-11

---



[toc]



# MPI (Message-Passing Interface)

- a spec for devs and users of msg passing libraries

  - by itself, it's an interface NOT a library

- opposite term: shared-memory

  



## History & Evolution

# MPI API

<img src="https://p.ipic.vip/fn72j1.png" alt="image-20230613122604220" style="zoom:67%;" />

- easy for scalable compared to shared-memory



<img src="https://p.ipic.vip/01znja.png" alt="image-20230613161341091" style="zoom:80%;" />



Supercomputing conference!

![image-20230613161707907](https://p.ipic.vip/hdajuz.png)

#### SPMD

<img src="https://p.ipic.vip/gzc3x2.png" alt="image-20230613172432877" style="zoom:50%;" />





## Communication Methods

### Env

<img src="https://p.ipic.vip/99otsf.png" alt="image-20230613172646088" style="zoom:67%;" />

<img src="https://p.ipic.vip/4p5cmk.png" alt="image-20230613172858188" style="zoom:67%;" />



![image-20230613173033146](https://p.ipic.vip/8cl546.png)



![image-20230613173058617](https://p.ipic.vip/24hcfj.png)

- if recv() calls first, it's value will be false since no data sent yet, so, need to check!



<img src="https://p.ipic.vip/hsvy1a.png" alt="image-20230613173427769" style="zoom:80%;" />

- SPMD, each program still executes itself's code, but only communication in MPI call()



![image-20230613174115721](https://p.ipic.vip/pa6drb.png)

- Group Communicator -- symbolizes the group
- predefined communicator -- MPI_COMM_WORLD





<img src="https://p.ipic.vip/9xd8vu.png" alt="image-20230613174240148" style="zoom:80%;" />

- each process get itsown rank_id



![image-20230613174421144](https://p.ipic.vip/jkgoap.png)



### P2P Communication Routines

mainly send & recv

![image-20230613185929392](https://p.ipic.vip/s6xm9t.png)

![image-20230613190000830](https://p.ipic.vip/j2tlr4.png)

★ send & recv一定要接在一起，要是沒對在一起就是會hang! 都要有人去call，要是有人沒call就難debug了；只有在switch時會有branches

★這兩個x會是在不同machines 上的，要注意, so, independent memories



![image-20230613190500434](https://p.ipic.vip/vvast6.png)

- 效能通常比較好，就是解決communication problems
- only recv cares about MPI_Wait, which can be moved to myrank==1 branch
- Test() 不會去等







### Collective communication Routines

- everyone have to call, and blocking till everyone finishes!



##### Barrier & Bcast

![image-20230613182929995](https://p.ipic.vip/wkh9zh.png)



##### Scatter & Gather

![image-20230613183415583](https://p.ipic.vip/hpvo6b.png)

- recv buffer 不需要allocate sendbuffer space



##### Reduce

![image-20230613183542689](https://p.ipic.vip/x7oc0a.png)



##### Allxxx -- 

Allgather: Gather + Bcast

![image-20230613183720841](https://p.ipic.vip/6ke6p8.png)





##### Other APIs

e.g. e-gather --  senders' sendcount are not the same



### Group & Communicator Mngment Routines

![image-20230613184531520](https://p.ipic.vip/ufm9x4.png)

- All are "Collective (Blocking) call"





![image-20230613184740990](https://p.ipic.vip/v969hh.png)

- PS all nodes need to call MPI_Group_incl, since it's a blocking-call





# MPI-IO

<img src="https://p.ipic.vip/prt2d4.png" alt="image-20230613191153074" style="zoom:67%;" />



![image-20230613191303068](https://p.ipic.vip/viiuvm.png)

- split file into small ones -- amount of files is a pressure to FS

- hard to manage!



![image-20230613191641979](https://p.ipic.vip/vd7roi.png)

- again, a collective call(), MPI Lib help sync help manage the tokens



![image-20230613191746053](https://p.ipic.vip/3nj63w.png)



![image-20230613192227639](https://p.ipic.vip/2sbt1s.png)





#### Odd-Even Sort

![image-20230613202013245](https://p.ipic.vip/i6a2fz.png)

![image-20230613202152804](https://p.ipic.vip/rnoqij.png)

![image-20230613202139671](https://p.ipic.vip/mj9ut5.png)

