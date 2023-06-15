---
layout: post
categories: HighPerf
tag: [] 
date: 2022-06-02
---



conformal partitioning 

![image-20230610193910796](https://p.ipic.vip/ly3jg5.png)





The unfortunate truth is that performing a floating-point operation, a multiply or an add is orders of magnitude faster than it is to move data between main memory and registers.





![image-20230610200634635](https://p.ipic.vip/zv29y6.png)


$$
\begin{equation*}
\begin{array}{l}
{\bf for~}  i := 0, \ldots , M-1  \\
~~~ {\bf for~}  j := 0, \ldots , N-1  \\
~~~ ~~~ {\rm Load~}  C_{i,j} \mbox{ into registers} \\
~~~ ~~~ {\bf for~}  p := 0, \ldots , K-1  \\
~~~ ~~~ ~~~ {\rm Load~}  A_{i,p} , {\rm ~and~}  B_{p,j}
\mbox{ into registers} \\
~~~ ~~~ ~~~  C_{i,j} := A_{i,p} B_{p,j} + C_{i,j}  \\
~~~ ~~~ {\bf end} \\
~~~ ~~~ {\rm Store~}  C_{i,j} \mbox{ to memory} \\
~~~ {\bf end} \\
{\bf end}
\end{array}
\end{equation*}
$$
<img src="https://p.ipic.vip/ktzawh.png" alt="img" style="zoom: 50%;" />





![image-20230610202611873](https://p.ipic.vip/j8ts1z.png)





### Vector register and Instructions

SIMD: single instruction multiple data. Where does that come from? The instruction is still an FMA(Fused Mul & Add(, a fuse multiplied add, but we're now doing that same instruction simultaneously on multiple sets of data.Mi



#### Microkernel with Vector Instructions

![image-20230610203721599](https://p.ipic.vip/ne871w.png)

![image-20230610203533705](https://p.ipic.vip/mczaux.png)



```cPP
#include <immintrin.h> 

void Gemm_MRxNRKernel( int k, double *A, int ldA, double *B, int ldB, 
		double *C, int ldC ) 
{
  /* Declare vector registers to hold 4x4 C and load them */
  __m256d gamma_0123_0 = _mm256_loadu_pd( &gamma( 0,0 ) ); 
  __m256d gamma_0123_1 = _mm256_loadu_pd( &gamma( 0,1 ) ); 
  __m256d gamma_0123_2 = _mm256_loadu_pd( &gamma( 0,2 ) ); 
  __m256d gamma_0123_3 = _mm256_loadu_pd( &gamma( 0,3 ) ); 
   	
  for ( int p=0; p<k; p++ ){
    /* Declare vector register for load/broadcasting beta( p,j ) */
    __m256d beta_p_j; 
    
    /* Declare a vector register to hold the current column of A and load 
       it with the four elements of that column. */
    __m256d alpha_0123_p = _mm256_loadu_pd( &alpha( 0,p ) ); 

    /* Load/broadcast beta( p,0 ). */
    beta_p_j = _mm256_broadcast_sd( &beta( p, 0) ); 
    
    /* update the first column of C with the current column of A times 
       beta ( p,0 ) */
    gamma_0123_0 = _mm256_fmadd_pd( alpha_0123_p, beta_p_j, gamma_0123_0 ); 
    
    /* REPEAT for second, third, and fourth columns of C.  Notice that the 
       current column of A needs not be reloaded. */

  
    
  }
  
  /* Store the updated results */
  _mm256_storeu_pd( &gamma(0,0), gamma_0123_0 ); 
  _mm256_storeu_pd( &gamma(0,1), gamma_0123_1 ); 
  _mm256_storeu_pd( &gamma(0,2), gamma_0123_2 ); 
  _mm256_storeu_pd( &gamma(0,3), gamma_0123_3 ); 
}
```



![img](https://p.ipic.vip/o2lg40.png)





ref: https://www.cs.utexas.edu/users/flame/laff/pfhp/LAFF-On-PfHP.html
