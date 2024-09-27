# Makefile 规则

规则包含两个部分，一个是依赖关系，一个是生成目标的方法。

## 语法

```makefile
targets : prerequisites
    command
    ...
```

## 规则中使用通配符

make支持三个通配符： `*` ， `?` 和 `~` 。这是和Unix的B-Shell是相同的。

```makefile
clean:
    cat main.c
    rm -f *.o
```

列出文件夹中所有的`.c`文件

```
obj := $(wildcard *.c)
```



