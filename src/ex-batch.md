# 不进入gdb直接执行命令

## 例子

```c
#include <stdio.h>
int global_var;

void change_var(){
    global_var=100;
}

int main(void){
    change_var();
    return 0;
}
```

## 技巧

使用 gdb 时，可以通过"`gdb [file] -ex [commands] --batch`"或者"`gdb [file] -batch -ex [commands] `"命令来不进入 gdb 直接执行相应命令并打印结果，以上述程序为例：

```shell
/h/test # ❯❯❯ gdb test1 -ex "disass main" -ex "disass change_var" --batch
Dump of assembler code for function main:
   0x000000000000112a <+0>:     push   %rbp
   0x000000000000112b <+1>:     mov    %rsp,%rbp
   0x000000000000112e <+4>:     mov    $0x0,%eax
   0x0000000000001133 <+9>:     call   0x1119 <change_var>
   0x0000000000001138 <+14>:    mov    $0x0,%eax
   0x000000000000113d <+19>:    pop    %rbp
   0x000000000000113e <+20>:    ret
End of assembler dump.
Dump of assembler code for function change_var:
   0x0000000000001119 <+0>:     push   %rbp
   0x000000000000111a <+1>:     mov    %rsp,%rbp
   0x000000000000111d <+4>:     movl   $0x64,0x2eed(%rip)        # 0x4014 <global_var>
   0x0000000000001127 <+14>:    nop
   0x0000000000001128 <+15>:    pop    %rbp
   0x0000000000001129 <+16>:    ret
End of assembler dump.

/h/test # ❯❯❯ gdb test1 -batch -ex "disass main" -ex "disass change_var"                                          
Dump of assembler code for function main:
   0x000000000000112a <+0>:     push   %rbp
   0x000000000000112b <+1>:     mov    %rsp,%rbp
   0x000000000000112e <+4>:     mov    $0x0,%eax
   0x0000000000001133 <+9>:     call   0x1119 <change_var>
   0x0000000000001138 <+14>:    mov    $0x0,%eax
   0x000000000000113d <+19>:    pop    %rbp
   0x000000000000113e <+20>:    ret
End of assembler dump.
Dump of assembler code for function change_var:
   0x0000000000001119 <+0>:     push   %rbp
   0x000000000000111a <+1>:     mov    %rsp,%rbp
   0x000000000000111d <+4>:     movl   $0x64,0x2eed(%rip)        # 0x4014 <global_var>
   0x0000000000001127 <+14>:    nop
   0x0000000000001128 <+15>:    pop    %rbp
   0x0000000000001129 <+16>:    ret
End of assembler dump.
```

上述命令在处理 core 文件时，非常有用，可以避免反复加载 core 文件。