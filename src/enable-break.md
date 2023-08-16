# 使能断点

## 技巧

在 gdb 下通过"`disable|enable breakpoints`"可以使能|去使能我们前面设置过的断点，其中 Num 为通过 "`info breakpoints`"看到的断点编号

```shell
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x000055555555515d in func at test1.c:8
2       breakpoint     keep y   0x000055555555517c in main at test1.c:12
        breakpoint already hit 1 time
(gdb) disable breakpoints 1
(gdb) disable breakpoints 2
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
1       breakpoint     keep n   0x000055555555515d in func at test1.c:8
2       breakpoint     keep n   0x000055555555517c in main at test1.c:12
        breakpoint already hit 1 time
(gdb) enable breakpoints 2
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
1       breakpoint     keep n   0x000055555555515d in func at test1.c:8
2       breakpoint     keep y   0x000055555555517c in main at test1.c:12
        breakpoint already hit 1 time
```

可以看到上述`info b`结果中的 Enb 即表示当前断点是否被使能，不指定 Num 时即可使能和去使能所有断点