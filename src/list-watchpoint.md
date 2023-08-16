# 查看观察点

在 gdb 下通过"`info watchpoints`"即可看到前面设置过的所有观察点信息:

```shell
(gdb) info watchpoints
Num     Type           Disp Enb Address            What
2       hw watchpoint  keep y                      global
(gdb) info breakpoints
Num     Type           Disp Enb Address            What
2       hw watchpoint  keep y                      global
```

watch 所设置的观察点也可以通过控制断点的命令来控制。如 disable、enable、delete等。

当然，通过"`info breakpoints`"同样可以看到所有的观察点。