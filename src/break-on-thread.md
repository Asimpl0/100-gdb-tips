# 在特定线程上打断点

## 例子

```
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <unistd.h>

void *thread_func(void *p_arg)
{
        while (1)
        {
                printf("%s\n", (char*)p_arg);
                sleep(10);
        }
}
int main(void)
{
        pthread_t t1, t2;

        pthread_create(&t1, NULL, thread_func, "Thread 1");
        pthread_create(&t2, NULL, thread_func, "Thread 2");

        sleep(1000);
        return;
}
```

## 技巧

在使用 gdb 时，如果想对某个线程设置断点，可以使用"`b func thread Id`"命令给编号为 Id 的线程中的 func 函数设置断点，以上述程序为例：

```
(gdb) info threads
  Id   Target Id                                 Frame
* 1    Thread 0x7ffff7fbe680 (LWP 24060) "test1" main () at test1.c:21
  2    Thread 0x7ffff7d6d6c0 (LWP 24129) "test1" 0x00007ffff7e45c75 in clock_nanosleep () from /usr/lib/libc.so.6
  3    Thread 0x7ffff756c6c0 (LWP 24130) "test1" 0x00007ffff7e45c75 in clock_nanosleep () from /usr/lib/libc.so.6
(gdb) b sleep thread 2
Breakpoint 4 at 0x7ffff7e69260
(gdb) c
Continuing.
Thread 1
Thread 2
[Switching to Thread 0x7ffff7d6d6c0 (LWP 24129)]

Thread 2 "test1" hit Breakpoint 4, 0x00007ffff7e69260 in sleep () from /usr/lib/libc.so.6
(gdb) bt
#0  0x00007ffff7e69260 in sleep () from /usr/lib/libc.so.6
#1  0x000055555555518b in thread_func (p_arg=0x555555556004) at test1.c:11
#2  0x00007ffff7dfc9eb in ?? () from /usr/lib/libc.so.6
#3  0x00007ffff7e8123c in ?? () from /usr/lib/libc.so.6
```

可以看到提示 thread 2 触发了断点，同时 bt 显示的调用栈即为 thread 2 的调用栈。

