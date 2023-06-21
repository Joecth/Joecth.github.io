---
layout: post
categories: ParallelProg
tag: [] 
date: 2023-06-21
---



## Parallel Regions

The following example uses parallelism for an actual calculation:

```cpp
result = f(x)+g(x)+h(x)
```

could parallelize this as

```cpp
double result,fresult,gresult,hresult;
#pragma omp parallel
{ int num = omp_get_thread_num();
  if (num==0)      fresult = f(x);
  else if (num==1) gresult = g(x);
  else if (num==2) hresult = h(x);
}
result = fresult + gresult + hresult;
```



**Remark**  In 5.1 the master construct will be deprecated, and masked (with added functionality) will take its place. 



### Nested Parallelism

```cpp
int main() {
  ...
#pragma omp parallel
  {
  ...
  func(...)
  ...
  }
} // end of main
void func(...) {
#pragma omp parallel
  {
  ...
  }
}
```

-  `OMP_MAX_ACTIVE_LEVELS` (default: 1) to set the number of levels of parallel nesting. Equivalently, there are functions `omp_set_max_active_levels` and



## Loop Parallelism

```cpp
#pragma omp parallel
{
  code1();
#pragma omp for
  for (int i=1; i<=4*N; i++) {
    code2();
  }
  code3();
}
```

<img src="https://p.ipic.vip/juxv0n.png" alt="image-20230621202839429" style="zoom: 50%;" />

- Loop cannot contain break, return ,exit
- OK to have continue 
- Index should be an increment (or decrement) by a <u>fixed amount</u>, and no changes inside the lo

### Nested Loops

- `collapse(num)` can only collapse "perfectly nested loops" -- outer loop can consist ONLY of the inner loop;



## SIMD Processing

**Remark** Depending on your compiler, it may be necessary to give an extra option enabling SIMD:

- `-fopenmp-simd` for *GCC* / *Clang* , and
- `-qopenmp-simd` for *ICC* .

end of remark



If a loop is both multi-threadable and vectorizable, you can combine directives as `pragma omp parallel for simd` .

Compilers can be made to report whether a loop was vectorized:

```
   LOOP BEGIN at simdf.c(61,15)
      remark #15301: OpenMP SIMD LOOP WAS VECTORIZED
   LOOP END
```

with such options as `-Qvec-report=3` for the Intel compiler.



## 

multi-threading and vectorization are two different optimization concepts, each targeting a different level of parallelism.

1. Multi-threading: Multi-threading is a method of parallel computing that divides a task into multiple independent subtasks, each executed in a separate thread. This allows for the simultaneous utilization of the computational power of multiple cores, resulting in faster overall computation. In the given example, the OpenMP directive `pragma omp parallel` is used, indicating that the iterations of the loop can be executed in parallel across different threads.
2. Vectorization: Vectorization is an optimization technique that utilizes vector instructions (SIMD instructions) of the processor to perform operations on multiple data elements simultaneously, enhancing computational efficiency. By packing multiple data operations into a single vector instruction, multiple data elements can be processed in a single instruction execution. In the provided example, the OpenMP directive `pragma omp simd` is used, indicating that the data operations within the loop can be vectorized.

To summarize, multi-threading and vectorization are both optimization techniques for parallel computing, but they target different levels of parallelism. Multi-threading leverages the parallelism capabilities of multi-core processors by distributing tasks among different threads. On the other hand, vectorization exploits vector instructions of the processor to process multiple data elements simultaneously, improving efficiency at the instruction level. In some cases, multi-threading and vectorization can be combined to further enhance computational performance.



ref: 

[The Art of HPC](https://theartofhpc.com/pcse/index.html)

https://hackmd.io/@kosl/week2#21-Welcome-to-Week-2
