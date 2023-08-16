# 命令中的条件循环

## 例子

```c
#include "stdio.h"

int main()
{
  int array[201];
  int i;

  for (i = 0; i < 201; i++)
    array[i] = i;

  return 0;
}
```

## if/else/end

在 gdb 中命令支持条件执行，基本功能与 c 语言一致。if 后带一个参数，可以是任意的合法的 c 表达式，以 end 作为条件结束。以上述程序为例：

```shell
(gdb) b 9
Breakpoint 4 at 0x55555555515f: file test1.c, line 9.
(gdb) commands
Type commands for breakpoint(s) 4, one per line.
End with a line saying just "end".
>if i % 2 == 1
 >print "odd"
 >else
 >print "even"
 >end
>end
(gdb) c
Continuing.

Breakpoint 4, main () at test1.c:9
9           array[i] = i;
$60305 = "even"
(gdb)
Continuing.

Breakpoint 4, main () at test1.c:9
9           array[i] = i;
$60306 = "odd"
```

## while/end

在 gdb 中命令支持循环执行，基本功能与 c 语言一致。whlie 条件为真时，循环执行循环体中的所有命令，以 end 作为条件结束。loop_break 和 loop_continue 在循环体中使用，同 c 语言中的 break 和 continue。以上述程序为例：

```shell
(gdb) define check
Redefine command "check"? (y or n) y
Type commands for definition of "check".
End with a line saying just "end".
>set $j = 0
>while $j < 200
 >if array[$j]== 100
  >p $j
  >loop_break
  >end
 >set $j=$j+1
 >end
>end
(gdb) check
$1 = 100
```



 