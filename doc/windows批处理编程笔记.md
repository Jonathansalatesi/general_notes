# Windows批处理编程

## 1. 简介

1. 什么是批处理程序？

Batch file programming 是微软操作系统自带的开发语言，不需要任何额外的环境便可以执行。

2. 批处理程序可以做什么？

使用一系列内置的命令进行自动化操作，例如：匹配规则，删除文件，新建文件日志等甚至可以批量创建计算机病毒。

3. 一个基本的bat程序：

```bash
@echo off
echo hello world
pause
```

4. 命令分类
   - 内部命令：cls，ipconfig
   - 外部命令：java, python等
5. 常见的命令：
   - 清屏 cls
   - 创建新文件夹 mkdir
   - 打印当前目录内容 dir

## 2. 批处理运算操作

### 2.1 算术运算

算术运算：+ - * / %，分优先级进行运算。

#### 2.1.1 命令模式

```bash
> set /a 1+2
```

`/a`代表着算术运算的意思。

#### 2.1.2 文本模式

```bash
@echo off

set /a var = 1 + 2
echo %var%

pause
```

### 2.2 重定向运算

某一条运算指令需要提交给下一条指令或者是保存在特定的文件当中，此时就需要进行**重定向操作**。

重定向运算对应于四个运算符：**>**, **>>**, **<** ,**<<**，代表需要将数据存储到对应的对象中。至于一个箭头和两个箭头的本质区别是：一个符号会覆盖原有的内容，而两个符号的话，是不会覆盖原有的内容，是一种追加的状态，将最新的状态保存在文件当中。

```bash
> dir
	.
	..
> echo hello world
hello world

> echo hello world > a.txt
> echo hello world >> a.txt
```

倒数第二个命令的意思是将"hello world"这个字符串存储到a.txt这个文件当中，并把原来的内容清空。而最后一个命令是在原来文件的末尾追加"hello world"。

文本文件内容查看是使用的`type`。这仅仅是个查看的命令，并不能进行更改。

但，除此之外，**>**和**<**还是关系逻辑运算的运算符。

### 2.3 多命令运算

两个典型的多命令运算符：**&&**和**||**。**&&**具有短路的作用，第一个命令错误不会执行第二个命令。

```bash
> aa && ipconfig
```

由于第一条命令不存在，第二条命令并不会被执行。

```bash
> aa || ipconfig
```

第一条命令执行成功，就不会指令第二个命令，断路机制。

### 2.4 管道操作运算

只有一个运算符 **|**。典型的格式是 `A | B`，A命令的输出将作为B命令的输入进行执行。

```bash
> dir
	...
	1.txt
	2.txt
	...
	
> dir | find ".txt"
```

第二个命令是筛选出dir的结果中以.txt作为扩展名的文件。

