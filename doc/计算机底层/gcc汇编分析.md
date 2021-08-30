# gcc汇编分析

## 伪指令

### CFI

Call frame information 调用栈帧信息

`.cfi_`这些伪指令均是用于调试信息，可以在调用失败后回调栈

A --> B --> C(异常)：可以找到B调用的C，A调用的B



`-fno-asynchronous-unwind-tables`可用于去除调试信息

```bash
gcc -m32 -S -fno-asynchronous-unwind-tables main.c
```



### PIC

position independent code / 位置无关代码

用于动态链接

​	mov eax, eip

上面这条指令是无效的。调用了`__x86.get_pc_thunk.ax`函数来获取`eip`的值。

而`printf`是存储在`_GLOBAL_OFFSET_TABLE_`中的，上面通过系统函数获取到了`printf`在`_GLOBAL_OFFSET_TABLE`的偏移。

`-fno-pic`命令可用于去除位置无关代码的信息。

```bash
gcc -m32 -S -fno-asynchronous-unwind-tables -fno-pic hello.c -o hello.s
```

### 栈对齐

`andl $-16, %esp`这一行的汇编指令是用来进行栈对齐的。其中：
$$
-16(10) = 1000 \; 0000\; 0000\; 0000\; 0000\; 0000\; 0001\; 0000\; \\
		= 1111 \; 1111\; 1111\; 1111\; 1111\; 1111\; 1110\; 1111\; \\
		= 1111 \; 1111\; 1111\; 1111\; 1111\; 1111\; 1111\; 0000\; \\
		= 0 \rm{xffff \;fff0}
$$
然后，这行代码的意义是将`esp`的最后四位变成0。将栈对齐到16位。

`-mpreferred-stack-boundary=2`这个命令行参数可用来去掉栈对齐。

```bash
gcc -m32 -S -fno-asynchronous-unwind-tables -fno-pic -mpreferred-stack-boundary=2 main.c -o main.s
```



### Intel语法

`-masm=intel`是用来生成intel语法的汇编。

```bash
gcc -m32 -S -fno-asynchronous-unwind-tables -fno-pic -mpreferred-stack-boundary=2 -masm=intel main.c -o main.s
```

```assembly
_main:
	push ebp
	mov ebp, esp
	sub esp, 4
	call ___main
	mov DWORD PTR [esp], OFFSET FLAT:LC0
	call _printf
	mov eax, 0
	leave 	; mov esp, ebp \ pop ebp
	ret
	.ident "GCC: (tdm64-1) 5.1.0"
	.def _printf;	.scl	2;	.type	32;	.endef
```

