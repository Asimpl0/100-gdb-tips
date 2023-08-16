# 显示所有自定义命令

在 gdb 下通过"`show user`"可以打印所有用户自定义的命令：

```
(gdb) show user
User command "example":
  print "123"
  dont-repeat

User command "show_string":
  x/10s $arg0
  dont-repeat

User command "sums":
  print $arg0 +$arg1 + $arg2
```



