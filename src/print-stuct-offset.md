# 打印结构提大小和成员偏移

## 例子

```c
#include <stdio.h>
#include <pthread.h>

typedef struct
{
        int a;
        int b;
        int c;
        int d;
        pthread_mutex_t mutex;
}ex_st;

int main(void) {
        ex_st st = {1, 2, 3, 4, PTHREAD_MUTEX_INITIALIZER};
        printf("%d,%d,%d,%d\n", st.a, st.b, st.c, st.d);
        return 0;
}
```

## 技巧

可以通过"`p sizeof(struct tmp)`"来获得结构体 tmp 的大小，以上述代码为例：

```
(gdb) p sizeof(ex_st)
$1 = 56
```

可以通过"`p &((struct tmp *)0)->var`"来获得结构体 tmp 中成员 var 的相对偏移，以上述代码为例：

```
(gdb) p &((ex_st *)0)->b
$2 = (int *) 0x4
```

