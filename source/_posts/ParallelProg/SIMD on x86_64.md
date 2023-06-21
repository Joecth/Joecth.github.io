---
layout: post
categories: SIMD
tag: [] 
date: 2023-6-15
---



# SIMD on X86_64

```bash
/content/drive/MyDrive/mySIMD# lscpu 
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   46 bits physical, 48 bits virtual
CPU(s):                          2
On-line CPU(s) list:             0,1
Thread(s) per core:              2
Core(s) per socket:              1
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       GenuineIntel
CPU family:                      6
Model:                           79
Model name:                      Intel(R) Xeon(R) CPU @ 2.20GHz
Stepping:                        0
CPU MHz:                         2199.998
BogoMIPS:                        4399.99
Hypervisor vendor:               KVM
Virtualization type:             full
L1d cache:                       32 KiB
L1i cache:                       32 KiB
L2 cache:                        256 KiB
L3 cache:                        55 MiB
NUMA node0 CPU(s):               0,1
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Mitigation; PTE Inversion
Vulnerability Mds:               Vulnerable; SMT Host state unknown
Vulnerability Meltdown:          Vulnerable
Vulnerability Mmio stale data:   Vulnerable
Vulnerability Retbleed:          Vulnerable
Vulnerability Spec store bypass: Vulnerable
Vulnerability Spectre v1:        Vulnerable: __user pointer sanitization and usercopy 
                                 barriers only; no swapgs barriers
Vulnerability Spectre v2:        Vulnerable, IBPB: disabled, STIBP: disabled, PBRSB-eI
                                 BRS: Not affected
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Vulnerable
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge 
                                 mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht sy
                                 scall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl
                                  xtopology nonstop_tsc cpuid tsc_known_freq pni pclmu
                                 lqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe p
                                 opcnt aes xsave avx f16c rdrand hypervisor lahf_lm ab
                                 m 3dnowprefetch invpcid_single ssbd ibrs ibpb stibp f
                                 sgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpc
                                 id rtm rdseed adx smap xsaveopt arat md_clear arch_ca
                                 pabilities
```



