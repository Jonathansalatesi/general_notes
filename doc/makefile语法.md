# makefile语法

## makefile的格式

首先源目录中的makefile的名字是**makefile**，没有任何的扩展名。

通常的格式是：

```
target : dependencies
		 command

目标：目标的依赖文件
	 命令（例如编译，调试之类的）
```

例子1：

```makefile
test: test.c
	gcc test.c -o test
```

例子2：

```makefile
main: main.c tool.o
	gcc main.c tool.o -o main

tool.o: tool.c
	gcc -c tool.c

clean:
	rm *.o main
```

例子3：

```makefile
main: main.c foo.o bar.o
	gcc main.c foo.o bar.o -o main

foo.o: foo.c
	gcc -c foo.c

bar.o: bar.c
	gcc -c bar.c

clean:
	rm *.o main
```

例子4：

声明变量，利用`$()`来调用变量。

```makefile
CC = gcc
main: main.c foo.o bar.o
	$(CC) main.c foo.o bar.o -o main

foo.o: foo.c
	$(CC) -c foo.c

bar.o: bar.c
	$(CC) -c bar.c

clean:
	rm *.o main
```

例子5：

同时生成多个可执行文件。

```makefile
.PHONY: clean
CC = gcc

all: main_max main_min

main_max: main_max.c foo.o bar.o
	$(CC) main_max.c foo.o bar.o -o main_max

main_min: main_min.c foo.o bar.o
	$(CC) main_min.c foo.o bar.o -o main_min

foo.o: foo.c
	$(CC) -c foo.c

bar.o: bar.c
	$(CC) -c bar.c

clean:
	rm -rf *.o main_max main_min
```

## makefile的高级法则

```makefile
%.o: %.c
	$(CC) -c $(CFLAGS) -o $@ $<
	
%.o: %.cpp
	$(CXX) -c $(CXXFLAGS) -o $@ $<

%.o: %.f
	$(FC) -c $(FFLAGS) -o $@ $<

%.o: %.p
	$(PC) -c $(PFLAGS) -o $@ $<		# 编译p文件
```

`%.o`代表着该目录下所有的`.o`文件，`%.c`代表着所有的`.c`程序。`$@`代表着目标文件，而`$<`代表着所有的依赖程序。其他的同理。

