---
layout: post
categories: RISC-V
tag: [] 
date: 2022-5-17
---



ISA -- Instruction Set Architecture

- CISC
  - 在處理器中去特別設計電路去處理這指令
- RISC
  - 指令相對少，每個指令很簡易



32位元是取決於cpu中的<u>暫存器register的位元寬度</u>，如果是32位元的話，那尋址範位就是在2^32 == 4GB

所以當電腦是32位的時候，即使主機板插超過4GB的RAM，OS也沒法讀到，就是因為處理器裡的暫存器沒辦法定址到這麼高位元的資料



注意一個問題：ISA的寬度和指令編碼長度無關，像x86指令長度不固定，但是在Risc-V不管是32還是64位元的版本，編譯後，其指令的長度「大致上」是一致的，例外是像RV32C這種特規的指令



知名ISA:

- X86
- SPARC
- Power
- ARM 
- MIPS 
  - 因商業化考量加上發展不當，一直被交易，現在沒落，
  - 現在都會基於Risc-V作開發，所以接下來的開源指令集架構都會以Rics-V為主
- Risc-V





![image-20230515224234562](https://p.ipic.vip/ao2elh.png)

安裝啦！[Ubuntu 20.04.5 LTS (Focal Fossa)](https://cdimage.ubuntu.com/releases/20.04/release/)



ref: 

- [Gitee, riscv-operating-system-mooc: 开放课程《循序渐进，学习开发一个 RISC-V 上的操作系统》配套教材代码仓库。 mirror to https://github.com/plctlab/riscv-operating-system-mooc (gitee.com)](https://gitee.com/unicornx/riscv-operating-system-mooc#/unicornx/riscv-operating-system-mooc/blob/main/howto-run-with-ubuntu1804_zh.md)

- [resource on Tencent Cloud](https://www.weiyun.com/disk/folder/37e60709a663d833fd17f0dc114bd201)



---

- ADDR 4 個 bits, 代表CPU只能訪問16個bytes
  - ![image-20230518110010231](https://p.ipic.vip/vmv5ai.png)
- ![image-20230518110752766](https://p.ipic.vip/qfcrqv.png)
  - Von Neumann -- 指令和數據放在同個地方的
    - 例子：黃色是指令，綠色是數據，搭配下張圖一起解讀！![image-20230518111436451](https://p.ipic.vip/mrc3v9.png)
    - ![image-20230518111403461](https://p.ipic.vip/zhwup0.png)
    - 再個例子
      ![image-20230518111722403](https://p.ipic.vip/mgtont.png)



### 機器語言到匯編語言到高級語言

![image-20230518112444353](https://p.ipic.vip/p4cjb8.png)



### OS★

![image-20230518113118013](https://p.ipic.vip/2bzyui.png)





# ISA

## ISA (Instruction Set Architecture)

<img src="https://p.ipic.vip/aqj5iq.png" alt="image-20230518114052278" style="zoom: 25%;" />

- 是底層硬件向上層軟件程序提供的「接口規範」，一種「標準」
- 主要是跟CPU打交道時操作它的registers
- WORD -- 32 bits
- RAM少，所以需要用少點bytes，所以RISC；早期的中文字也是，一個字代表很多義，因為紙、羊皮貴
- 現今intel也往RISC變化中



★ 暫存器的大小，決定了ISA指令集的`寬度`，也決定了這款CPU的處理能力
![image-20230518115502590](https://p.ipic.vip/osvdlx.png)

- 8051指的就是每個register只有8bits這麼寬，每次只能讀8bits的數據到裡面去
- 後來就是16位，就是8086 x86，MSP是TI的概念機



★ 另， 指令集的寬度和每條指令的長度是兩回事；cpu32 or 64位裡，不管，RISC-V的每條指令都是32bits



### 知名ISA們

如開頭



## RISC-V ISA

<img src="https://p.ipic.vip/icq88m.png" alt="image-20230518152728511" style="zoom:50%;" />



- 沒有歷史包俯



![image-20230518153202510](https://p.ipic.vip/y1g6mf.png)

- E: embedded
  - register變少



![image-20230518153708407](https://p.ipic.vip/eaib58.png)



### HART

- HARdware Thread 
- 每個流(thread)去取址、解碼、執行
- Intel號稱一個核可跑兩個流
- 抽象資源可以獨立去取址到執行
- 想成是本來以為的「CPU」，現在就是個「獨立運行的”指令流”」
- 不等於是「CPU」

![image-20230518154104259](https://p.ipic.vip/qfao4j.png)



### 特權級別

目的：保護register

有三種態：用戶態(去寫高級別的會報異常)、supervisor、Machine

![image-20230518154822022](https://p.ipic.vip/l4vqos.png)



### RAM保護及管理

- 如果要啟動虛擬內存(由特殊hardware--MMU實現) 保護，是需要CPU支援Supervisor這級別的，這對CPU設計的要求高一些
  ![image-20230518155224862](https://p.ipic.vip/f7578l.png)



### Exception & Interrupt

![image-20230518155555184](https://p.ipic.vip/ljdser.png)

- 異常會回來本來這指令讓再執行一次
- 中斷是去到下一條



# Compile & Link

### GCC

## <img src="https://p.ipic.vip/h0c8uy.png" alt="image-20230518162443364" style="zoom: 25%;" />

<img src="https://p.ipic.vip/0nbppa.png" alt="image-20230518162432018" style="zoom:25%;" />

<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230518162457444.png" alt="image-20230518162457444" style="zoom:25%;" />



<img src="https://p.ipic.vip/odwogs.png" alt="image-20230518161015599" style="zoom: 15%;" />

### ELF, Executable Linkable Format

<img src="https://p.ipic.vip/r8e62o.png" alt="image-20230518161427574" style="zoom: 20%;" />

- Unix-like 系統上的二進文件格式標準
- 運行視圖：這個是在運行時候才會用到的
  - 目的：由於有alignment, 這可以讓不到4k的.text, .init各自就占了一個4k大小，取代的是將多個section合為一個segment，在運行時在內存裡合併在一起



★ Binutils
<img src="https://p.ipic.vip/y9srnp.png" alt="image-20230518162150835" style="zoom: 20%;" />



### Exercise

```shell
jo@fossa4gb:~/repo/temp$ readelf -h hello.o
ELF Header:
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
  Class:                             ELF64
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              REL (Relocatable file)
  Machine:                           AArch64
  Version:                           0x1
  Entry point address:               0x0
  Start of program headers:          0 (bytes into file)
  Start of section headers:          776 (bytes into file)
  Flags:                             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           0 (bytes)
  Number of program headers:         0
  Size of section headers:           64 (bytes)
  Number of section headers:         13
  Section header string table index: 12
```

```shell
jo@fossa4gb:~/repo/temp$ readelf -SW hello.o
There are 13 section headers, starting at offset 0x308:

Section Headers:
  [Nr] Name              Type            Address          Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            0000000000000000 000000 000000 00      0   0  0
  [ 1] .text             PROGBITS        0000000000000000 000040 000020 00  AX  0   0  4
  [ 2] .rela.text        RELA            0000000000000000 000240 000048 18   I 10   1  8
  [ 3] .data             PROGBITS        0000000000000000 000060 000000 00  WA  0   0  1
  [ 4] .bss              NOBITS          0000000000000000 000060 000000 00  WA  0   0  1
  [ 5] .rodata           PROGBITS        0000000000000000 000060 00000b 00   A  0   0  8
  [ 6] .comment          PROGBITS        0000000000000000 00006b 00002c 01  MS  0   0  1
  [ 7] .note.GNU-stack   PROGBITS        0000000000000000 000097 000000 00      0   0  1
  [ 8] .eh_frame         PROGBITS        0000000000000000 000098 000038 00   A  0   0  8
  [ 9] .rela.eh_frame    RELA            0000000000000000 000288 000018 18   I 10   8  8
  [10] .symtab           SYMTAB          0000000000000000 0000d0 000150 18     11  12  8
  [11] .strtab           STRTAB          0000000000000000 000220 00001b 00      0   0  1
  [12] .shstrtab         STRTAB          0000000000000000 0002a0 000061 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  p (processor specific)
```



#### 反匯編

```shell
jo@fossa4gb:~/repo/temp$ rm hello.o 
jo@fossa4gb:~/repo/temp$ gcc -g -c hello.c
jo@fossa4gb:~/repo/temp$ objdump -S hello.o 

hello.o:     file format elf64-littleaarch64


Disassembly of section .text:

0000000000000000 <main>:
#include <stdio.h>

void main()
{
   0:   a9bf7bfd        stp     x29, x30, [sp, #-16]!
   4:   910003fd        mov     x29, sp
    printf("Hi! World!");
   8:   90000000        adrp    x0, 0 <main>
   c:   91000000        add     x0, x0, #0x0
  10:   94000000        bl      0 <printf>
  14:   d503201f        nop
  18:   a8c17bfd        ldp     x29, x30, [sp], #16
  1c:   d65f03c0        ret
jo@fossa4gb:~/repo/temp$ 
```





# Dev on Embedded

![image-20230518163714090](https://p.ipic.vip/4j6fg7.png)



<img src="/Users/joe/Library/Application Support/typora-user-images/image-20230518163851710.png" alt="image-20230518163851710" style="zoom:50%;" />



### QEMU

![image-20230518164151876](https://p.ipic.vip/7fh111.png)



```shell
jo@fossa4gb:~/repo/temp$ riscv64-linux-gnu-gcc hello.c 
jo@fossa4gb:~/repo/temp$ ls -lcth 
total 52K
-rwxrwxr-x 1 jo jo 8.4K May 18 09:25 a.out
-rw-rw-r-- 1 jo jo   62 May 18 08:42 hello.c
-rw-rw-r-- 1 jo jo 5.7K May 18 08:30 hello.o
-rw-rw-r-- 1 jo jo 1.6K May 18 08:05 foo.o
-rw-rw-r-- 1 jo jo  556 May 18 08:05 foo.s
-rw-rw-r-- 1 jo jo  17K May 18 08:04 foo.i
jo@fossa4gb:~/repo/temp$ ./a.out 
bash: ./a.out: cannot execute binary file: Exec format error
jo@fossa4gb:~/repo/temp$ file a.out 
a.out: ELF 64-bit LSB shared object, UCB RISC-V, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=b66403ff821ac85c13c3f145822d54c3290b0fe4, for GNU/Linux 4.15.0, not stripped

jo@fossa4gb:~/repo/temp$ riscv64-linux-gnu-gcc -static hello.c 
jo@fossa4gb:~/repo/temp$ qemu-riscv64 ./a.out 
Hi! World!
```

