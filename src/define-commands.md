# 自定义GDB命令

在 gdb 下使用"`define [command]`"可以将 gdb 所支持的各种命令组合起来，并通过自定义的命令名在 gdb 中使用。每个自定义命令最多可以接受10个参数，例如：

```shell
(gdb) define sum   -- 定义命令sum
Type commands for definition of "sum".
End with a line saying just "end".
>print $arg0 +$arg1 + $arg2     ---分别对应参数1、2、3
>end  ---命令结束

(gdb) sums 1 2 3
$1 = 6
```

通过"`documtent [command]`"可以为自定义的命令设置提示信息，在"`help command`"时可以看到：

```shell
(gdb) document sums
Type documentation for "sums".
End with a line saying just "end".
>compute the sum of three inputs
>end
(gdb) help sums
compute the sum of three inputs
```

在 gdb 中执行完一条命令后，如果继续回车则默认继续执行上一条命令，可以通过在定义命令时使用"`dont-repeat`"禁用回车重复执行：

```shell
(gdb) define example
Type commands for definition of "example".
End with a line saying just "end".
>print "123"
>end
(gdb) example
$2 = "123"
(gdb)
$3 = "123"
(gdb) define example
Redefine command "example"? (y or n) y
Type commands for definition of "example".
End with a line saying just "end".
>print "123"
>dont-repeat
>end
(gdb) example
$4 = "123"
(gdb)
(gdb)
```



