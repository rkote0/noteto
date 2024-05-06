# Go

## 变量

常量的定义的几种方式:

```
1. 标准格式
var 变量名 变量类型

2. 批量格式
var ( 变量名 变量类型 )

3. 简短模式
变量名 := 表达式


注意: 简短模式有以下限制：
		- 1. 只能用来定义变量，同时会初始化
		- 2. 不能提供数据类型
		- 3. 只能在函数内部，不能定义全局变量
```



```Go
package main

import "fmt"

// 全局变量
var (
	username string
	age      int
	weight   float64
)

func main() {
  // 局部变量(函数体内定义的变量)
	var a = 1
	var b, c *int
	var d = true
	var e int
	f := "apple"

	fmt.Println(a, b, c)
	fmt.Println(!d)
	fmt.Println(e)
	fmt.Println(f)

	username = "Demo"
	age = 18
	weight = 65.0

	fmt.Println(username, age, weight)

}

// 结果
// 1 <nil> <nil>
// false
// 0
// apple
// Demo 18 65

```

## 常量

常量声明如下:

```go
// const 常量名 常量类型 = 常量值
const s string = "constant"
```

#### iota 常量生成器

```go
package main

import "fmt"

// 定义一个枚举类型表示星期几
type Weekday int

const (
	Sunday Weekday = iota // 使用 iota 生成连续的值，从 0 开始递增
	Monday
	Tuesday
	Wednesday
	Thursday
	Friday
	Saturday
)

func main() {
	// 输出枚举值
	fmt.Println(Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday)
}

// iota 用于生成一组以相似规则初始化的变量
// 输出：
// 0 1 2 3 4 5 6

```



## 流程控制语句

### 1. if...else

```go
package main

import "fmt"

func main() {
	if 7%2 == 0 {
		fmt.Println("False")
	} else {
		fmt.Println("True")
	}

	if 7%2 == 0 || 8%2 == 0 {
		fmt.Println("false")
	} else {
		fmt.Println("True")
	}

	if num := 10; num < 0 {
		fmt.Println(num, "is negative")
	} else if num < 10 {
		fmt.Println(num, "has 1 digit")
	} else {
		fmt.Println(num, "has multiple digits")
	}
}

/*
结果：
True
false
10 has multiple digits
*/
```





### 2. For

for 循环的三种格式：

```
1. 和 `C` 的 `for(;;)` 一样
for init; condition; post { }

2. 和 `C` 语言的 `for` 一样
for condition { }

3. 和 `C` 的 `while` 一样
for { }


init: 给控制变量赋值
condition: 关系表达式，循环控制条件
post: 赋值表达式，给控制变量增量或减量

执行过程: condition 判读是否符合条件，若为真则进行循环体，然后执行 post，然后继续判别 condition，知道 condition 不满足
```



```GO
package main

import "fmt"

var (
	UserNum  int
	manNum   int
	womanNum int
)

const SumNum = 10
const OldNum = 1
const childNum = 5

func main() {
	fmt.Println(UserNum, manNum, womanNum, SumNum)
	// 第一种
	for i := OldNum; i <= childNum; i++ {
		fmt.Println(i)
	}
	// 第二种
	for j := range 8 {
		fmt.Println(j)
	}

	//第三种,默认是 while true
	for {
		fmt.Println("hello word")
		break
	}
}


// 结果
// 0 0 0 10
// 1
// 2
// 3
// 4
// 5
// 0
// 1
// 2 
// 3
// 4
// 5
// 6
// 7
// hello word

```

### 3. Switch

 **一分支多值**, 一分多值 case 表达式使用逗号分隔

```go
package main

import "fmt"

var (
	user = "jake"
)

func main() {
	switch user {
	case "jake", "marry":
		fmt.Println("is jake or marry")
	}
}

// is jake or marry
```

**分支表达式**,case 后既可以是常量也可以和if一样添加表达式

```go
package main

import "fmt"

var (
	Num = 12
)

func main() {
	switch {
	case Num < 5 && Num > 0:
		fmt.Println("0 < Num < 5")
	case Num > 5:
		fmt.Println("Num > 5")
	}
}

// Num > 5
```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	today := t.Weekday()
	fmt.Println(t)
	switch today {
	case time.Sunday:
		fmt.Println("Sunday")
	case time.Monday:
		fmt.Println("Monday")
	case time.Thursday:
		fmt.Println("Monday")
	case time.Wednesday:
		fmt.Println("Monday")
	case time.Tuesday:
		fmt.Println("Monday")
	case time.Friday:
		fmt.Println("Monday")
	case time.Saturday:
		fmt.Println("Monday")
	}
}

// 2024-04-23 21:46:11.303226 +0800 CST m=+0.000174751
// Monday

