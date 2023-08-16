# 是否进入带调试信息的函数

## 例子

	#include <stdio.h>
	
	int func(void)
	{
		return 3;
	}
	
	int main(void)
	{
		int a = 0;
		
		a = func();
		printf("%d\n", a);
		return 0;
	}



## 技巧

使用gdb调试遇到函数时，使用step命令（缩写为s）可以进入函数（函数必须有调试信息）。以上面代码为例：

	(gdb) n
	12              a = func();
	(gdb) s
	func () at a.c:5
	5               return 3;
	(gdb) n
	6       }
	(gdb)
	main () at a.c:13
	13              printf("%d\n", a);


可以看到gdb进入了func函数。

可以使用next命令（缩写为n）不进入函数，gdb会等函数执行完，再显示下一行要执行的程序代码：

	(gdb) n
	12              a = func();
	(gdb) n
	13              printf("%d\n", a);
	(gdb) n
	3
	14              return 0;



可以看到gdb没有进入func函数。



可以使用stepi命令（缩写为si）进入函数，gdb执行完一条汇编指令后停下来：

```
(gdb) disass main
Dump of assembler code for function main:
   0x0000555555555144 <+0>:     push   %rbp
   0x0000555555555145 <+1>:     mov    %rsp,%rbp
   0x0000555555555148 <+4>:     sub    $0x10,%rsp
   0x000055555555514c <+8>:     movl   $0x0,-0x4(%rbp)
=> 0x0000555555555153 <+15>:    call   0x555555555139 <func>
   0x0000555555555158 <+20>:    mov    %eax,-0x4(%rbp)
   0x000055555555515b <+23>:    mov    -0x4(%rbp),%eax
   0x000055555555515e <+26>:    mov    %eax,%esi
   0x0000555555555160 <+28>:    lea    0xe9d(%rip),%rax        # 0x555555556004
   0x0000555555555167 <+35>:    mov    %rax,%rdi
   0x000055555555516a <+38>:    mov    $0x0,%eax
   0x000055555555516f <+43>:    call   0x555555555030 <printf@plt>
   0x0000555555555174 <+48>:    mov    $0x0,%eax
   0x0000555555555179 <+53>:    leave
   0x000055555555517a <+54>:    ret
End of assembler dump.
(gdb) si
func () at test1.c:4
4       {
(gdb) disass func
Dump of assembler code for function func:
=> 0x0000555555555139 <+0>:     push   %rbp
   0x000055555555513a <+1>:     mov    %rsp,%rbp
   0x000055555555513d <+4>:     mov    $0x3,%eax
   0x0000555555555142 <+9>:     pop    %rbp
   0x0000555555555143 <+10>:    ret
End of assembler dump.
```

可以看到 gdb 进入了 func 并且每次仅执行了一条指令。



可以使用nexti命令（缩写为ni）不进入函数，gdb执行完一条汇编指令后停下来：

```
(gdb) disass main
Dump of assembler code for function main:
   0x0000555555555144 <+0>:     push   %rbp
   0x0000555555555145 <+1>:     mov    %rsp,%rbp
   0x0000555555555148 <+4>:     sub    $0x10,%rsp
   0x000055555555514c <+8>:     movl   $0x0,-0x4(%rbp)
=> 0x0000555555555153 <+15>:    call   0x555555555139 <func>
   0x0000555555555158 <+20>:    mov    %eax,-0x4(%rbp)
   0x000055555555515b <+23>:    mov    -0x4(%rbp),%eax
   0x000055555555515e <+26>:    mov    %eax,%esi
   0x0000555555555160 <+28>:    lea    0xe9d(%rip),%rax        # 0x555555556004
   0x0000555555555167 <+35>:    mov    %rax,%rdi
   0x000055555555516a <+38>:    mov    $0x0,%eax
   0x000055555555516f <+43>:    call   0x555555555030 <printf@plt>
   0x0000555555555174 <+48>:    mov    $0x0,%eax
   0x0000555555555179 <+53>:    leave
   0x000055555555517a <+54>:    ret
End of assembler dump.
(gdb) ni
0x0000555555555158      12              a = func();
(gdb) disass main
Dump of assembler code for function main:
   0x0000555555555144 <+0>:     push   %rbp
   0x0000555555555145 <+1>:     mov    %rsp,%rbp
   0x0000555555555148 <+4>:     sub    $0x10,%rsp
   0x000055555555514c <+8>:     movl   $0x0,-0x4(%rbp)
   0x0000555555555153 <+15>:    call   0x555555555139 <func>
=> 0x0000555555555158 <+20>:    mov    %eax,-0x4(%rbp)
   0x000055555555515b <+23>:    mov    -0x4(%rbp),%eax
   0x000055555555515e <+26>:    mov    %eax,%esi
   0x0000555555555160 <+28>:    lea    0xe9d(%rip),%rax        # 0x555555556004
   0x0000555555555167 <+35>:    mov    %rax,%rdi
   0x000055555555516a <+38>:    mov    $0x0,%eax
   0x000055555555516f <+43>:    call   0x555555555030 <printf@plt>
   0x0000555555555174 <+48>:    mov    $0x0,%eax
   0x0000555555555179 <+53>:    leave
   0x000055555555517a <+54>:    ret
End of assembler dump.
```

可以看到 gdb 没有进入func，且每次仅执行了一条指令。



详情参见[gdb手册](https://sourceware.org/gdb/onlinedocs/gdb/Continuing-and-Stepping.html)

## 贡献者

nanxiao



