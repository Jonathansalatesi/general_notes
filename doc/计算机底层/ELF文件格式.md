# ELF文件格式

Executable and Linking Format / 可执行和链接的格式

- 可执行程序  python / bash / gcc --> PE(.exe)
- 可重定位文件 / 静态库 / 目标文件 --> .o / .a / .lib
- 共享的目标文件 / 动态库文件 --> .so / .dll

----

- 代码段 .text
- 数据
- - .data / 已经初始化过的数据
  - .bss / 未初始化过的数据 - buffer 缓冲区域

----

Linux下：

```bash
readelf -e file.o
```

用来读取elf文件的各个段的信息。

```bash
objdump -d a.out (-M intel)
```

用来查看可执行文件中的二进制码和对应的反汇编后的汇编代码。默认是AT&T格式的汇编，通过加入 -M intel来将汇编的格式调整成intel的格式。

PIE - Position Independent Executable

gas 是GNU的汇编器