## declare

`declare`命令可以声明一些特殊类型的变量，为变量设置一些限制，比如声明只读类型的变量和整数类型的变量。

它的语法形式如下。

```BASH
declare OPTION VARIABLE=value
```

`declare`命令的主要参数（OPTION）如下。

- `-a`：声明数组变量。
- `-f`：输出所有函数定义。
- `-F`：输出所有函数名。
- `-i`：声明整数变量。
- `-l`：声明变量为小写字母。
- `-p`：查看变量信息。
- `-r`：声明只读变量。
- `-u`：声明变量为大写字母。
- `-x`：该变量输出为环境变量。

`declare`命令如果用在函数中，声明的变量只在函数内部有效，等同于`local`命令。

## `-i` 参数

`-i`参数声明整数变量以后，可以直接进行数学运算

```bash
#!/bin/bash
declare -i res
a=2
b=10
res=a*b
printf $res
```

如果变量`result`不声明为整数，`val1*val2`会被当作字面量，不会进行整数运算。

注意，一个变量声明为整数以后，依然可以被改写为字符串.

## `-x` 参数

`-r`参数可以声明只读变量，无法改变变量值，也不能`unset`变量

## `-u` 参数

`-u`参数声明变量为大写字母，可以自动把变量值转成大写字母

```bash
$ declare -u foo
$ foo=upper
$ echo $foo
UPPER
```

## `-l` 参数

`-l`参数声明变量为小写字母，可以自动把变量值转成小写字母

```bash
$ declare -l bar
$ bar=LOWER
$ echo $bar
lower
```

## `-p` 参数

`-p`参数输出变量信息。

```bash
$ foo=hello
$ declare -p foo
declare -- foo="hello"
$ declare -p bar
bar：未找到
```

上面例子中，`declare -p`可以输出已定义变量的值，对于未定义的变量，会提示找不到。

如果不提供变量名，`declare -p`输出所有变量的信息。

```bash
$ declare -p
```

## `-f `参数

`-f`参数输出当前环境的所有函数，包括它的定义。

```bash
$ declare -f
```

## `-F `参数

`-F`参数输出当前环境的所有函数名，不包含函数定义。

```bash
$ declare -F
```

# readonly

`readonly`命令等同于`declare -r`，用来声明只读变量，不能改变变量值，也不能`unset`变量。

```bash
$ readonly foo=1
$ foo=2
bash: foo：只读变量
$ echo $?
1
```

上面例子中，更改只读变量`foo`会报错，命令执行失败。

`readonly`命令有三个参数。

- `-f`：声明的变量为函数名。
- `-p`：打印出所有的只读变量。
- `-a`：声明的变量为数组

# let 

`let`命令声明变量时，可以直接执行算术表达式。

```bash
$ let foo=1+2
$ echo $foo
3
```

上面例子中，`let`命令可以直接计算`1 + 2`。

`let`命令的参数表达式如果包含空格，就需要使用引号。

```bash
$ let "foo = 1 + 2"
```

`let`可以同时对多个变量赋值，赋值表达式之间使用空格分隔。

```bash
$ let "v1 = 1" "v2 = v1++"
$ echo $v1,$v2
2,1
```

上面例子中，`let`声明了两个变量`v1`和`v2`，其中`v2`等于`v1++`，表示先返回`v1`的值，然后`v1`自增。

