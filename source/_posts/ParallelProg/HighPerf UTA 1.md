---
layout: post
categories: HighPerf
tag: [] 
date: 2022-06-01
---

 



**HW requirements**

- Intel architectures: Haswell, Broadwell, Skylake, Kaby Lake, Coffee Lake.
- AMD architectures: Ryzen/Epyc

```bash
/content/drive/MyDrive/mySIMD# gcc -march=native -Q --help=target|grep march
  -march=                               haswell
```



```bash
content/drive/MyDrive/mySIMD# more /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 63
model name      : Intel(R) Xeon(R) CPU @ 2.30GHz
```



```bash
/content/drive/MyDrive/mySIMD/repos/LAFF-On-PfHP/Assignments/Week0/C# make HelloWorld 
gcc driver.o FLA_Clock.o MaxAbsDiff.o RandomMatrix.o /root/blis/lib/libblis.a -o driver.x -lpthread -m64 -lm -fopenmp 
echo 

./driver.x
Hello World
Hello World
```



Understand how alg and architectures interact

Using e.g. of matrix-matrix multiply 

### GEMM - GEneral Matrix Multiplication


$$
\begin{equation*}
A = 
\left(\begin{array}{cccc}
\alpha_{0,0} & \alpha_{0,1} & \cdots & \alpha_{0,k-1} \\
\alpha_{1,0} & \alpha_{1,1} & \cdots & \alpha_{1,k-1} \\
\vdots & \vdots & & \vdots \\
\alpha_{m-1,0} & \alpha_{m-1,1} & \cdots & \alpha_{m-1,k-1} 
\end{array}\right),
B = 
\left(\begin{array}{cccc}
\beta_{0,0} & \beta_{0,1} & \cdots & \beta_{0,n-1} \\
\beta_{1,0} & \beta_{1,1} & \cdots & \beta_{1,n-1} \\
\vdots & \vdots & & \vdots \\
\beta_{k-1,0} & \beta_{k-1,1} & \cdots & \beta_{k-1,n-1} 
\end{array}\right)
\end{equation*}
$$

$$
\begin{equation*}
C = 
\left(\begin{array}{cccc}
\gamma_{0,0} & \gamma_{0,1} & \cdots & \gamma_{0,n-1} \\
\gamma_{1,0} & \gamma_{1,1} & \cdots & \gamma_{1,n-1} \\
\vdots & \vdots & & \vdots \\
\gamma_{m-1,0} & \gamma_{m-1,1} & \cdots & \gamma_{m-1,n-1} 
\end{array}\right).
\end{equation*}
$$


$C := A B + C \text{.}$


$$
\begin{equation}
\gamma_{i,j} :=  \sum_{p=0}^{k-1} \alpha_{i,p} \beta_{p,j} + \gamma_{i,j}\label{week1-eqn-gamma}\tag{1.1.1}
\end{equation}
$$

$$
\begin{equation*}
\begin{array}{l}
{\bf for~} i := 0, \ldots , m-1  \\
~~~ {\bf for~} j := 0, \ldots , n-1  \\
~~~~~~ {\bf for~} p := 0, \ldots , k-1  \\
~~~~~~~~~ \gamma_{i,j} := \alpha_{i,p} \beta_{p,j} + \gamma_{i,j}  \\
~~~~~~{\bf end} \\
~~~{\bf end} \\
{\bf end}
\end{array}
\end{equation*}
$$

### Pseudo Code to C code

```c
#define alpha( i,j ) A[ (j)*ldA + i ]   // map alpha( i,j ) to array A 
#define beta( i,j )  B[ (j)*ldB + i ]   // map beta( i,j )  to array B
#define gamma( i,j ) C[ (j)*ldC + i ]   // map gamma( i,j ) to array C

void MyGemm( int m, int n, int k, double *A, int ldA,
	     double *B, int ldB, double *C, int ldC )
{
  for ( int i=0; i<m; i++ )
    for ( int j=0; j<n; j++ )
      for ( int p=0; p<k; p++ )
        gamma( i,j ) += alpha( i,p ) * beta( p,j );
}
```



Gemm "Gemm" is a commonly used acronym that stands for "Ge"neral "m"atrix "m"ultiplication.

The term "driver" is typically used for a main program that exercises (tests and/or times) functionality implemented in a routine. In our case, the driver tests a range of problem sizes by creating random matrices C , A , and B . 



execution time as a function of the problem size!



### GFLOPS

