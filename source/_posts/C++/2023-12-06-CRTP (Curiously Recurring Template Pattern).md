---
layout: post
categories: C++
tag: []
date: 2023-12-06
Author: Jo
---



# CRTP (Curiously Recurring Template Pattern)

### My Errors

1. C 語言本身並不支持模板（templates），這是 C++ 語言的一個特性。模板是 C++ 中用於泛型編程的一種工具，
2. `include <iostream>` , instead of `include <stdio.h>`
3. `g++`, instead of `gcc`

```c++
// #include <stdio.h>   // no!
#include <iostream> 
using namespace std;
template <typename T>
T myMax(T x, T y) {
    return (x > y) ? x : y;
}

int main() {
    cout << myMax<int>(3, 7) << endl;
    cout << myMax<char>('g', 'e') << endl;
    // cout << "hello" << endl;
    return 0;
}
```





## 生成

1. **生成預處理文件 (.i)**: 這個文件包含了經過預處理的源代碼，其中包括所有包含的頭文件和展開的宏。

   ```bash
   bashCopy code
   g++ -E my_fT.cpp -o my_fT.i
   ```

   這將生成一個 `my_fT.i` 文件，其中包含預處理後的代碼。

2. **生成組合語言文件 (.s)**: 這個文件包含了轉換成組合語言的源代碼。

   ```bash
   bashCopy code
   g++ -S my_fT.cpp -o my_fT.s
   // or g++ -S my_fT.i -o my_fT.s, same
   ```

   這將生成一個 `my_fT.s` 文件，其中包含組合語言代碼。

3. **生成目標文件 (.o)**: 這是編譯過程中生成的二進制代碼，但還沒有進行最終的鏈接。

   ```bash
   bashCopy code
   g++ -c my_fT.cpp -o my_fT.o
   ```

   這將生成一個 `my_fT.o` 文件，這是一個目標文件，包含機器代碼但尚未鏈接成可執行文件。

4. **生成可執行文件**: 最後，您可以使用以下命令將目標文件鏈接成可執行文件：

   ```bash
   bashCopy code
   g++ my_fT.o -o my_fT
   ```

   這將生成 `my_fT` 可執行文件，您可以像之前一樣運行它。



## 驗證

要驗證編譯器是否為模板函數 `myMax` 生成了特定的實例（如 `int` 和 `char` 版本），您可以使用一些進階的技術。由於模板實例化通常在編譯過程中進行，並且可能不會直接反映在預處理或組合語言輸出中，您需要使用其他方法來檢查這些實例。

1. **查看符號信息**: 使用 `nm` 工具查看目標文件（`.o` 文件）中的符號信息。這可以顯示出編譯器為模板生成的所有實例。

   ```bash
   jo@fossa4gb:~/repo/exp_functionTemplate$ nm my_fT.o  | grep myMax
   0000000000000000 W _Z5myMaxIcET_S0_S0_
   0000000000000000 W _Z5myMaxIiET_S0_S0_
   ```

   在 `nm` 的輸出中，尋找 `myMax<int>` 和 `myMax<char>` 的實例。它們可能會以某種編碼（名稱修飾）的形式出現。

2. **使用 c++filt 工具**: `c++filt` 是一個工具，用於解碼（demangle）C++ 符號名稱。這對於理解 `nm` 工具輸出的名稱修飾特別有用。

   ```bash
   jo@fossa4gb:~/repo/exp_functionTemplate$ nm my_fT.o  | c++filt
                    U __cxa_atexit
                    U __dso_handle
   00000000000000d8 t _GLOBAL__sub_I_main
   0000000000000000 T main
   0000000000000078 t __static_initialization_and_destruction_0(int, int)
   0000000000000000 W char myMax<char>(char, char)
   0000000000000000 W int myMax<int>(int, int)
                    U std::basic_ostream<char, std::char_traits<char> >::operator<<(int)
                    U std::basic_ostream<char, std::char_traits<char> >::operator<<(std::basic_ostream<char, std::char_traits<char> >& (*)(std::basic_ostream<char, std::char_traits<char> >&))
                    U std::ios_base::Init::Init()
                    U std::ios_base::Init::~Init()
                    U std::cout
                    U std::basic_ostream<char, std::char_traits<char> >& std::endl<char, std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&)
   0000000000000000 r std::piecewise_construct
   0000000000000000 b std::__ioinit
                    U std::basic_ostream<char, std::char_traits<char> >& std::operator<< <std::char_traits<char> >(std::basic_ostream<char, std::char_traits<char> >&, char)
   ```

   這樣可以讓您更容易閱讀和理解 `nm` 的輸出，尤其是對於模板實例化。