```bash
> netstat -an
活动连接

  协议  本地地址          外部地址        状态
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:443            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:902            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:912            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:1032           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:1947           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5357           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:7680           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:11773          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:27000          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:36100          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:38100          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING
  TCP    127.0.0.1:1001         0.0.0.0:0              LISTENING
  TCP    127.0.0.1:8307         0.0.0.0:0              LISTENING
  TCP    127.0.0.1:9606         0.0.0.0:0              LISTENING
  TCP    127.0.0.1:11764        127.0.0.1:65001        ESTABLISHED
  TCP    127.0.0.1:11775        127.0.0.1:11776        ESTABLISHED
  TCP    127.0.0.1:11776        127.0.0.1:11775        ESTABLISHED
  TCP    127.0.0.1:11777        127.0.0.1:27000        ESTABLISHED
  TCP    127.0.0.1:11778        0.0.0.0:0              LISTENING
  TCP    127.0.0.1:11779        0.0.0.0:0              LISTENING
  TCP    127.0.0.1:12153        127.0.0.1:36100        TIME_WAIT
  TCP    127.0.0.1:12154        127.0.0.1:36100        TIME_WAIT
  TCP    127.0.0.1:12158        127.0.0.1:12157        TIME_WAIT
  TCP    127.0.0.1:12161        127.0.0.1:36100        ESTABLISHED
  TCP    127.0.0.1:12163        127.0.0.1:12162        TIME_WAIT
  TCP    127.0.0.1:27000        127.0.0.1:11777        ESTABLISHED
  TCP    127.0.0.1:36100        127.0.0.1:12161        ESTABLISHED
  TCP    127.0.0.1:65001        0.0.0.0:0              LISTENING
  TCP    127.0.0.1:65001        127.0.0.1:11764        ESTABLISHED
  TCP    192.168.150.1:139      0.0.0.0:0              LISTENING
  TCP    192.168.253.1:139      0.0.0.0:0              LISTENING
  TCP    [::]:135               [::]:0                 LISTENING
  TCP    [::]:443               [::]:0                 LISTENING
  TCP    [::]:445               [::]:0                 LISTENING
  TCP    [::]:1032              [::]:0                 LISTENING
  TCP    [::]:1947              [::]:0                 LISTENING
  TCP    [::]:5357              [::]:0                 LISTENING
  TCP    [::]:7680              [::]:0                 LISTENING
  TCP    [::]:11773             [::]:0                 LISTENING
  TCP    [::]:27000             [::]:0                 LISTENING
  TCP    [::]:49664             [::]:0                 LISTENING
  TCP    [::]:49665             [::]:0                 LISTENING
  TCP    [::]:49666             [::]:0                 LISTENING
  TCP    [::]:49667             [::]:0                 LISTENING
  TCP    [::]:49668             [::]:0                 LISTENING
  TCP    [::1]:8307             [::]:0                 LISTENING
  UDP    0.0.0.0:123            *:*
  UDP    0.0.0.0:1947           *:*
  UDP    0.0.0.0:3702           *:*
  UDP    0.0.0.0:3702           *:*
  UDP    0.0.0.0:5050           *:*
  UDP    0.0.0.0:5353           *:*
  UDP    0.0.0.0:5355           *:*
  UDP    0.0.0.0:56236          *:*
  UDP    0.0.0.0:57086          *:*
  UDP    0.0.0.0:60548          *:*
  UDP    0.0.0.0:64139          *:*
  UDP    127.0.0.1:1900         *:*
  UDP    127.0.0.1:10140        *:*
  UDP    127.0.0.1:51046        *:*
  UDP    127.0.0.1:54710        *:*
  UDP    127.0.0.1:55244        *:*
  UDP    192.168.150.1:137      *:*
  UDP    192.168.150.1:138      *:*
  UDP    192.168.150.1:1900     *:*
  UDP    192.168.150.1:5353     *:*
  UDP    192.168.150.1:54709    *:*
  UDP    192.168.253.1:137      *:*
  UDP    192.168.253.1:138      *:*
  UDP    192.168.253.1:1900     *:*
  UDP    192.168.253.1:5353     *:*
  UDP    192.168.253.1:54708    *:*
  UDP    [::]:123               *:*
  UDP    [::]:1947              *:*
  UDP    [::]:3702              *:*
  UDP    [::]:3702              *:*
  UDP    [::]:5353              *:*
  UDP    [::]:5355              *:*
  UDP    [::]:57087             *:*
  UDP    [::]:64140             *:*
  UDP    [::1]:1900             *:*
  UDP    [::1]:5353             *:*
  UDP    [::1]:54707            *:*
  UDP    [fe80::98a1:12f:ca:3069%13]:1900  *:*
  UDP    [fe80::98a1:12f:ca:3069%13]:54705  *:*
  UDP    [fe80::c108:65df:ba49:a1f9%16]:1900  *:*
  UDP    [fe80::c108:65df:ba49:a1f9%16]:54706  *:*
> netstat -an | find "ESTABLISHED"
  TCP    127.0.0.1:11764        127.0.0.1:65001        ESTABLISHED
  TCP    127.0.0.1:11775        127.0.0.1:11776        ESTABLISHED
  TCP    127.0.0.1:11776        127.0.0.1:11775        ESTABLISHED
  TCP    127.0.0.1:11777        127.0.0.1:27000        ESTABLISHED
  TCP    127.0.0.1:12169        127.0.0.1:36100        ESTABLISHED
  TCP    127.0.0.1:27000        127.0.0.1:11777        ESTABLISHED
  TCP    127.0.0.1:36100        127.0.0.1:12169        ESTABLISHED
  TCP    127.0.0.1:65001        127.0.0.1:11764        ESTABLISHED
```