```bash
~# gcc -march=native -c -Q --help=target
The following options are target specific:
  -m128bit-long-double                  [enabled]
  -m16                                  [disabled]
  -m32                                  [disabled]
  -m3dnow                               [disabled]
  -m3dnowa                              [disabled]
  -m64                                  [enabled]
  -m80387                               [enabled]
  -m8bit-idiv                           [disabled]
  -m96bit-long-double                   [disabled]
  -mabi=                                sysv
  -mabm                                 [enabled]
  -maccumulate-outgoing-args            [disabled]
  -maddress-mode=                       long
  -madx                                 [enabled]
  -maes                                 [enabled]
  -malign-data=                         compat
  -malign-double                        [disabled]
  -malign-functions=                    0
  -malign-jumps=                        0
  -malign-loops=                        0
  -malign-stringops                     [enabled]
  -mandroid                             [disabled]
  -march=                               broadwell
  -masm=                                att
  -mavx                                 [enabled]
  -mavx2                                [enabled]
  -mavx256-split-unaligned-load         [disabled]
  -mavx256-split-unaligned-store        [disabled]
  -mavx5124fmaps                        [disabled]
  -mavx5124vnniw                        [disabled]
  -mavx512bitalg                        [disabled]
  -mavx512bw                            [disabled]
  -mavx512cd                            [disabled]
  -mavx512dq                            [disabled]
  -mavx512er                            [disabled]
  -mavx512f                             [disabled]
  -mavx512ifma                          [disabled]
  -mavx512pf                            [disabled]
  -mavx512vbmi                          [disabled]
  -mavx512vbmi2                         [disabled]
  -mavx512vl                            [disabled]
  -mavx512vnni                          [disabled]
  -mavx512vpopcntdq                     [disabled]
  -mbionic                              [disabled]
  -mbmi                                 [enabled]
  -mbmi2                                [enabled]
  -mbranch-cost=<0,5>                   3
  -mcall-ms2sysv-xlogues                [disabled]
  -mcet-switch                          [disabled]
  -mcld                                 [disabled]
  -mcldemote                            [disabled]
  -mclflushopt                          [disabled]
  -mclwb                                [disabled]
  -mclzero                              [disabled]
  -mcmodel=                             [default]
  -mcpu=                      
  -mcrc32                               [disabled]
  -mcx16                                [enabled]
  -mdispatch-scheduler                  [disabled]
  -mdump-tune-features                  [disabled]
  -mf16c                                [enabled]
  -mfancy-math-387                      [enabled]
  -mfentry                              [disabled]
  -mfentry-name=              
  -mfentry-section=           
  -mfma                                 [enabled]
  -mfma4                                [disabled]
  -mforce-drap                          [disabled]
  -mforce-indirect-call                 [disabled]
  -mfp-ret-in-387                       [enabled]
  -mfpmath=                             sse
  -mfsgsbase                            [enabled]
  -mfunction-return=                    keep
  -mfused-madd                
  -mfxsr                                [enabled]
  -mgeneral-regs-only                   [disabled]
  -mgfni                                [disabled]
  -mglibc                               [enabled]
  -mhard-float                          [enabled]
  -mhle                                 [enabled]
  -miamcu                               [disabled]
  -mieee-fp                             [enabled]
  -mincoming-stack-boundary=            0
  -mindirect-branch-register            [disabled]
  -mindirect-branch=                    keep
  -minline-all-stringops                [disabled]
  -minline-stringops-dynamically        [disabled]
  -minstrument-return=                  none
  -mintel-syntax              
  -mlarge-data-threshold=<number>       65536
  -mlong-double-128                     [disabled]
  -mlong-double-64                      [disabled]
  -mlong-double-80                      [enabled]
  -mlwp                                 [disabled]
  -mlzcnt                               [enabled]
  -mmanual-endbr                        [disabled]
  -mmemcpy-strategy=          
  -mmemset-strategy=          
  -mmitigate-rop                        [disabled]
  -mmmx                                 [enabled]
  -mmovbe                               [enabled]
  -mmovdir64b                           [disabled]
  -mmovdiri                             [disabled]
  -mmpx                                 [disabled]
  -mms-bitfields                        [disabled]
  -mmusl                                [disabled]
  -mmwaitx                              [disabled]
  -mno-align-stringops                  [disabled]
  -mno-default                          [disabled]
  -mno-fancy-math-387                   [disabled]
  -mno-push-args                        [disabled]
  -mno-red-zone                         [disabled]
  -mno-sse4                             [disabled]
  -mnop-mcount                          [disabled]
  -momit-leaf-frame-pointer             [disabled]
  -mpc32                                [disabled]
  -mpc64                                [disabled]
  -mpc80                                [disabled]
  -mpclmul                              [enabled]
  -mpcommit                             [disabled]
  -mpconfig                             [disabled]
  -mpku                                 [disabled]
  -mpopcnt                              [enabled]
  -mprefer-avx128             
  -mprefer-vector-width=                none
  -mpreferred-stack-boundary=           0
  -mprefetchwt1                         [disabled]
  -mprfchw                              [enabled]
  -mptwrite                             [disabled]
  -mpush-args                           [enabled]
  -mrdpid                               [disabled]
  -mrdrnd                               [enabled]
  -mrdseed                              [enabled]
  -mrecip                               [disabled]
  -mrecip=                    
  -mrecord-mcount                       [disabled]
  -mrecord-return                       [disabled]
  -mred-zone                            [enabled]
  -mregparm=                            6
  -mrtd                                 [disabled]
  -mrtm                                 [enabled]
  -msahf                                [enabled]
  -msgx                                 [disabled]
  -msha                                 [disabled]
  -mshstk                               [disabled]
  -mskip-rax-setup                      [disabled]
  -msoft-float                          [disabled]
  -msse                                 [enabled]
  -msse2                                [enabled]
  -msse2avx                             [disabled]
  -msse3                                [enabled]
  -msse4                                [enabled]
  -msse4.1                              [enabled]
  -msse4.2                              [enabled]
  -msse4a                               [disabled]
  -msse5                      
  -msseregparm                          [disabled]
  -mssse3                               [enabled]
  -mstack-arg-probe                     [disabled]
  -mstack-protector-guard-offset= 
  -mstack-protector-guard-reg= 
  -mstack-protector-guard-symbol= 
  -mstack-protector-guard=              tls
  -mstackrealign                        [disabled]
  -mstringop-strategy=                  [default]
  -mstv                                 [enabled]
  -mtbm                                 [disabled]
  -mtls-dialect=                        gnu
  -mtls-direct-seg-refs                 [enabled]
  -mtune-ctrl=                
  -mtune=                               broadwell
  -muclibc                              [disabled]
  -mvaes                                [disabled]
  -mveclibabi=                          [default]
  -mvect8-ret-in-mem                    [disabled]
  -mvpclmulqdq                          [disabled]
  -mvzeroupper                          [enabled]
  -mwaitpkg                             [disabled]
  -mwbnoinvd                            [disabled]
  -mx32                                 [disabled]
  -mxop                                 [disabled]
  -mxsave                               [enabled]
  -mxsavec                              [disabled]
  -mxsaveopt                            [enabled]
  -mxsaves                              [disabled]

  Known assembler dialects (for use with the -masm= option):
    att intel

  Known ABIs (for use with the -mabi= option):
    ms sysv

  Known code models (for use with the -mcmodel= option):
    32 kernel large medium small

  Valid arguments to -mfpmath=:
    387 387+sse 387,sse both sse sse+387 sse,387

  Known indirect branch choices (for use with the -mindirect-branch=/-mfunction-return= options):
    keep thunk thunk-extern thunk-inline

  Known choices for return instrumentation with -minstrument-return=:
    call none nop5

  Known data alignment choices (for use with the -malign-data= option):
    abi cacheline compat

  Known vectorization library ABIs (for use with the -mveclibabi= option):
    acml svml

  Known address mode (for use with the -maddress-mode= option):
    long short

  Known preferred register vector length (to use with the -mprefer-vector-width= option):
    128 256 512 none

  Known stack protector guard (for use with the -mstack-protector-guard= option):
    global tls

  Valid arguments to -mstringop-strategy=:
    byte_loop libcall loop rep_4byte rep_8byte rep_byte unrolled_loop vector_loop

  Known TLS dialects (for use with the -mtls-dialect= option):
    gnu gnu2

  Known valid arguments for -march= option:
    i386 i486 i586 pentium lakemont pentium-mmx winchip-c6 winchip2 c3 samuel-2 c3-2 nehemiah c7 esther i686 pentiumpro pentium2 pentium3 pentium3m pentium-m pentium4 pentium4m prescott nocona core2 nehalem corei7 westmere sandybridge corei7-avx ivybridge core-avx-i haswell core-avx2 broadwell skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake tigerlake bonnell atom silvermont slm goldmont goldmont-plus tremont knl knm intel geode k6 k6-2 k6-3 athlon athlon-tbird athlon-4 athlon-xp athlon-mp x86-64 eden-x2 nano nano-1000 nano-2000 nano-3000 nano-x2 eden-x4 nano-x4 k8 k8-sse3 opteron opteron-sse3 athlon64 athlon64-sse3 athlon-fx amdfam10 barcelona bdver1 bdver2 bdver3 bdver4 znver1 znver2 btver1 btver2 generic native

  Known valid arguments for -mtune= option:
    generic i386 i486 pentium lakemont pentiumpro pentium4 nocona core2 nehalem sandybridge haswell bonnell silvermont goldmont goldmont-plus tremont knl knm skylake skylake-avx512 cannonlake icelake-client icelake-server cascadelake tigerlake intel geode k6 athlon k8 amdfam10 bdver1 bdver2 bdver3 bdver4 btver1 btver2 znver1 znver2

```





