---
layout: post
categories: SIMD
tag: [] 
date: 2022-5-28
---

 



Numerical Algs --> Optimizing for Performance --> Writing Parallel Algs

# Law

## 1. Amdahl's Law

   阿姆达尔定律是一个计算机科学界的经验法则，因IBM公司计算机架构师吉恩·阿姆达尔而得名。吉恩·阿姆达尔在1967年发表的论文中提出了这个重要定律。
   阿姆达尔定律主要用于发现仅仅系统的部分得到改进，整体系统可以得到的最大期望改进。它经常用于并行计算领域，用来预测适用多个处理器时理论上的最大加速比。在我们的性能调优领域，我们利用此定律有助于我们解决或者缓解性能瓶颈问题。
   阿姆达尔定律的模型阐释了我们现实生产中串行资源争用时候的现象。如下图模型，一个系统中，不可避免有一些资源必须串行访问，这限制了我们的加速比，即使我们增加了并发数（横轴），但取得效果并不理想，难以获得线性扩展能力(图中直线)。

## 2. Gustafson's Law

   Gustafson定律(Gustafson’s law)阐述了数据并行带来的影响。Gustafson 定律是由 John L. Gustafson 在1988年提出的。是并行计算领域除了 Amdahl 定律之后又一个重要定律。Gustafson定律看待加速比的角度会有所不同，其站在对于大量数据流处理过程中，针对重复工作的分布式运算带来的加速特性，而不仅仅局限在一个单一的程序进程&单一的计算机上，串行的部分主要在于数据的分配预处理以及计算结果的收集汇总处理上。



compiler optimization 

Options: -O1~ -O3, Ofast



![image-20230606203406991](https://p.ipic.vip/qb9bbd.png)



![image-20230606203539104](https://p.ipic.vip/8gn0if.png)





![Visual representation of latencies](https://p.ipic.vip/njwuzm.png)









Pipeline and HW oriented Design



-  sudo apt install linux-tools-generic



# Perf Stat

Branch prediction failed if unpredictable pattern in code would make the task-clock longer

```bash
(venv-py311) jo@fossa4gb:~/repo/V10793_C/Section 3/video2$ sudo perf stat ./spec

 Performance counter stats for './spec':

            342.40 msec task-clock                #    0.996 CPUs utilized          
                16      context-switches          #    0.047 K/sec                  
                 0      cpu-migrations            #    0.000 K/sec                  
             19574      page-faults               #    0.057 M/sec                  
   <not supported>      cycles                                                      
   <not supported>      instructions                                                
   <not supported>      branches                                                    
   <not supported>      branch-misses                                               

       0.343925345 seconds time elapsed

       0.243236000 seconds user
       0.099686000 seconds sys
       
(venv-py311) jo@fossa4gb:~/repo/V10793_C/Section 3/video2$ sudo perf stat ./spec2

 Performance counter stats for './spec2':

            465.56 msec task-clock                #    0.986 CPUs utilized          
                53      context-switches          #    0.114 K/sec                  
                 2      cpu-migrations            #    0.004 K/sec                  
             19574      page-faults               #    0.042 M/sec                  
   <not supported>      cycles                                                      
   <not supported>      instructions                                                
   <not supported>      branches                                                    
   <not supported>      branch-misses                                               

       0.472287268 seconds time elapsed

       0.348851000 seconds user
       0.117635000 seconds sys


    for(j=0;j<MAX;j++) {
        // i = rand() % 3;
        i = j % 3;      //spec1
        i = rand() % 3; //spec2
        switch(i) {
```



- unroll also makes faster







# AVX

[Intel® Intrinsics Guide](https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html)

x86 SIMD History

- MMX-SSE4 (1997-2006)
  O Early SIMD was integer-only, later extended to floating point
- AVX (2008)
  O 128 and 256 bit SIMD instructions with 16 new registers
- AVX2 (2013)
  O Introduced with Haswell, extends AVX to 256 bit, adds FMA
- AVX-512 (2015)
  O Originally in Xeon Phi KNL, extended to Skylake+: 512 bit extensions





![image-20230606211435215](https://p.ipic.vip/s60b9d.png)



Explore caches, branching, and memory speeds

Superscalar & pipelining

Staying out of the compiler's way

Vectorization code
