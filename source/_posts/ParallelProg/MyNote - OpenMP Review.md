---
layout: post
categories: ParallelProg
tag: [] 
date: 2023-06-21
---



# Notes

OpenMP,  is concerned with a single *cluster node* or *motherboard* , and getting the most out of the available parallelism available there.

```bash
jo@fossa4gb:~/repo/exp_omp$ gcc -fopenmp main.cpp 
/usr/bin/ld: /tmp/cccyI4Oq.o:(.data.rel.local.DW.ref.__gxx_personality_v0[DW.ref.__gxx_personality_v0]+0x0): undefined reference to `__gxx_personality_v0'
collect2: error: ld returned 1 exit status
jo@fossa4gb:~/repo/exp_omp$ mv main.cpp main.c
jo@fossa4gb:~/repo/exp_omp$ gcc -fopenmp main.c  <-- Solved!
jo@fossa4gb:~/repo/exp_omp$ 
```



or PGI



編譯指導語句的格式為：

pragma omp <directive> [clause[[,] clause]...]

directive部分是編譯指導語句的主要指令，用來指導多個CPU共享任務或指導多個

CPU同步；

clause部分是可選的子句，它給出了相應的指令參數，可以影響到編譯指導語句

的具體執行；

注意：換行符是必選項。位於被這個指令包圍的結構塊之前，表示這條編譯指導語向的終止。





```cpp
#pragma omp somedirective clause(value,othervalue)
  statement;

#pragma omp somedirective clause(value,othervalue)
 {
  statement 1;
  statement 2;
 }
```

with

- the `#pragma omp` *sentinel* to indicate that an OpenMP directive is coming;
- a `directive`, such as `parallel` ;
- and possibly `clauses` with values.
- After the directive comes either a single statement or a block in *curly braces* .



透過clause去告訴compiler 然後compiler去gen出來的

- might support nested 



原則上只能一個directive，parallel是個特例



# Constructs

## Parallel Construct

clause mainly used to control # of threads

struct --  包起來 w/ curly brackets

只有 ***Parallel 這個 directive*** 在建thread，其他的directive是在分配事情

- If -- 只在true時建thread，false壓成一個線程跑
- 常會跟著看num_thread的作設定
- there's implicit barrier at the end of construct

e.g. 
```cpp
#include <omp.h>
// Serial code
int A[10], B[10], C[10]
  
// Beginning of parallel section. Fork a team of threads
#pragma omp parallel for num_threads(10)
{
  for (int i = 0; i < 10; i++){
    A[i] = B[i] + C[i];
  }
}/* All threads join master thread and terminate */
```



## Work-Distribution Construct

- divides the execution of the enclosed code region among the threads that encounter it
- DO NOT launch new threads
- there's iimplicit barrier at the end of construct

3 分配方式![image-20230620220318645](/Users/joe/Library/Application Support/typora-user-images/image-20230620220318645.png)

### 1) Do/For Directive

- shares **iteration** across the team

- Represents a type of **data parallelism**

- Clauses:

  - nowait: don't wait for other threads 
  - **schedule**: it has built-in scheduleing
    - STATIC:
      Look divided into **chunks**, Round-Robin, chunk size big, hard to balance
    - DYNAMIC:
      when a thread finishes one chunk(default size:1), its *dynamically assigned another*
    - **★GUIDED**:
      ~DYNAMIC, except chunk size↓ over time (*better load balancing*)
    - RUNTIME: w/ env: $OMP_SCHEDULE
    - AUTO: 
      delegated to the compiler, usually sucks! XD
  - ordered: Iteration must exec in serial program, usually useless XD
  - collapses: flatten nested loops into 1D