![image-20230608013934605](https://p.ipic.vip/us5l1t.png)



<img src="https://p.ipic.vip/pgaej9.png" alt="image-20230608014505513" style="zoom:67%;" />

![image-20230608014348331](https://p.ipic.vip/tcasfw.png)





Mapping Matrices to memory

- Row major order (X)
- Col major order (V)
  - B.C Computational science used to used the Fortran programming language to program, and the Fortan programming language chose to store matrices by `columns major order`
  - ![image-20230608111535626](https://p.ipic.vip/qj1dak.png)<img src="https://p.ipic.vip/pzjxnb.png" alt="image-20230608111832859" style="zoom:67%;" />



The leading dimension

- ![5x4 matrix](https://p.ipic.vip/4p52nk.png)
  - The leading dimension of the boxed submatrix is : 5





 A convention regarding the letter used for the loop index

- why do the two implementations with better performance do better?
  1. They access matrices by columns in the inner loop.
  2. We store matrices in column-major order.
  3. Accessing data contiguously usually improves performance.





Nototion
$$
A =
	      \left( \begin{array}{r r r}
	      -2 & -3 & -1 \\
	      2 & 0 & 1 \\
	      3 & -2 & 2 \\
	      \end{array}
	      \right).
$$


- $$\alpha_{1,2} = 1$$

- $$a_{0} = 	  \left( \begin{array}{r r r} 	  -2 \\ 	  2 \\ 	  3  	  \end{array} 	  \right)$$

- $$\widetilde a_{2}^T 	  = 	  \left( \begin{array}{r r r} 	  3 & -2 & 2  	  \end{array} 	  \right)$$





Dot Product
$$
\begin{array}{l c r}
	      {\tt A[0]} & \longrightarrow & -1 \\
	      &&0 \\
	      && 2 \\
	      && 3 \\
	      && -1 \\
	      && 1 \\
	      && 4 \\
	      && -1 \\
	      && 3 \\
	      && 5 
	      \end{array}
$$

- Dots( 3, &A[1], 4, &A[3], 1, &gamma ) = $$1 + \left( \begin{array}{r}       0 \\       1 \\       5        \end{array} \right)^T       \left( \begin{array}{r}       3 \\       -1 \\       1        \end{array} \right)       =       1 + (0) \times (3) + (1) \times (-1) + (5) \times (1)       = 5.$$

> In particular, the stride when accessing a row of a matrix is ldA when the matrix is stored in column-major order with leading dimension ldA,

- \#define alpha(i,j) A[ (j)*ldA + (i)] to address the matrix in a more natural way





Matrix-vector multiplication via dot-product

![image-20230608152820750](https://p.ipic.vip/mv01r4.png)

<img src="https://p.ipic.vip/1ylxqs.png" alt="image-20230608154753003" style="zoom:80%;" />





Axpy

- alpha times x plus y
  - where alpha times x is a "broadcast" operation





Matrix-vector multiplication via axpy operations

## ![image-20230608155913443](https://p.ipic.vip/jrsjlx.png)

General form
$$
\begin{equation*}
\begin{array}{rcl}
y :=
\left( \begin{array}{c | c | c | c}
a_0  a_1  \cdots  a_{n-1} 
\end{array} \right)
\left( \begin{array}{c}
\chi_0 \\ \hline
\chi_1 \\ \hline
\vdots \\ \hline
\chi_{n-1}
\end{array}
\right) + y \\
=
\chi_0 a_0 + \chi_1 a_1 + \cdots + \chi_{n-1} a_{n-1} + y.
\end{array}
\end{equation*}
$$
![image-20230608161328342](https://p.ipic.vip/z8nmcl.png)



Gemm in terms of Gemv

![image-20230608161910223](https://p.ipic.vip/8r9a9l.png)



![image-20230608162233030](https://p.ipic.vip/t3qcip.png)





## Layering Matrix-Matrix Multiplication

rank-1 update by columns

Matrix-matrix multiplication via rank-1 updates

<img src="https://p.ipic.vip/09jkbw.png" alt="img" style="zoom: 50%;" />

- notice that here we're actually adding to a matrix. And it's called rank-1 update because an **`outer product` has a rank of at most 1**
  - e,g, 
    - A = [1, 2, 3] B = [4, 5, 6], A ⊗ B = [ 4, 5, 6 ] [ 8, 10, 12 ] [12, 15, 18 ], and the 1st row can represent the remaining 2 rows, so the 2nd and 3rd are not linear independent to 1st row, so rank is one!







### Row-times-matrix multiplications







ref: 

[1] https://www.cs.utexas.edu/users/flame/laff/pfhp/week0-what-should-we-know.html
