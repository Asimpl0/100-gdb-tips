# 打印函数调用栈

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
        func();
	printf("%d\n", global);
	return 0;
}
```

## 技巧

在 gdb 中，可以通过"`bt`"命令(`backtrace`命令的缩写)显示函数调用栈信息。以上面的程序为例：

```
(gdb) bt
#0  0x0000555555555143 in func () at test1.c:7
#1  0x000055555555515d in main () at test1.c:12
(gdb) bt
#0  0x0000555555555143 in func () at test1.c:7
#1  0x000055555555515d in main () at test1.c:12
```

