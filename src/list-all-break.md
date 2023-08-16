# 查看所有断点

## 技巧

在 gdb 下通过"`info breakpoints`"即可看到前面设置过的所有断点信息：

```
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
3       breakpoint     keep n   <PENDING>          test1.c:21
6       breakpoint     keep y   0x000055555555513d in func at test1.c:7
7       breakpoint     keep y   0x00007ffff7dc6250 <printf>
8       hw watchpoint  keep y                      global
9       catchpoint     keep y                      signal "<standard signals>"
```

其中 Num 为 gdb 给断点设置的编号，用于 delete disable 等命令对断点进行操作，Type 为类型，包括 breakpoint、watchpoint、catchpoint，Enb 表示当前断点是否使能(disable)，Address 即为断点对应的地址。



