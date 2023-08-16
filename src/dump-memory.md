# 将指定内存保存到文件

## 例子

```c
#include "stdio.h"

int main ()  
{  
        char s1[] = "abcdefghijklmnopqrstuvwxyz";  
        char s2[] = "0123456789";  
  
  return 0;  
} 
```

## 技巧一

在 gdb 中，我们可以通过"`dump [format] memory [file] [start] [stop]`"来将 start 到 stop 地址范围内的内存中的数据写到文件，文件类型由 format 指定，具体包含以下几种：

* binary    原始二进制文件格式
* ihex        intel 16进制格式
* srec        S-recored格式
* tekhex    tektronix 16进制格式

以上述程序为例，我们分别将机器指令和字符串按二进制 dump 到指定文件中：

```shell
(gdb) dump binary memory ./code main main+51
(gdb) dump binary memory ./str s1 s1+5
(gdb) shell cat ./str
abcde
```

在 gdb 中，我们可以通过"`dump [format] value [file] [exp]`"来将 exp 对应的内容写到文件，文件类型由 format 指定，以上述程序为例：

```shell
(gdb) dump binary value ./str s1
(gdb) shell cat ./str
abcdefghijklmnopqrstuvwxyz
```

## 技巧二

在 gdb 中，我们可以通过"`append [format] memory [file] [start] [stop]`"来将 start 到 stop 地址范围内的内存中的数据追加到文件中，文件类型由 format 指定，以上述程序为例：

```shell
(gdb) append binary memory ./code main+51 main+118
(gdb) append binary memory ./str s1+6 s1+10
(gdb) shell cat ./str
abcdeghij
```

在 gdb 中，我们可以通过"`append [format] value [file] [exp] `"来将 exp 对应的内容追加到文件中，文件类型由 format 指定，以上述程序为例：

```shell
(gdb) append binary value ./str s2
(gdb) shell cat ./str
abcdefghijklmnopqrstuvwxyz0123456789
```

## 技巧三

在 gdb 中，我们可以通过"`restore [file] [format] [offset] [begin] [end]`"将指定文件 file 中的 begin 到 end 中的内容加载到当前进程 offset 出，除了 file 其余参数均为可选，offset 不指定时为0。不指定 format 时，gdb 将自行识别文件格式。以上述程序为例：

```shell
(gdb) restore  ./str binary s1+10 2 7
Restoring binary file ./str into memory (0x7fffffffdfbc to 0x7fffffffdfc1)
(gdb) p s1
$4 = "abcdefghijklcdeghrstuvwxyz"
```



