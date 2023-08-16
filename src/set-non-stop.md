# 只暂停指定部分线程

## 例子

```c
#include <stdio.h>
#include <pthread.h>
static void *thread1_job()
{
    printf("this is 1\n");
}
static void *thread2_job()
{
    printf("this is 2\n");
}
int main()
{
    pthread_t tid1,tid2;
    pthread_create(&tid1, NULL, thread1_job, NULL);
    pthread_create(&tid2, NULL, thread2_job, NULL);
    pthread_join(tid1,NULL);
    pthread_join(tid2,NULL);
    printf("this is main\n");
    return 0;
}
```

## 技巧

在 gdb 下调试多线程程序时，默认采用的是 all-stop 模式，即只要有一个线程暂停执行，所有线程都随即暂停。但在某些场景中，我们可能需要调试个别的线程，并且不想在调试过程中，影响其它线程的运行。

可以将 gdb 的调试模式由 all-stop 模式更改为 non-stop 模式，该模式下调试多线程程序，当某一线程暂停运行时，其它线程仍可以继续执行。

通过"`show non-stop`"命令，我们可以查看当前 gdb 是否使用 non-stop 模式：

```
(gdb) show non-stop
Controlling the inferior in non-stop mode is off.
```

通过"`set non-stop [mode]`"命令，我们可以开启和关闭 non-stop 模式：

```
(gdb) set non-stop on
(gdb) show non-stop
Controlling the inferior in non-stop mode is on.

(gdb) set non-stop off
(gdb) show non-stop
Controlling the inferior in non-stop mode is off.
```

在 non-stop 模式下，我们对指定线程设置断点，不会中断其他线程的执行，以上述程序为例：

```
(gdb) set non-stop on
(gdb) start
Temporary breakpoint 1, main () at test1.c:12
12      {
(gdb) b 9
Breakpoint 2 at 0x555555555183: file test1.c, line 9.
(gdb) c
Continuing.
[New Thread 0x7ffff7d6d6c0 (LWP 24104)]
this is 1
[New Thread 0x7ffff756c6c0 (LWP 24105)]
[Thread 0x7ffff7d6d6c0 (LWP 24104) exited]

Thread 3 "test1" hit Breakpoint 2, thread2_job () at test1.c:9
9           printf("this is 2\n");
(gdb) info threads
  Id   Target Id                                 Frame
* 1    Thread 0x7ffff7fbe680 (LWP 24083) "test1" (running)
  3    Thread 0x7ffff756c6c0 (LWP 24105) "test1" thread2_job () at test1.c:9
```

可以看到，对 thread 3 的断点并不影响父进程和 thread 2 的执行，其中 thread 2 执行结束并完成了打印，父进程状态为 running 且在等待 thread 3 的创建完成。在 non-stop 模式下，线程中断后 gdb 并不会切换到中断线程



在 all-stop 模式下，continue、next、step 命令的作用对象并不是当前线程，而是所有的线程；但在 non-stop 模式下，continue、next、step 命令只作用于当前线程。

在 non-stop 模式下，如果想要 continue 命令作用于所有线程，可以为 continue 命令添加一个 -a 选项，即执行 continue -a 或者 c -a 命令，即可实现令所有线程继续执行的目的。

```
(gdb) b 5
Breakpoint 2 at 0x55555555516d: file test1.c, line 5.
(gdb) b 9
Breakpoint 3 at 0x555555555183: file test1.c, line 9.
(gdb) c
Continuing.
[New Thread 0x7ffff7d6d6c0 (LWP 24904)]

Thread 2 "test1" hit Breakpoint 2, thread1_job () at test1.c:5
5           printf("this is 1\n");
(gdb) [New Thread 0x7ffff756c6c0 (LWP 24905)]

Thread 3 "test1" hit Breakpoint 3, thread2_job () at test1.c:9
9           printf("this is 2\n");

(gdb) thread 2
[Switching to thread 2 (Thread 0x7ffff7d6d6c0 (LWP 24904))]
#0  thread1_job () at test1.c:5
5           printf("this is 1\n");
(gdb) continue -a
Continuing.
this is 2
[Thread 0x7ffff756c6c0 (LWP 24905) exited]
this is 1
this is main
[Thread 0x7ffff7d6d6c0 (LWP 24904) exited]
[Inferior 1 (process 24868) exited normally]
(gdb) info threads
No threads.
```