## 3. 内部命令

### 3.1 基本命令格式

命令格式：命令 子命令 参数 操作

命令帮助信息查看 /?	/help

```bash
>net /?
此命令的语法是:

NET
    [ ACCOUNTS | COMPUTER | CONFIG | CONTINUE | FILE | GROUP | HELP |
      HELPMSG | LOCALGROUP | PAUSE | SESSION | SHARE | START |
      STATISTICS | STOP | TIME | USE | USER | VIEW ]

>net user /?
此命令的语法是:

NET USER
[username [password | *] [options]] [/DOMAIN]
         username {password | *} /ADD [options] [/DOMAIN]
         username [/DELETE] [/DOMAIN]
         username [/TIMES:{times | ALL}]
         username [/ACTIVE: {YES | NO}]
```

### 3.2 批处理文件参数传递

.bat文件接受参数使用 %num

例子1：

```bash
@echo off
net user %1 %2 /add
pause
```

例子2：

```bash
@echo off
echo %1
echo %2
pause
```

### 3.3 注释

注释的命令是`rem`，后面的字符全是不会执行的注释信息。

```bash
@echo off
rem program for add new user
echo %1
echo %2
pause
```

### 3.4 启动命令

`start`命令是启动一个新的窗口，并执行对应的程序。

```bash
> start				rem 这会打开一个新的cmd.exe

> start gcc			rem 这会在新的窗口中运行gcc
```

### 3.5 调用其他的批处理文件

```bash
@echo off

rem program for add new user
echo %1
echo %2
rem use: 1.bat user password
net user %1 %2 /add

call 2.bat %1 %2
pause
```

2.bat:

```bash
@echo off
ipconfig

pause
```

传递过程是可以传入参数的。

### 3.6 任务列表查看命令

任务列表查看命令`tasklist`，查看运行在本地或远程机器上当前运行的进程信息。

### 3.7 任务关闭命令

`taskkill`按照进程id，或者是名称进行终止任务。

常用的格式如下：

```bash
TASKKILL [/S system [/U username [/P [password]]]]
         { [/FI filter] [/PID processid | /IM imagename] } [/T] [/F]

描述:
    使用该工具按照进程 ID (PID) 或映像名称终止任务。

参数列表:
    /S    system           指定要连接的远程系统。

    /U    [domain\]user    指定应该在哪个用户上下文执行这个命令。

    /P    [password]       为提供的用户上下文指定密码。如果忽略，提示
                           输入。

    /FI   filter           应用筛选器以选择一组任务。
                           允许使用 "*"。例如，映像名称 eq acme*

    /PID  processid        指定要终止的进程的 PID。
                           使用 TaskList 取得 PID。

    /IM   imagename        指定要终止的进程的映像名称。通配符 '*'可用来
                           指定所有任务或映像名称。

    /T                     终止指定的进程和由它启用的子进程。

    /F                     指定强制终止进程。

    /?                     显示帮助消息。

筛选器:
    筛选器名      有效运算符                有效值
    -----------   ---------------           -------------------------
    STATUS        eq, ne                    RUNNING |
                                            NOT RESPONDING | UNKNOWN
    IMAGENAME     eq, ne                    映像名称
    PID           eq, ne, gt, lt, ge, le    PID 值
    SESSION       eq, ne, gt, lt, ge, le    会话编号。
    CPUTIME       eq, ne, gt, lt, ge, le    CPU 时间，格式为
                                            hh:mm:ss。
                                            hh - 时，
                                            mm - 分，ss - 秒
    MEMUSAGE      eq, ne, gt, lt, ge, le    内存使用量，单位为 KB
    USERNAME      eq, ne                    用户名，格式为 [domain\]user
    MODULES       eq, ne                    DLL 名称
    SERVICES      eq, ne                    服务名称
    WINDOWTITLE   eq, ne                    窗口标题

    说明
    ----
    1) 只有在应用筛选器的情况下，/IM 切换才能使用通配符 '*'。
    2) 远程进程总是要强行 (/F) 终止。
    3) 当指定远程机器时，不支持 "WINDOWTITLE" 和 "STATUS" 筛选器。

例如:
    TASKKILL /IM notepad.exe
    TASKKILL /PID 1230 /PID 1241 /PID 1253 /T
    TASKKILL /F /IM cmd.exe /T
    TASKKILL /F /FI "PID ge 1000" /FI "WINDOWTITLE ne untitle*"
    TASKKILL /F /FI "USERNAME eq NT AUTHORITY\SYSTEM" /IM notepad.exe
    TASKKILL /S system /U 域\用户名 /FI "用户名 ne NT*" /IM *
    TASKKILL /S system /U username /P password /FI "IMAGENAME eq note*"
```

