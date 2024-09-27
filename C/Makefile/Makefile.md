# Makefile

Makefile 是在 linux 环境下 C/C++ 程序开发的一个工程管理文件。当使用 make 命令去编译项目时,  make 会去这个项目的根目录下去寻找 Makefile 文件, 然后根据这个文件去编译程序。

> makefile 通常存在3种格式: Makefile、makefile、GNUmakefile,  make 会在当前目录自动寻找三个文件名, 如果没有找到以此命名的文件就会报错退出：
>
> `make: *** No targets specified and no makefile found.  Stop.`

## Makefile 规则

Makefile里主要包含了五个东西：显式规则、隐式规则、变量定义、指令和注释。

```makefile
target ... : prerequisites ...
    recipe
    ...
    ...
```

- 目标：可以是一个object file (目标文件) , 也可以是一个可执行文件, 还可以是一个标签 (label) 

- 目标依赖：生成该 target 所依赖的文件和/或target

- 命令：该target要执行的命令 (任意的shell命令) 命令必须使用`tab`进行缩紧不然会报错



在编译程序的过程中,  Makefile 的主要作用就是：构建生成可执行文件的依赖树。

在程序编译阶段,  make 工具会解析这个 Makefile,  根据 Makefile 里的指定,  构建出编译可执行文件所需要的完整依赖关系。

make 会根据依赖关系取编译需要的文件,  通过时间戳可以动态识别,  目标依赖(prerequisites) 中如果有一个以上的文件比 target 文件要新的话, recipe 所定义的命令就会被执行。

### 示例

```makefile
edit : main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
    cc -o edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o

main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o

```

反斜杠 ( `\` ) 是换行符的意思,  在该目录下直接输入命令 `make` 就可以生成执行文件edit,  如果要删除可执行文件和所有的中间目标文件, 那么, 只要简单地执行一下 `make clean` 就可以。`clean` 不是一个文件, 它只不过是一个动作名字, 有点像C语言中的`label`。

### 变量

makefile中以 `$(objects)` 的方式来使用这个变量,  示例中的 `objects` 可以是随机的字符串。

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)
main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit $(objects)
```

### make 自动推导

make可以自动推导文件以及文件依赖关系后面的命令, 比如 `.o` 文件, 的make会自动识别, 并自己推导命令, make看到一个 `.o` 文件, 它就会自动的把 `.c` 文件加在依赖关系中, 如果make找到一个 `whatever.o` , 那么 `whatever.c` 就会是 `whatever.o` 的依赖文件。并且 `cc -c whatever.c` 也会被推导出来。

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean
clean :
    rm edit $(objects)
```

> `.PHONY` 表示 `clean` 是个伪目标文件

### 无目标规则

规则中的三个部分不是必须存在, 规则可能无目标依赖, 仅仅是为了实现某种操作。如下面的clean命令：

```makefile
clean:
	rm -f hello.c hello.out
```

当我们使用make clean命令清理编译的文件时, 会调用这个规则中的命令, 不需要什么依赖, 仅仅是执行删除操作, 所以这个规则中并没有目标依赖。

规则中也可以没有命令, 仅含有目标和目标依赖, 仅用来描述依赖关系。但一个规则中必须要存在目标。

### 默认目标

Makefile 中默认将第一个目标作为默认目标, 当我们想生成 hello .out 的时候需要给 make 指定一个目标, 

```bash
$ make hello.out
```

### 多目标

```makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h

.PHONY : clean
clean :
    rm edit $(objects)
```

### 清空目录的规则

Makefile中都应该写一个清空目标文件 ( `.o` ) 和可执行文件的规则, 这不仅便于重编译, 也很利于保持文件的清洁。

```makefile
.PHONY : clean
clean :
    -rm edit $(objects)
```

`rm` 命令前面加了一个小减号的意思就是, 遇到出问题的文件不管继续执行后续的操作。

`clean` 的规则, 从来都是放在文件的最后。

## 包含其他Makefile

在Makefile使用 `include` 指令可以把别的Makefile包含进来, 这很像C语言的 `#include` , 被包含的文件会原模原样的放在当前文件的包含位置。 `include` 的语法是：

```
include <filenames>...

# 忽略无法读取的文件，而继续执行
-include <filenames>...

# 如果要和其它版本 make 兼容, sinclude 代替 -include 
sinclude <filenames>...
```

`<filenames>` 可以是当前操作系统Shell的文件模式(可以包含路径和通配符)。

make命令开始时, 会找寻 `include` 所指出的其它Makefile, 并把其内容安置在当前的位置, 如果当前目录下没有找到, 那么, make还会在下面的几个目录下找：

1. 有 `-I` 或 `--include-dir` 参数，那么make就会在这个参数所指定的目录下去寻找。
2. 接下来按顺序寻找目录 `<prefix>/include` (一般是 `/usr/local/bin` )、 `/usr/gnu/include` 、 `/usr/local/include` 、 `/usr/include` 。

