---
layout: post
categories: AI-System
tag: [] 
date: 2023-05-29
---





# Why we need AI Compiler

- OPs ↑
- HW vendors release similar libs, e.g. CuDNN, MKL-DNN for their own chips
- Transplanation issue, e.g. CPU & GPU's existing  Pass hard to be transplanted to NPU





# AI Compiler

- auto optimize the process/code
- Introduce Pass
- FE -> Opt -> BE
- Main purpose --> Optiize the performance, and then make coding easier

<img src="https://p.ipic.vip/hvdjk2.png" alt="image-20230602171205297" style="zoom:33%;" />



### Difference from Traditional Compiler

![image-20230602171410674](https://p.ipic.vip/vsjbeo.png)



- IR 
  - for AI
    - High-level IR, to describe the computational graph
  - for Traditional IR
    - Low-level IR, e.g. load, store
- Pass
  - for AI
    - OPs fusion
    - able for Quantization
  - for Traditional 
    - no Quantization



# AI Compiler Architecture

Purposes: Inference or Train?!



- Python as FE interpreter
- Multilevel IR, including graph-compilzation, OP compilation, code-gen
- for NN
- DSA for chips



### History

![image-20230602171958136](https://p.ipic.vip/o4kzrn.png)

I had experiences in Stage I in 2017~early2019

and now being involved in Stage II



#### Stage II

<img src="https://p.ipic.vip/6y3qzh.png" alt="image-20230602172529020" style="zoom:67%;" />



#### Stage III

![image-20230602172708408](https://p.ipic.vip/vgjo4j.png)







ref: [AI videos series](https://youtu.be/bW3gsz9AjPY) on YT  



### General Architecture

![image-20230602190537524](https://p.ipic.vip/gr6gg4.png)

From paper -- <u>*The Deep LearningCompiler: A Comprehensive Survey*</u>



- IR - 

  1. High-level

  2. Low-level

- FE Opti.

  compose graph with python language

  1. Nodes level --  zero-dim-tensor elimination, Nop elimination
  2. Block level -- algebraic simplify, constant folding ,op fusion
  3. Data Stream level --  Common sub-expression elimination, DCE(data communcation equip.)

- BE Opti.

  1. Spefific hardware
     - low-level IR --> LLVM IR --> GPU/CPU codegen
     - Domain knowhow
  2. Auto-adjustment
     - Halide/TVM allows the **Separatino of Scheduleing and Compute**
     - <u>Polyhedral model</u> for param adjustment
  3. Kernel libs
     - from libs, e.g. NVidia's lib

![image-20230602191320979](https://p.ipic.vip/3orvbk.png)



# Challenges & Future

1. Dynamic Shape problems

2. Python Static
   ![image-20230602192507040](https://p.ipic.vip/j07i81.png)

   ![image-20230602192610709](https://p.ipic.vip/agcvp6.png)

3. Specific Optimization 

   - auto parallel
     - Scale out 
     - Scale up
   - HPC for Jacobian matrix, for Hessian matrix

4. Transparency 

5. Compiling cost、Overhead



- Umifying representation of graph?
- Umifying optimization for compilation?



![image-20230602193127645](https://p.ipic.vip/ypwr0w.png)

