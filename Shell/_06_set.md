# Set

如果命令行下不带任何参数，直接运行`set`，会显示所有的环境变量和 Shell 函数。

```
$ set
```

## 1. set -u

> 注：执行脚本时，如果遇到不存在的变量，Bash 默认忽略它。

`set -u`就用来改变这种行为。脚本在头部加上它，遇到不存在的变量就会报错，并停止执行。

```
#!/usr/bin/env bash
set -u

echo $a
echo bar
```

运行结果如下。

```bash
$ bash script.sh
bash: script.sh:行4: a: 未绑定的变量
```

`-u`还有另一种写法`-o nounset`，两者是等价的。

```
set -u
和
set -o nounset
```

## 2. set -x

默认情况下，脚本执行后，只输出运行结果，没有其他内容。如果多个命令连续执行，它们的运行结果就会连续输出。有时会分不清，某一段内容是什么命令产生的。

`set -x`用来在运行结果之前，先输出执行的那一行命令。

```bash
#!/usr/bin/env bash
set -x

echo bar
```

执行上面的脚本，结果如下。

```bash
$ bash script.sh
+ echo bar
bar
```

`-x`还有另一种写法`-o xtrace`。

```bash
set -o xtrace
```

> 脚本当中如果要关闭命令输出，可以使用`set +x`

## 3. set -e

**`set +e`表示关闭`-e`选项**

上面这些写法多少有些麻烦，容易疏忽。`set -e`从根本上解决了这个问题，它使得脚本只要发生错误，就终止执行。

```bash
#!/usr/bin/env bash
set -e

foo
echo bar
```

执行结果如下。

```
$ bash script.sh
script.sh:行4: foo: 未找到命令
```

`-e`还有另一种写法`-o errexit`。

```bash
set -o errexit
```

## 4. set -o pipefail

`set -e`有一个例外情况，就是不适用于管道命令。

所谓管道命令，就是多个子命令通过管道运算符（`|`）组合成为一个大的命令。Bash 会把最后一个子命令的返回值，作为整个命令的返回值。也就是说，只要最后一个子命令不失败，管道命令总是会执行成功，因此它后面命令依然会执行，`set -e`就失效了。

```bash
#!/usr/bin/env bash
set -e

foo | echo a
echo bar
```

执行结果如下。

```bash
$ bash script.sh
a
script.sh:行4: foo: 未找到命令
bar
```

上面代码中，`foo`是一个不存在的命令，但是`foo | echo a`这个管道命令会执行成功，导致后面的`echo bar`会继续执行。

`set -o pipefail`用来解决这种情况，只要一个子命令失败，整个管道命令就失败，脚本就会终止执行。

```bash
#!/usr/bin/env bash
set -eo pipefail

foo | echo a
echo bar
```

运行后，结果如下。

```bash
$ bash script.sh
a
script.sh:行4: foo: 未找到命令
```

可以看到，`echo bar`没有执行。

## 5. set -E

一旦设置了`-e`参数，会导致函数内的错误不会被`trap`命令捕获,`-E`参数可以纠正这个行为，使得函数也能继承`trap`命令。

```bash
#!/bin/bash
set -e

trap "echo ERR trap fired!" ERR

myfunc()
{
  # 'foo' 是一个不存在的命令
  foo
}

myfunc
```

上面示例中，`myfunc`函数内部调用了一个不存在的命令`foo`，导致执行这个函数会报错。

```
$ bash test.sh
test.sh:行9: foo：未找到命令
```

但是，由于设置了`set -e`，函数内部的报错并没有被`trap`命令捕获，需要加上`-E`参数才可以。

```
#!/bin/bash
set -Eeuo pipefail

trap "echo ERR trap fired!" ERR

myfunc()
{
  # 'foo' 是一个不存在的命令
  foo
}

myfunc
```

执行上面这个脚本，就可以看到`trap`命令生效了。

```
$ bash test.sh
test.sh:行9: foo：未找到命令
ERR trap fired!
```

## 6. 其他参数

`set`命令还有一些其他参数。

- `set -n`：等同于`set -o noexec`，不运行命令，只检查语法是否正确。
- `set -f`：等同于`set -o noglob`，表示不对通配符进行文件名扩展。
- `set -v`：等同于`set -o verbose`，表示打印 Shell 接收到的每一行输入。
- `set -o noclobber`：防止使用重定向运算符`>`覆盖已经存在的文件。

上面的`-f`和`-v`参数，可以分别使用`set +f`、`set +v`关闭。

## set 命令总结

上面重点介绍的`set`命令的几个参数，一般都放在一起使用。

```bash
# 写法一
set -Eeuxo pipefail

# 写法二
set -Eeux
set -o pipefail
```

# Bash 的错误处理

如果脚本里面有运行失败的命令（返回值非`0`），Bash 默认会继续执行后面的命令





参考文档：

- https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/