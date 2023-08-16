# 给断点添加命令

## 例子

```c
#include <stdio.h>

int func(int a) 
{
	return (++a);
}

int main(void)
{
	printf("%d\n", func(1));
	return 0;
}
```

## 技巧

在 gdb 中，可以通过"`commands [bnum]`"命令来给指定的断点添加命令，在该断点触发后即会自动执行该命令，bum 可以为单个断点的编号如 `5`，也可以为范围如 `5-9`，不指定 bum 时，则给最后一个设置的断点添加命令。以上述程序为例：

```shell
(gdb) disass func
Dump of assembler code for function func:
   0x0000555555555139 <+0>:     push   %rbp
   0x000055555555513a <+1>:     mov    %rsp,%rbp
   0x000055555555513d <+4>:     mov    %edi,-0x4(%rbp)   -- edi即为func的参数
   0x0000555555555140 <+7>:     addl   $0x1,-0x4(%rbp)
   0x0000555555555144 <+11>:    mov    -0x4(%rbp),%eax
   0x0000555555555147 <+14>:    pop    %rbp
   0x0000555555555148 <+15>:    ret
End of assembler dump.
(gdb) b *0x0000555555555140
Breakpoint 7 at 0x555555555140: file test1.c, line 5.
(gdb) commands
Type commands for breakpoint(s) 7, one per line.
End with a line saying just "end".
>silent     -- 触发断点后不打印信息
>p $rdi     -- 打印 func 的参数
>bt			-- 打印调用栈
>continue   -- 继续执行
>end
(gdb) c
Continuing.
$3 = 1
#0  func (a=1) at test1.c:5
#1  0x0000555555555157 in main () at test1.c:10
2
[Inferior 1 (process 11696) exited normally]
```