```shell
/content/drive/MyDrive/mySIMD# g++ -o build_vec_avx2 -mavx2 build_vec.cc
/content/drive/MyDrive/mySIMD# ./build_vec_avx2 
v0: [ 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ]
v1: [ 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ]
v0 = v0 + 1: [ 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 ]
v1 = v1 + 2: [ 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 ]
v0 = v0 + v1: [ 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 ]
v1 = v0 * v1: [ 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 ]

v16si a = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15};
v16si b = {0, 2, 4, 6, 8, 1, 3, 5, 7, 9, 10, 11, 12, 13, 14, 15};
c = a > b: [ 0 0 0 0 0 -1 -1 -1 -1 0 0 0 0 0 0 0 ]
d = (a > b) ? v0 : v1: [ 6 6 6 6 6 3 3 3 3 6 6 6 6 6 6 6 ]
/content/drive/MyDrive/mySIMD# g++ -o build_vec_avx512f -mavx512f build_vec.cc
/content/drive/MyDrive/mySIMD# ./build_vec_avx512f 
v0: [ 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ]
v1: [ 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 ]
Illegal instruction (core dumped)
/content/drive/MyDrive/mySIMD# 

Other options: -mmmx、-msse、-msse2、-msse3、-mssse3、-msse4.1、-msse4.2、-msse4、-mavx、-mavx2、-mavx512f、-mavx512pf、-mavx512er、-mavx512cd、-mavx512vl、-mavx512bw、-mavx512dq、-mavx512ifma、-mavx512vbmi。
```





