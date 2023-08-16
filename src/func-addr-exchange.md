# 函数变量符号和地址转换

## 例子

```
#include <stdio.h>

int global = 1;

int func(void) 
{
	return (++global);
}

int main(void)
{
	printf("%d\n", global);
	return 0;
}
```

## 技巧

在 gdb 中可以通过"`info symbol [addr]`"将 addr 对应的函数或者变量的符号信息打印出来。以上述程序为例：

```
(gdb) info symbol 0x555555555139
func in section .text of /home/test/test1
(gdb) info symbol 0x555555558018
global in section .data of /home/test/test1
```

在 gdb 中可以通过"`info address [sym]`"将 sym 对应的地址信息打印出来。以上述程序为例：

```
(gdb) info address func
Symbol "func" is a function at address 0x555555555139.
(gdb) info address global
Symbol "global" is static storage at address 0x555555558018.
```