```

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("It's before noon")
	default:
		fmt.Println("It's after noon")
	}
  
  // interface{} 这里表示空接口，表示可以接受任何类型作为他的参数
	whatAmI := func(i interface{}) {
		switch t := i.(type) {
		case bool:
			fmt.Println("I'm a bool")
		case int:
			fmt.Println("I'm an int")
		default:
			fmt.Printf("Don't know type %T\n", t)
		}
	}
	whatAmI(true)
	whatAmI(1)
	whatAmI("hey")
}

/*
It's after noon
I'm a bool
I'm an int
Don't know type string
*/
```

### 4. Array

```GO

package main

import "fmt"

func main() {

    var a [5]int
    fmt.Println("emp:", a)

    a[4] = 100
    fmt.Println("set:", a)
    fmt.Println("get:", a[4])

    fmt.Println("len:", len(a))

    b := [5]int{1, 2, 3, 4, 5}
    fmt.Println("dcl:", b)

    b = [...]int{1, 2, 3, 4, 5}
    fmt.Println("dcl:", b)

    b = [...]int{100, 3: 400, 500}
    fmt.Println("idx:", b)

    var twoD [2][3]int
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)

    twoD = [2][3]int{
        {1, 2, 3},
        {1, 2, 3},
    }
    fmt.Println("2d: ", twoD)
}

/*
emp: [0 0 0 0 0]
set: [0 0 0 0 100]
get: 100
len: 5
dcl: [1 2 3 4 5]
dcl: [1 2 3 4 5]
idx: [100 0 0 400 500]
2d:  [[0 1 2] [1 2 3]]
2d:  [[1 2 3] [1 2 3]]
*/
```

### 5. Goto 语句

在`Go`中，可以通过`goto`语句跳转标签，进行点检的无条件的跳转，`goto`语句可以快速跳出循环，避免重复退出

```go
package main

import "fmt"

func main() {
	for i := 0; i <= 5; i++ {
		if i == 2 {
			fmt.Println(i)
			goto breakTag
		}
	}
	return
breakTag:
	fmt.Println("breaknow")
}

/*
2
breaknow
*/
```

### 6. break

Go 的 break 可以结束for、switch、select 的代码块。另外，还可以在break语句后添加标签，表示退出某个标签对应的代码块

```go
package main

import "fmt"

func main() {
	println("------无 Tag ------")
	for i := 0; i <= 5; i++ {
		fmt.Println(i)
		for j := 0; j <= 5; j++ {
			fmt.Println(j)
			break
		}
	}

	println("------有 Tag ------")
// break会直接到这里将整个循环给停止
breakTag:
	for i := 0; i <= 5; i++ {
		fmt.Println(i)
		for j := 0; j <= 5; j++ {
			fmt.Println(j)
			break breakTag
		}
	}

}

/*
------无 Tag ------
0
0
1
0
2
0
3
0
4
0
5
0
------有 Tag ------
0
0
*/
```

### 7. continue

Go contiue 可以结束当前循环

```go
package main

import "fmt"

func main() {
	println("------无 Tag ------")
	for i := 0; i <= 5; i++ {
		fmt.Println(i)
		for j := 0; j <= 5; j++ {
			fmt.Println(j)
			continue
		}
	}

	println("------有 Tag ------")
breakTag:
	for i := 0; i <= 5; i++ {
		fmt.Println(i)
		for j := 0; j <= 5; j++ {
			fmt.Println(j)
      // 每次到0的时候直接跳出到代码块的头部，继续循环这里的j永远是0
			continue breakTag
		}
	}

}
/*
------无 Tag ------
0
0
1
2
3
4
5
1
0
1
2
3
4
5
2
0
1
2
3
4
5
3
0
1
2
3
4
5
4
0
1
2
3
4
5
5
0
1
2
3
4
5
------有 Tag ------
0
0
1
0
2
0
3
0
4
0
5
0
*/
```

## 获取当前时间

```go
package main

import (
	"fmt"
	"time"
)

func time_now() string {
	t := time.Now()
	year := t.Year()
	month := int(t.Month())
	day := t.Day()
	hour := t.Hour()
	minute := t.Minute()
	second := t.Second()
	timeStr := fmt.Sprintf("%d-%02d-%02d %02d:%02d:%02d", year, month, day, hour, minute, second)
	return timeStr
}

func main() {
	now := time_now()
	println(now)
}


// 结果
// 2024-04-23 21:27:01
```

## 字符串操作

Go 语言中字符串的字节使用 `UTF-8`编码来表示`Unicode`文本，`UTF-8`是文本的标准编码，所以无需像 C++、Java 或者 Python那样需要对文本进行编码或者解码，Go中字符串根据需要占用 1-4 个字节，和 Java的始终两个字节更加节省对内存和硬盘的占用。每个中文汉字大约占用3个字节

```
```

