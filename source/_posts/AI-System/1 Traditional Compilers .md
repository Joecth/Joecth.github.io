---
layout: post
categories: AI-System
tag: [] 
date: 2023-05-27
---





Interpreter VS Compiler

JIT vs AOT

Pass & IR





## History of Compilers -- GCC VS LLVM

![image-20230602161322366](https://p.ipic.vip/wjd955.png)

Compiler --  e.g. turn hello.c to hello.o(binary)

```tex
Sourcecode --> 		Compiler --> 		exe binary
​						(FE, IR Optimizer, BE)
(to AST)												(MachineCode)
Abstract Syntax Tree
```





GCC 1.0 @ 1987

<img src="https://p.ipic.vip/5y76rq.png" alt="image-20230602160844823" style="zoom:50%;" />

​						Free the Free!

(Richard Matthew Stallman)



Apple -- Free! but OS kernel pretty closed

![image-20230602161010238](https://p.ipic.vip/mxxzdg.png)



### Apple met LLVM

<img src="https://p.ipic.vip/lm33h3.png" alt="image-20230602161146709" style="zoom: 25%;" />





### GCC

<img src="https://p.ipic.vip/bcso75.png" alt="image-20230602161456787" style="zoom: 50%;" />

<img src="https://p.ipic.vip/eht8wd.png" alt="image-20230602161738949" style="zoom:67%;" />





### LLVM 

low level VM

Compiler Toolchain

<img src="https://p.ipic.vip/wwjwth.png" alt="image-20230602161604175" style="zoom: 67%;" />





Introduced IR! bettern than GCC

<img src="https://p.ipic.vip/4jgjjm.png" alt="image-20230602161815300" style="zoom:67%;" />





## CLANG VS GCC

![image-20230602162331702](https://p.ipic.vip/cqugk4.png)



![image-20230602162406871](https://p.ipic.vip/304pgi.png)

![image-20230602162448009](https://p.ipic.vip/n0rsd7.png)

![image-20230602162529839](https://p.ipic.vip/rtj8ia.png)

​	- *.ll  -- huan readable



- ```bash
  > clang -emit-llvm hello.i -c -o hello.bc
  > code hello.bc
  > od -b hello.bc
  
  $ clang -ccc-print-phases hello.cpp
                 +- 0: input, "hello.cpp", c++
              +- 1: preprocessor, {0}, c++-cpp-output
           +- 2: compiler, {1}, ir
        +- 3: backend, {2}, assembler
     +- 4: assembler, {3}, object
  +- 5: linker, {4}, image
  6: bind-arch, "arm64", {5}, image
  
  (base)
  # joe @ J-M1-Pro-16 in ~/exp [16:35:49]
  $ cat hello.s | less
  	.cfi_endproc
                                          ; -- End function
  	.p2align	2                               ; -- Begin function _ZNSt3__18ios_base8setstateEj
  __ZNSt3__18ios_base8setstateEj:         ; @_ZNSt3__18ios_base8setstateEj
  	.cfi_startproc
  ; %bb.0:
  	sub	sp, sp, #32
  	stp	x29, x30, [sp, #16]             ; 16-byte Folded Spill
          .section        __TEXT,__text,regular,pure_instructions
          .build_version macos, 12, 0     sdk_version 12, 3
          .globl  _main                           ; -- Begin function main
          .p2align        2
  _main:                                  ; @main
          .cfi_startproc
  ; %bb.0:
          stp     x29, x30, [sp, #-16]!           ; 16-byte Folded Spill
          mov     x29, sp
          .cfi_def_cfa w29, 16
          .cfi_offset w30, -8
          .cfi_offset w29, -16
          adrp    x0, l_.str@PAGE
          add     x0, x0, l_.str@PAGEOFF
          bl      _printf
          adrp    x0, __ZNSt3__14coutE@GOTPAGE
  ```



### LLVM IR★

![image-20230602163418928](https://p.ipic.vip/e85fsn.png)

- SSA - Static Single Assignment 
  - ~  RISC ISA
  - 3-address-code
    - <img src="https://p.ipic.vip/jwvvo5.png" alt="image-20230602163826992" style="zoom:33%;" />
  - inf registers
- Structure
  - Module -> Function -> Basicblock -> Instruction
- Concepts
  - Value, Use, User, 



### LLVM FE

<img src="https://p.ipic.vip/qtr860.png" alt="image-20230602164519532" style="zoom: 50%;" />

- Lexical Analysis --> Syntatical Analysis --> Semantic Analysis

  - ````bash
    10018  clang -cc1 -dump-tokens hello.cpp
    10019  clang -fsyntax-only -Xclang -ast-dump hello.cpp
    10020  clang -c hello.cpp



### LLVM Optimizer

- Finding PASS

  - Analysis Pass

  - Transform pass

  - ```	bash
    > opt hello.bc -instcount -time-passes -domtree -o hello-tmp.bc -stats
    ```

  - [LLVM’s Analysis and Transform Passes — LLVM 17.0.0git documentation](https://llvm.org/docs/Passes.html)

    - e.g. -adce: Aggressive Dead Code Elimination
    - e.g.  -constmerge: Merge Duplicate Global Constants

  - ModulePass, FunctionPass, BasicBlockPass



### LLVM BE

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230602165508513.png" alt="image-20230602165508513" style="zoom: 40%;" />



#### SelectionDAG -> Scheduling

![image-20230602165708989](https://p.ipic.vip/3r23t5.png)



Topological sort to know nodes that are able to run parallelly



#### MCInst -> Code Emission



#### Review

![image-20230602165926323](https://p.ipic.vip/gtnep5.png)



## OnGoing History

Chris start his bizness at "Modular"



## LLVM Related AI Compilers

built on LLVM, including

- XLA in TF,  by Google
- JAX, autograd + XLA, by Google
- TensorFlow, on XLA, then on LLVM
- TVM for different hardwares
- Julia, for HPC, using LLVM JIT





ref to 

- series of [AI videos]( https://youtu.be/i5_BptwCBHA) on YT