3. **閱讀組合語言輸出**: 雖然直接在 `.s` 文件中找到模板實例化可能比較困難，但您可以嘗試尋找特定的函數調用和操作。這可能需要對組合語言有一定的了解。

   ```bash
   jo@fossa4gb:~/repo/exp_functionTemplate$ grep -n3 "myMax" my_fT.s
   23-     mov     x29, sp
   24-     mov     w1, 7
   25-     mov     w0, 3
   26:     bl      _Z5myMaxIiET_S0_S0_
   27-     mov     w1, w0
   28-     adrp    x0, :got:_ZSt4cout
   29-     ldr     x0, [x0, #:got_lo12:_ZSt4cout]
   --
   35-     bl      _ZNSolsEPFRSoS_E
   36-     mov     w1, 101
   37-     mov     w0, 103
   38:     bl      _Z5myMaxIcET_S0_S0_
   39-     and     w0, w0, 255
   40-     mov     w1, w0
   41-     adrp    x0, :got:_ZSt4cout
   --
   55-     .cfi_endproc
   56-.LFE1522:
   57-     .size   main, .-main
   58:     .section        .text._Z5myMaxIiET_S0_S0_,"axG",@progbits,_Z5myMaxIiET_S0_S0_,comdat
   59-     .align  2
   60:     .weak   _Z5myMaxIiET_S0_S0_
   61:     .type   _Z5myMaxIiET_S0_S0_, %function
   62:_Z5myMaxIiET_S0_S0_:
   63-.LFB1759:
   64-     .cfi_startproc
   65-     sub     sp, sp, #16
   --
   80-     ret
   81-     .cfi_endproc
   82-.LFE1759:
   83:     .size   _Z5myMaxIiET_S0_S0_, .-_Z5myMaxIiET_S0_S0_
   84:     .section        .text._Z5myMaxIcET_S0_S0_,"axG",@progbits,_Z5myMaxIcET_S0_S0_,comdat
   85-     .align  2
   86:     .weak   _Z5myMaxIcET_S0_S0_
   87:     .type   _Z5myMaxIcET_S0_S0_, %function
   88:_Z5myMaxIcET_S0_S0_:
   89-.LFB1762:
   90-     .cfi_startproc
   91-     sub     sp, sp, #16
   --
   106-    ret
   107-    .cfi_endproc
   108-.LFE1762:
   109:    .size   _Z5myMaxIcET_S0_S0_, .-_Z5myMaxIcET_S0_S0_
   110-    .text
   111-    .align  2
   112-    .type   _Z41__static_initialization_and_destruction_0ii, %function
   jo@fossa4gb:~/repo/exp_functionTemplate$ 
   ```

4. **使用 GDB 調試器**: 使用 GDB 調試器運行您的程序，並在 `myMax` 函數上設置斷點。當程序執行到這些點時，您可以檢查哪個版本的 `myMax` 被調用。

   ```bash
   bashCopy code
   g++ -g my_fT.cpp -o my_fT
   gdb ./my_fT
   ```

   然後在 GDB 中設置斷點（例如，`break myMax<int>` 和 `break myMax<char>`），並運行程序來看看是否觸發這些斷點。

這些方法可以幫助您驗證編譯器為模板函數生成的特定實例。由於模板實例化是在編譯時進行的，所以通常需要查看編譯後的代碼或使用調試工具來進行驗證。





## Ask:

- STL 的Derive containers 好像變成了 Container adaptors?! 然後，怎麼查source ?
- CRTP 中提到兩種class相關的例子差別、目的為何？

- 「快了許多」是否有量測或驗證方式?