[Intel Manual](https://www.intel.com/content/www/us/en/docs/cpp-compiler/developer-guide-reference/2021-8/intrinsics.html)





![img](https://p.ipic.vip/nblx24.jpg)

## Hardware Aspects

![image-20230524114039763](https://p.ipic.vip/98sfm8.png)

![image-20230524114127909](https://p.ipic.vip/0ba5i2.png)



![image-20230524114230862](https://p.ipic.vip/4i3ul1.png)





> •性能提升更多依赖体系结构改进
> •提高流水线效率
> 提高Cache效率
> 向量指令，提高向量长度
> 引入多核
> 以Intel为例
> MMx 11993
> MMX (64-bit)
> wl vector re
> supports
> SSE、 SSE2、 (128-bit)
> indeger ope
> •
> AVX. AVx2 (256-bit)
> AVX-512 (512-bit)
> ARM Neon指令集
> 当前ARM V8的向量化指令集
> 128-bit向量宽度
> ARM SVE指令集
> • ARM下-代向量化指令集
> 向量长度可伸缩，从128-bit 到2048-bit
> 与RISC-V V扩展类似
> RISC-V ISA指令集架构，
> 精简的基础指令集，RV321 , RV641
> •扩展指令集
> • M,A,F,D,QC
> 讨论状态：V，B，J等等
> RISC-V "V" Vector Extension
> •当前版本1.0
> https://github.com/riscv/riscv-V-spec



## Software Aspects

![image-20230524114212963](https://p.ipic.vip/nd1dy4.png)

VS





## AVX512 VS GPU

|              |                                    |                             |
| ------------ | ---------------------------------- | --------------------------- |
| CPU + AVX512 | big RAM, low Latency, Cache strong | High Power, Bandwidth low.. |
| GPU          | Scaling! CUDA                      |                             |

- Intel want "generalization" + "super high computing power"... many other people instead seek solution in ASIC or more core HW





## AVX & SSE

https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html#text=_mm256_loadu_pd&ig_expand=4122

in 2007, AMD announced SSE5 (previously SSE1~SSE4 all announced by Intel), so in 2008, Intel announced AVX, which owns some more distinct features

1 Float length doubled

2 Support 3OPs instruction for old version SSEs





![image-20230606224503357](https://p.ipic.vip/qomm14.png)

![image-20230606224622809](https://p.ipic.vip/n6fj6m.png)

> 誰是最有效率就是軟件遊戲沒有動力學優化軟體覺得我花費大量的人力物理優化提升30%的性能還不如直接等1年後的我根本就沒有用畫的東西很多錢可能會想和背景新市區有直接等過兩天是一個新的提醒我就可能是目前也不好多錢目前來看他用你
