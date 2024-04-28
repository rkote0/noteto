

# Shell

## Case

![768762155](\.assets/768762155.png)

### Case 语句语法：

Bash case 语句的语法由“ `case`”关键字后跟要匹配的值、“ `in`”关键字以及一个或多个模式以及包含在“ `;;`”语句中的相应代码块组成：

```shell
#!/bin/bash
case EXPRESSION in

  PATTERN_1)
    STATEMENTS
    ;;

  PATTERN_2 | PATTERN_3)
    STATEMENTS
    ;;

  PATTERN_N)
    STATEMENTS
    ;;

  *)
    STATEMENTS
    ;;
esac

```



## 连接字符串

```shell
VAR1="Hello,"
VAR2=" World"
VAR3="$VAR1$VAR2"
echo "$VAR3"


# Hello, World
```

## If...else

![WeChat719c50663fca735bcb7daf7dcb1ccc45](.assets/WeChat719c50663fca735bcb7daf7dcb1ccc45.jpg)

```shell
if TEST-COMMAND
then
  STATEMENTS1
else
  STATEMENTS2
fi
```





```shell
#!/bin/bash

echo -n "Enter a number: "
read VAR

if [[ $VAR -gt 10 ]]
then
  echo "The variable is greater than 10."
elif [[ $VAR -eq 10 ]]
then
  echo "The variable is equal to 10."
else
  echo "The variable is less than 10."
fi
```

```
-n 字符串长度大于0，True
-z 字符串为空， True
str1 = str2 如果 str1 和 str2 相等 True
str1 != str2 如果 str1 和 str2 不相等 True
str1 -eq str2 如果 str1 和 str2 相等 True
str1 -gt str2 如果 str1 大于 str2  True
str1 -lt str2 如果str1 小于 str2 True
str1 -ge str2 如果 str1 大于或等于 str2 则 True
str1 -le str2 如果 str1 小于或等于 str2 则 True

-h FILE- 如果FILE存在并且是符号链接，则为真。
-r FILE- 如果FILE存在并且可读则为真。
-w FILE- 如果FILE存在并且可写则为真。
-x FILE- 如果FILE存在并且可执行，则为真。
-d FILE- 如果FILE存在并且是目录则为真。
-e FILEFILE- 如果存在并且是文件，则为True ，无论类型如何（节点、目录、套接字等）。
-f FILE-如果FILE存在 并且是常规文件（不是目录或设备），则为 true。
```