### 3.8 文件夹结构命令

`tree`将当前所在路径中的文件夹与文件按照树形进行展开。

### 3.9 关机命令

`shutdown`关机或者是重启，同种命令不同功能。

### 3.10 批处理环境变量

`set`查看当前系统的环境变量。

```bash
C:\Users\段明铭>set /?
显示、设置或删除 cmd.exe 环境变量。

SET [variable=[string]]

  variable  指定环境变量名。
  string    指定要指派给变量的一系列字符串。

要显示当前环境变量，键入不带参数的 SET。

如果命令扩展被启用，SET 会如下改变:

可仅用一个变量激活 SET 命令，等号或值不显示所有前缀匹配
SET 命令已使用的名称的所有变量的值。例如:

    SET P

会显示所有以字母 P 打头的变量

如果在当前环境中找不到该变量名称，SET 命令将把 ERRORLEVEL
设置成 1。

SET 命令不允许变量名含有等号。

在 SET 命令中添加了两个新命令行开关:

    SET /A expression
    SET /P variable=[promptString]

/A 命令行开关指定等号右边的字符串为被评估的数字表达式。该表达式
评估器很简单并以递减的优先权顺序支持下列操作:

    ()                  - 分组
    ! ~ -               - 一元运算符
    * / %               - 算数运算符
    + -                 - 算数运算符
    << >>               - 逻辑移位
    &                   - 按位“与”
    ^                   - 按位“异”
    |                   - 按位“或”
    = *= /= %= += -=    - 赋值
      &= ^= |= <<= >>=
    ,                   - 表达式分隔符

如果你使用任何逻辑或取余操作符， 你需要将表达式字符串用
引号扩起来。在表达式中的任何非数字字符串键作为环境变量
名称，这些环境变量名称的值已在使用前转换成数字。如果指定
了一个环境变量名称，但未在当前环境中定义，那么值将被定为
零。这使你可以使用环境变量值做计算而不用键入那些 % 符号
来得到它们的值。如果 SET /A 在命令脚本外的命令行执行的，
那么它显示该表达式的最后值。该分配的操作符在分配的操作符
左边需要一个环境变量名称。除十六进制有 0x 前缀，八进制
有 0 前缀的，数字值为十进位数字。因此，0x12 与 18 和 022
相同。请注意八进制公式可能很容易搞混: 08 和 09 是无效的数字，
因为 8 和 9 不是有效的八进制位数。(& )

/P 命令行开关允许将变量数值设成用户输入的一行输入。读取输入
行之前，显示指定的 promptString。promptString 可以是空的。

环境变量替换已如下增强:

    %PATH:str1=str2%

会扩展 PATH 环境变量，用 "str2" 代替扩展结果中的每个 "str1"。
要有效地从扩展结果中删除所有的 "str1"，"str2" 可以是空的。
"str1" 可以以星号打头；在这种情况下，"str1" 会从扩展结果的
开始到 str1 剩余部分第一次出现的地方，都一直保持相配。

也可以为扩展名指定子字符串。

    %PATH:~10,5%

会扩展 PATH 环境变量，然后只使用在扩展结果中从第 11 个(偏
移量 10)字符开始的五个字符。如果没有指定长度，则采用默认
值，即变量数值的余数。如果两个数字(偏移量和长度)都是负数，
使用的数字则是环境变量数值长度加上指定的偏移量或长度。

    %PATH:~-10%

会提取 PATH 变量的最后十个字符。

    %PATH:~0,-2%

会提取 PATH 变量的所有字符，除了最后两个。

终于添加了延迟环境变量扩充的支持。
该支持总是按默认值被停用，但也可以
通过 CMD.EXE 的 /V 命令行开关而被启用/停用。请参阅 CMD /?

考虑到读取一行文本时所遇到的目前扩充的限制时，延迟环境
变量扩充是很有用的，而不是执行的时候。
以下例子
说明直接变量扩充的问题:

 set VAR=before
 if "%VAR%" == "before" (
set VAR=after
 if "%VAR%" == "after" @echo If you see this, it worked )


不会显示消息，因为在读到第一个 IF 语句时，BOTH IF 语句中的 %VAR% 会被代替；
原因是: 它包含 IF 的文体
，IF 是一个复合语句。所以，
复合语句中的 IF 实际上是在比较 "before"
和"after"，这两者永远不会相等。同样，以下这个例子
也不会达到预期效果:

 set LIST=
 for% i in (*) do set LIST=%LIST%%i
 echo%LIST%

 原因是，它不会在目前的目录中建立一个文件列表，
而只是将LIST 变量设成找到的最后一个文件。
这也是因为 %LIST% 在
FOR 语句被读取时，只被扩充了一次；而且，那时的 LIST 变量是空的。
因此，我们真正执行的 FOR 循环是:

 for% i in (*) do set LIST= %i

这个循环继续将 LIST 设成找到的最后一个文件。

延迟环境变量扩充允许你使用一个不同的
字符(惊叹号)在
执行时间扩充环境变量。如果延迟的变量扩充被启用，
可以将上面例子写成以下所示，以达到预期效果:

 set VAR=before
if "%VAR%" == "before" (
 set VAR=after
 if "!VAR!" == "after" @echo If you see this, it worked
 )

 set LIST=
 for% i in (*) do set LIST=!LIST! %i
 echo %LIST%

如果命令扩展被启用，有几个动态环境变量可以被扩展，但不会出现在 SET 显示的变
量列表中。每次变量数值被扩展时，这些变量数值都会被动态计算。如果用户用这些
名称中任何一个明确定义变量，那个定义会替代下面描述的动态定义:

%CD% - 扩展到当前目录字符串。

%DATE% - 用跟 DATE 命令同样的格式扩展到当前日期。

%TIME% - 用跟 TIME 命令同样的格式扩展到当前时间。

%RANDOM% - 扩展到 0 和 32767 之间的任意十进制数字。

%ERRORLEVEL% - 扩展到当前 ERRORLEVEL 数值。

%CMDEXTVERSION% - 扩展到当前命令处理器扩展版本号。

%CMDCMDLINE% - 扩展到调用命令处理器的原始命令行。

%HIGHESTNUMANODENUMBER% - 扩展到此计算机上的最高 NUMA 节点号。
```

### 4.1 目录浏览命令 dir

### 4.2 目录新建与删除

目录新建 mkdir

```bash
> mkdir test
```

新建test文件夹。



删除目录 rmdir

```bash
> rmdir /s test
```

删除该目录及其子目录，用于删除目录树。

### 4.3 目录切换命令 cd

```bash
> mkdir test
> cd test
> cd ..				返回上一级目录
```

### 4.4 目录重命名命令 ren

`rename`的一个缩写。

```bash
> mkdir test
> ren test test1
```

### 4.5 文件目录拷贝命令 copy

```bash
> md test1
> md test2
> cd test2
> dir
> echo 111 > 1.txt
> echo 222 > 2.txt
> cd ..
> copy test2 test1
```

将test中的所有内容复制到test1当中。

```bash
> copy * test
```

是将当前文件夹下的所有的文件复制到test文件夹下。

### 4.6 文件删除命令 del

### 4.7 剪切文件 move

```bash
> move c:/1/test.txt test
```

### 5.1 if-else结构

条件判断结构：

```bash
@echo off
rem 演示if-else结构来判断字符串是否为规定的字符串内容
set v=hello
if %v% == hello (echo ok) else (echo no)

pause
```

也可以将赋值语句写为：`set "v = hello"`





