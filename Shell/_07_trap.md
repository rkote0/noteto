# Linux 信号和 trap 命令的使用

## 1. 信号介绍

运行Shell脚本时，如果按下快捷键`Ctrl+c`或`Ctrl+x`(x为其他字符)，程序就会终止运行，

在有些情况下，我们并不希望Shell脚本在运行时被信号中断，此时就可以使用屏蔽信号手段，让程序忽略用户输入的信号指令，从而继续运行Shell脚本程序，

简单的说，Linux的信号是由一个整数构成的异步消息，它可以由某个进程发给其他的进程，也可以在用户按下特定键发生某种异常事件时，由系统发给某个进程。

Shell 执行一个或多个命令或函数调用。可以捕获的情况包括 **`错误(ERR)、脚本退出(EXIT)、中断(SIGINT)、终止(SIGTERM) 和 kill(KILL)信号`**。

## 2. 信号列表

在Linux下和洗好相关的常见命令为`kill`和`trap`命令，执行`kill -l`或`trap -l`命令，可以列出系统支持的 中断(SIG)信号

```
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX
```

### Linux系统中重要的信号

| 信号 | 值      | 描述                                     |
| :--- | :------ | :--------------------------------------- |
| 1    | SIGHP   | 挂起进程, 通常因终端掉线或用户退出而引发 |
| 2    | SIGINT  | 终止进程, 通常因按下 Ctrl+c 组合件而引发 |
| 3    | SIGQUIT | 停止进程, 通常因按下 Ctrl+\ 组合键而引发 |
|6|SIGABRT|中止，通常因某些严重的执行错误而引发|
| 9    | SIGKILL | 无条件终止进程                           |
|14|SIGALRM|报警，通常用来处理超时|
| 15   | SIGTERM | 尽可能终止进程, 通常在系统关机时发送     |
| 17   | SIGSTOP | 无条件停止进程，但不是终止进程           |
| 18   | SIGTSTP | 停止或暂停进程，但不终止进程             |
| 19   | SIGCONT | 继续运行停止的进程                       |
|20|SIGTSTP|停止进程的运行，但该信号可以被处理和忽略，通常因按下 Ctrl+z 组合键而引发|

## 3. 控制信号

trap 命令用于指定在接受到信号后将要采取的行动，信号的相关说明前面已经提到过，trap 命令的一种常见用途是在脚本程序被终端时完成清理工作，或者屏蔽用户非法使用的某些信号，在使用信号名时需要省略 SIG 前缀，可以在命令提示符下输入命令`trap -l`来查看信号的编号及其关联的名称。

trap 命令的参数分为两部分，前一部分是接收到指定信号时要采取的行动，后一部分是要处理的信号名。

```
“信号” 常用的有以下几个。

HUP：编号1，脚本与所在的终端脱离联系。
INT：编号2，用户按下 Ctrl + C，意图让脚本终止运行。
QUIT：编号3，用户按下 Ctrl + 斜杠，意图退出脚本。
KILL：编号9，该信号用于杀死进程。
TERM：编号15，这是kill命令发出的默认信号。
EXIT：编号0，这不是系统信号，而是 Bash 脚本特有的信号，不管什么情况，只要退出脚本就会产生。
```



trap命令的使用语法如下:

```bash
trap command signal
```

signal是指接收到的信号，command是指接收到该信号应采取的行动，也就是：

```bash
trap '命令;命令' 信号编号
```

或者

```bash
trap '命令;命令' 信号名 信号名
```

### 控制单个信号

信号2 -> Ctrl+c

在 crtl + c 的疏忽输出 "<Ctrl+c> Failure."

```bash
$ trap 'echo "<Ctrl+c> Failure."' 2
```

### 控制多个信号

```bash
$ trap '' 1 2 3 20 15
或
$ trap '' SIGHP SIGINT SIGQUIT SIGTSTP SIGTERM
```

### 处理所有信号

```bash
$ trap ':' `echo {1..64}`
```

### 恢复信号

```bash
$ trap ':' 1 2 3 20 15        # 该命令即可恢复指定信号
```

### 取消 trap action设置

```bash
$ trap -p SIGINT SIGTERM
```

### trap 触发多个信号

```bash
function egress {
	command1
	commadn2
	command3
}

trap egress EXIt
```

 



参考链接：

- https://www.putorius.net/mktemp-working-with-temporary-files.html
