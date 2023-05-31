---
layout: post
categories: RISC-V
tag: [] 
date: 2022-5-18
---



向量化



<img src="https://p.ipic.vip/urvre6.png" alt="image-20230523155640469" style="zoom: 67%;" />

![image-20230518175557944](https://p.ipic.vip/dkm9i4.png)  



   ![image-20230523160032501](https://p.ipic.vip/xkw1wl.png)







## risc-spec 

![image-20230523114758291](https://p.ipic.vip/89hx0k.png)

![image-20230523114829608](https://p.ipic.vip/6i7517.png)



![image-20230523115015670](https://p.ipic.vip/5tjoya.png)



![image-20230523120003689](https://p.ipic.vip/y7jbys.png)

![image-20230523152154334](https://p.ipic.vip/zfwwht.png)



```shell
jo@fossa4gb:~/repo/riscv-operating-system-mooc/code/asm/add$ kill -9 29584
jo@fossa4gb:~/repo/riscv-operating-system-mooc/code/asm/add$ make debug 
Press Ctrl-C and then input 'quit' to exit GDB and QEMU
-------------------------------------------------------
Reading symbols from test.elf...
Breakpoint 1 at 0x80000000: file test.s, line 12.
0x00001000 in ?? ()
=> 0x00001000:  97 02 00 00     auipc   t0,0x0
1: /z $x5 = 0x00000000
2: /z $x6 = 0x00000000
3: /z $x7 = 0x00000000

Breakpoint 1, _start () at test.s:12
12              li x6, 1                # x6 = 1
=> 0x80000000 <_start+0>:       13 03 10 00     li      t1,1
1: /z $x5 = 0x80000000
2: /z $x6 = 0x00000000
3: /z $x7 = 0x00000000
(gdb) si
13              li x7, 2                # x7 = 2
=> 0x80000004 <_start+4>:       93 03 20 00     li      t2,2
1: /z $x5 = 0x80000000
2: /z $x6 = 0x00000001
3: /z $x7 = 0x00000000
(gdb) si 
14              add x5, x6, x7          # x5 = x6 + x7
=> 0x80000008 <_start+8>:       b3 02 73 00     add     t0,t1,t2
1: /z $x5 = 0x80000000
2: /z $x6 = 0x00000001
3: /z $x7 = 0x00000002
(gdb) si 
stop () at test.s:17
17              j stop                  # Infinite loop to stop execution
=> 0x8000000c <stop+0>: 6f 00 00 00     j       0x8000000c <stop>
1: /z $x5 = 0x00000003
2: /z $x6 = 0x00000001
3: /z $x7 = 0x00000002
(gdb) si 
17              j stop                  # Infinite loop to stop execution
=> 0x8000000c <stop+0>: 6f 00 00 00     j       0x8000000c <stop>
1: /z $x5 = 0x00000003
2: /z $x6 = 0x00000001
3: /z $x7 = 0x00000002
(gdb) si 
17              j stop                  # Infinite loop to stop execution
=> 0x8000000c <stop+0>: 6f 00 00 00     j       0x8000000c <stop>
1: /z $x5 = 0x00000003
2: /z $x6 = 0x00000001
3: /z $x7 = 0x00000002
(gdb) qemu-system-riscv32: terminating on signal 2
Quit
(gdb) quit
```

