---
layout: post
categories: HighPerf
tag: [] 
date: 2023-03-02
---



![image-20230625191321163](https://p.ipic.vip/wcsp4h.png)

for (int i = tid; i<m; i+=nthreads) 在加了 pragma 後就是用 i++就可

> rom Wikipedia:
>
> “A multi-core processor is a single computing component with two or more independent processing units called cores, which read and execute program instructions.”
>
> 
>
> A thread is a thread of execution.
>
> - It is an stream of program instructions and associated data. All these instructions may execute on a single core, multiple cores, or they may move from core to core.
> - Multiple threads can execute on different cores or on the same core.
>
> In other words:
>
> - Cores are hardware components that can compute simultaneously.
> - Threads are streams of execution that can compute simultaneously.





ref: 
1 [PfHP](https://www.cs.utexas.edu/users/flame/laff/pfhp/week4-of-cores-and-threads.html#p-1288-part2)

2 Tim Mattson’s (Intel) [“Introduction to OpenMP”](https://www.youtube.com/playlist?list=PLLX-Q6B8xqZ8n8bwjGdzBJ25X2utwnoEG) (2013) on YouTube.

- Slides: [Intro_To_OpenMP_Mattson.pdf](https://www.openmp.org/wp-content/uploads/Intro_To_OpenMP_Mattson.pdf)
- Exercise files: [Mattson_OMP_exercises.zip](https://www.openmp.org/wp-content/uploads/Mattson_OMP_exercises.zip)