- Examples

  - General

    ```cpp
    #inclue <omp.h>
    #define NUM_THREAD 2
    #define CHUNKSIZE 100
    #define N 1000
    main() {
      int a[N], b[N], c[N];
      /*some Inits*/
      for (int i = 0; i < N; i++) a[i] = b[i] = i;
      int chunk = CHUNKSIZE;
      int thread = NUM_THREAD;
      
    	#pragma omp parallel num_thread(thread) shared(a, b, c) private(i)
      {
        #pragma omp for schedule(dynamic, chunk) nowait
        for(int i = 0; i < n; i++) c[i] = a[i] + b[i];
      }
    }
    ```

  - Collapse

    ```cpp
    #pragma omp parallel num_thread(6)
    #pragma omp for schedule(dynamic) collapse(2)
    
    for (int i = 0; i < 3; i++)
      for (int j = 0; j < 3; j++)
        printf("i=%d, j=%d, thread=%d\n", i, j, omp_get_thread_num());
    ```

       - better & balance performance

       - no data dependency among diff layers

       - in [Manual](https://www.openmp.org/wp-content/uploads/OpenMPRefCard-5-2-web.pdf), there's **omp_set_max_active_levels** to set

         

### 2) SECTIONS Directive

- a section is a segment of code, which executed by a thread

- section 不會被重覆做的

- 上面應該已經有講了parallel, already created the threads

- available thread 會先做，然後看有幾個section就幾個thread被assign過去做事

- mapping btw thread & section is not able to control

- Examples

  - General
    ```cpp
    int N = 1000
    int a[N], b[N], c[N], d[N]
      
    #pragma omp parallel num_thread(2) shared(a,b,c,d) private(i)
    {
      #pragma omp sections	/*specify sections*/
      {
        #pragma omp section	/* 1st section */
        {
          for (int i = 0; i < N; i++) c[i] = a[i] + b[i];
        }
        #pragma omp section	/* 2nd section */
        {
          for (int i = 0; i < N; i++) d[i] = a[i] + b[i];
        }    
      } /* end of sections */
    }	/* end of parallel section */



### 3) SINGLE Directive

- blocking
- e.g. I/O, only requires one thread to write, but I don't know which thread is assigned the task
- blocking other no-doing threads, unless "`nowait`" is added after "single"
- 這樣就不用跳出目前的parallel region 然後再進去到另一個parallel region, 就用個Single 解決，然後code也比較好看

- Examples

  - General
    ```cpp
    int input;
    #pragma omp parallel num_thread(10) shared(input)
    {
      // computing code that can be processed in parallel
      #pragma omp single	/*specify section*/
      {
        scanf("%d", &input);
      }	/* end of serialized I/O call*/
      
      printf("input is %d", input);
    }	/* end of parallel section */



## Synchronization Construct

pretty similar to Single

### Directives

master, barrier, `critical` (gross-grain), `atomic`(fine-grain; which is. like monitor in java, using synchornized to a protected var)



## LOCK Routines

void omp_init_lock(), void omp_destroy_lock(), void omp_set_lock(), void tmp_unset_lock()

int omp_test_lock() 

### Examples Comparison

The following 2 segments of code are the same, and there's still advantage of using critical over lock:

- no need to declare, init and destroy a lock
- you always have explicit control over where your critical section ends
- Less overhead with compiler assist

#### With OpenMP

```cpp
#inlcude <omp.h>
main(){
  int count = 0;
  #pragma omp parallel 
  	#pragma omp critical
  		count++;
}
```

#### With Manual Call

```cpp
#include <omp.h>
main(){
	int count = 0;
  omp_lock_t *lock;
  omp_init_lock(lock);
  #pragma omp parallel
  {
    omp_set_lock(lock);
    count++;
    omp_unset_lock(lock);
  }
  omp_destroy_lock(lock);
}
```



# Clauses



- 因為都是compiler gen的code, 所以openmp所見的code跟實際的差別會在要對data的scope了解

- for data data scope

- private variable or shared variable?
  ![image-20230621193839393](/Users/joe/Library/Application Support/typora-user-images/image-20230621193839393.png)

- by default, a variable is shared-variable(global variable)

- Types

  - private, shared, 

  - firstprivate: 

    - **Inited** according to the value of their original objects prior to entry into the parallel region

    - e.g.
      ```cpp
      int va1 = 10;
      #pragma omp parallel firstprivate(var1)
      {
        printf("var1:%d" var1);
      }
      ```

      all threads see var1 equals to 10, but different memory references, and are inited to 10

  - lastprivate:

    - with a copy from the **LAST** loop iteration or section to the original variable object

    - e.g.
      ```cpp
      int var1 = 10
      #pragma omp parallel lastprivate(va1) num_thread(10)
      {
        int id = omp_get_thread_num();
        sleep(id);
        var1 = id;
      }
      printf("var1:%d", var1);
      ```

      printed out var1 would be the result of the "slowest" thread

  - default 

    - to specify a default scope for ALL variables in the parallel region

  - `copyin`(var_list) VS `copyprivate`(var_list)

    - copyin:
      assigning the same variable value based on the instance from the master thread; as the "broadcast to worker threads from main thread"
    - copyprivate
      **broadcast values** acquired by a single thread directly to all instances in the other threads; Associated with the SINGLE directive

  - `reduction` (operator: var_list)

    - a private copy for each list variable is created for each thread

    - performs a **reduction on all riable instances**

    - Write the **final result to the global shared copy**

    - e.g.
      ```cpp
      #include <omp.h>
      main(){
        int i = 0, n = 10, chunk = 2, result = 0;
        int a[100], b[100];
        for (int i = 0; i < n; i++) a[i] = b[i] = i;
      #pragma omp parallel default(shared) private(i) schedule(static, chunk) reduction(+:result)
        {
          for (int i = 0; i < N; i++){
            result = result + a[i] * b[i]
          }
        }
        printf("Final result = %f\n", result);
      }
      ```

      i 定是個local, 因為是for的index，當然是個local

      reduction用了，會把result這個default是share的作overwrite成private 的local variable ；
      就是說這個for loop的結果要針對result這個值去作累加，也等同第9行 `result=result+ `這個動作；但這個result其實是個private的variable ，是沒有share的。我們這邊是希望不同的thread只要負責某些index的乘法，然後各別有一個"partial sum"，然後離開了這個parallel region時候，再把這些partial sum加在一起，變成外面第12行的這個global的total的sum
      至於怎麼分配的？就去看 schedule的策略以及那個chunk
      中間也不需要再加critical section 
      ref **[Video](https://youtu.be/xk6xpx8HY6s?t=421)**

## 	Summary Table

![image-20230621190410743](https://p.ipic.vip/kw4gra.png)



# Routines

generally, query infos, 就是一些輔助的fn calls

e.g. 
![image-20230621191328781](https://p.ipic.vip/gk650m.png)





# Other Appendix

- when compile scikit-learn

  ![image-20230619232207661](https://p.ipic.vip/g7j59e.png)





omp編程規範

開源的多線程處理器

gcc也是開源的

編譯指導語句，這些表達示有一定的區別

![image-20230620112406786](/Users/joe/Library/Application Support/typora-user-images/image-20230620112406786.png)

![image-20230620112528350](https://p.ipic.vip/qn52kg.png)



![image-20230620112706782](https://p.ipic.vip/xexgpx.png)



## Prof to see Perf





ref: 

1 https://theartofhpc.com/pcse/index.html

2  [周志遠平行程式 9B](https://youtu.be/xk6xpx8HY6s?t=421)