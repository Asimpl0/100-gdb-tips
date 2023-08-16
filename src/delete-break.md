# 删除断点

## 技巧

在 gdb 下通过"`delete breakpoints Num`"可以删除我们前面设置过的断点，其中 Num 为通过"`info breakpoints`"看到的断点编号

```
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
3       breakpoint     keep n   <PENDING>          test1.c:21
6       breakpoint     keep y   0x000055555555513d in func at test1.c:7
7       breakpoint     keep y   0x00007ffff7dc6250 <printf>
8       hw watchpoint  keep y                      global
9       catchpoint     keep y                      signal "<standard signals>"
(gdb) delete breakpoints
Delete all breakpoints? (y or n) n
(gdb) delete breakpoints 7
(gdb) info b
Num     Type           Disp Enb Address            What
3       breakpoint     keep n   <PENDING>          test1.c:21
6       breakpoint     keep y   0x000055555555513d in func at test1.c:7
8       hw watchpoint  keep y                      global
9       catchpoint     keep y                      signal "<standard signals>"
```

可以看到当我们不指定 Num 的时候会删除所有断点，指定 Num 即可删除对应的断点。