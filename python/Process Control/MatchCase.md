# Python 3.10里面的Match-Case语法详解

### 前言

[PEP 634 – Structural Pattern Matching: Specification](https://peps.python.org/pep-0634/) : 介绍 match 语法和支持的模式 [PEP 635 – Structural Pattern Matching: Motivation and Rationale](https://peps.python.org/pep-0635/) : 解释语法这么设计的理由 [PEP 636 – Structural Pattern Matching: Tutorial](https://peps.python.org/pep-0636/) 

### switch-case 和 match-case 的区别

下面是通过 HTTP CODE 返回对应类型错误信息函数，在之前的例子中通过 if 判断要这么写:

```python
def http_error(status):
    if status == 400:
        return 'Bad request'
    elif status == 401:
        return 'Unauthorized'
    elif status == 403:
        return 'Forbidden'
    elif status == 404:
        return 'Not found'
    else:
        return 'Unknown status code'
```

如果使用`match-case`语法:

```python
def http_error(status):
    match status:
        case 400:
            return 'Bad request'
        case 401:
            return 'Unauthorized'
        case 403:
            return 'Forbidden'
        case 404:
            return 'Not found'
        case _:
            return 'Unknown status code'
```

`match`后面跟要匹配的变量，`case`后面跟不同的条件，之后是符合条件需要执行的语句。最后一个 case 加下划线表示缺省匹配，如果前面的条件没有匹配上就跑到这个 case 里面执行，相当于之前的`else`。

这其实是一个典型的`switch-case`用法，如果只是这样，我也觉得确实没必要添加这个新语法，一方面代码没有做到优化，一方面缩进反而更多了。

### 字面量 (Literal) 模式

上面的例子就是一个字面量模式，使用 Python 自带的基本数据结构，如字符串、数字、布尔值和 None:

```python3
match number:
    case 0:
        print('zero')
    case 1:
        print('one')
    case 2:
        print('two')
```

### 捕捉 (Capture) 模式

可以匹配**单个**表达式的赋值目标。为了演示方便，每个例子都会放到函数中，把 match 后面的待匹配的变量作为参数 (capture_pattern.py):

```
def capture(greeting):
    match greeting:
        case "":
            print("Hello!")
        case name:
            print(f"Hi {name}!")
    if name == "Santa":
        print('Match')
```

如果 greeting 非空，就会赋值给 name，但是要注意，如果 greeting 为空会抛 NameError 或者 UnboundLocalError 错误，因为 name 在之前没有定义过:

```python
In : capture('Alex')
Hi Alex!

In : capture('Santa')
Hi Santa!
Match

In : capture('')
Hello!
---------------------------------------------------------------------------
UnboundLocalError                         Traceback (most recent call last)
Input In [4], in <cell line: 1>()
----> 1 capture('')

Input In [1], in capture(greeting)
      1 def capture(greeting):
      2     match greeting:
      3         case "":
      4             print("Hello!")
      5         case name:
      6             print(f"Hi {name}!")
----> 7     if name == "Santa":
      8         print('Match')

UnboundLocalError: local variable 'name' referenced before assignment
```

### 序列 (Sequence) 模式

可以在 match 里使用列表或者元组格式的结果，还可以按照 [PEP 3132 – Extended Iterable Unpacking](https://peps.python.org/pep-3132/) 里面使用`first, *rest = seq`模式来解包。我用一个例子来介绍:

```python
In : def sequence(collection):
...:     match collection:
...:         case 1, [x, *others]:
...:             print(f"Got 1 and a nested sequence: {x=}, {others=}")
...:         case (1, x):
...:             print(f"Got 1 and {x}")
...:         case [x, y, z]:
...:             print(f"{x=}, {y=}, {z=}")
...:

In : sequence([1])

In : sequence([1, 2])
Got 1 and 2

In : sequence([1, 2, 3])
x=1, y=2, z=3

In : sequence([1, [2, 3]])
Got 1 and a nested sequence: x=2, others=[3]

In : sequence([1, [2, 3, 4]])
Got 1 and a nested sequence: x=2, others=[3, 4]

In : sequence([2, 3])

In : sequence((1, 2))
Got 1 and 2
```

注意，需要符合如下条件:

1. 这个 match 条件第一个元素需要是 1，否则匹配失败
2. 第一个 case 用的是列表和解包，第二个 case 用的是元组，其实和列表语义一样，第三个还是列表

如果 case 后接的模式是单项的可以去掉括号，这么写:

```python
def sequence2(collection):
    match collection:
        case 1, [x, *others]:
            print(f"Got 1 and a nested sequence: {x=}, {others=}")
        case 1, x:
            print(f"Got 1 and {x}")
        case x, y, z:
            print(f"{x=}, {y=}, {z=}")
```

但是注意，其中`case 1, [x, *others]`是不能去掉括号的，去掉了解包的逻辑就变了，要注意。

### 通配符 (Wildcard) 模式

使用单下划线`_`匹配任何结果，**但是不绑定 (不赋值到某个或者某些变量上)**。一开始的例子:

```python
def http_error(status):
    match status:
        ... # 省略
        case _:
            return 'Unknown status code'
```

最后的`case _`就是通配符模式，当然还可以有多个匹配:

```python
In : def wildcard(data):
...:     match data:
...:         case [_, _]:
...:             print('Some pair')
...:

In : wildcard(None)

In : wildcard([1])

In : wildcard([1, 2])
Some pair
```

在前面说到的序列模式也支持`_`:

```python
In : def sequence2(collection):
...:     match collection:
...:         case ["a", *_, "z"]:
...:             print('matches any sequence of length two or more that starts with "a" and ends with "z".')
...:         case (_, _, *_):
...:             print('matches any sequence of length two or more.')
...:         case [*_]:
...:             print('matches a sequence of any length.')
...:

In : sequence2(['a', 2, 3, 'z'])
matches any sequence of length two or more that starts with "a" and ends with "z".

In : sequence2(['a', 2, 3, 'b'])
matches any sequence of length two or more.

In : sequence2(['a', 'b'])
matches any sequence of length two or more.

In : sequence2(['a'])
matches a sequence of any length.
```

使用通配符需求注意逻辑顺序，把范围小的放在前面，范围大的放在后面，防止不符合预期。

### 恒定值 (constant value) 模式

这种模式主要匹配常量或者 enum 模块的枚举值:

```python
In : class Color(Enum):
...:     RED = 1
...:     GREEN = 2
...:     BLUE = 3
...:

In : class NewColor:
...:     YELLOW = 4
...:

In : def constant_value(color):
...:     match color:
...:         case Color.RED:
...:             print('Red')
...:         case NewColor.YELLOW:
...:             print('Yellow')
...:         case new_color:
...:             print(new_color)
...:

In : constant_value(Color.RED)  # 匹配第一个case
Red

In : constant_value(NewColor.YELLOW)  # 匹配第二个case
Yellow

In : constant_value(Color.GREEN)  # 匹配第三个case
Color.GREEN

In : constant_value(4)  # 常量值一样都匹配第二个case
Yellow

In : constant_value(10)  # 其他常量
10
```

这里注意，因为 case 具有绑定的作用，所以不能直接使用 YELLOW 这种常量，例如下面这样:

```python
YELLOW = 4


def constant_value(color):
    match color:
        case YELLOW:
            print('Yellow')
```

这样语法是错误的。

### 映射 (Mapping) 模式

其实就是 case 后支持使用字典做匹配:

```python
In : def mapping(config):
...:     match config:
...:         case {'sub': sub_config, **rest}:
...:             print(f'Sub: {sub_config}')
...:             print(f'OTHERS: {rest}')
...:         case {'route': route}:
...:             print(f'ROUTE: {route}')
...:

In : mapping({})

In : mapping({'route': '/auth/login'})  # 匹配第一个case
ROUTE: /auth/login

# # 匹配有sub键的字典，值绑定到sub_config上，字典其他部分绑定到rest上
In : mapping({'route': '/auth/login', 'sub': {'a': 1}})  # 匹配第二个case
Sub: {'a': 1}
OTHERS: {'route': '/auth/login'}
```

### 类 (Class) 模式

case 后支持任何对象做匹配。我们先来一个错误的示例:

```python
In : class Point:
...:     def __init__(self, x, y):
...:         self.x = x
...:         self.y = y
...:

In : def class_pattern(obj):
...:     match obj:
...:         case Point(x, y):
...:             print(f'Point({x=},{y=})')
...:

In : class_pattern(Point(1, 2))
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Input In [], in <cell line: 1>()
----> 1 class_pattern(Point(1, 2))

Input In [], in class_pattern(obj)
      1 def class_pattern(obj):
      2     match obj:
----> 3         case Point(x, y):
      4             print(f'Point({x=},{y=})')

TypeError: Point() accepts 0 positional sub-patterns (2 given)
```

这是因为对于匹配来说，**位置需要确定**，所以需要使用位置参数来标识:

```python
In : def class_pattern(obj):
...:     match obj:
...:         case Point(x=1, y=2):
...:             print(f'match')
...:

In : class_pattern(Point(1, 2))
match
```

另外一个解决这种自定义类不用位置参数的匹配方案，使用`__match_args__`返回一个位置参数的数组，就像这样:

```python
In : class Point:
...:     __match_args__ = ('x', 'y')
...:
...:     def __init__(self, x, y):
...:         self.x = x
...:         self.y = y
...:

In : from dataclasses import dataclass

In : @dataclass
...: class Point2:
...:     x: int
...:     y: int
...:

In : def class_pattern(obj):
...:     match obj:
...:         case Point(x, y):
...:             print(f'Point({x=},{y=})')
...:         case Point2(x, y):
...:             print(f'Point2({x=},{y=})')
...:

In : class_pattern(Point(1, 2))
Point(x=1,y=2)

In : class_pattern(Point2(1, 2))
Point2(x=1,y=2)
```

这里的 Point2 使用了标准库的 dataclasses.dataclass 装饰器，它会提供`__match_args__`属性，所以可以直接用。

### 组合 (OR) 模式

可以使用`|`将多个字面量组合起来表示或的关系，`|`可以在一个 case 条件内存在多个，表示多个或关系:

```python
def or_pattern(obj):
    match obj:
        case 0 | 1 | 2: # 0,1,2三个数字匹配
            print('small number')
        case list() | set():  # 列表或者集合匹配
            print('list or set')
        case str() | bytes():  # 字符串或者bytes符合
            print('str or bytes')
        case Point(x, y) | Point2(x, y):  # 借用之前的2个类，其中之一符合即可
            print(f'{x=},{y=}')
        case [x] | x:  # 列表且只有一个元素或者单个值符合
            print(f'{x=}')
```

这里注意一下，由于匹配顺序，`case [x] | x`这句中的`[x]`是不会被触发的，另外 x 不能是集合、字符串、byte 等类型，因为在前面的条件中会被匹配到不了这里。我们试一下:

```python
In : or_pattern(1)
small number

In : or_pattern(2)
small number

In : or_pattern([1])
list or set

In : or_pattern({1, 2})
list or set

In : or_pattern('sss')
str or bytes

In : or_pattern(b'sd')
str or bytes

In : or_pattern(Point(1, 2))
x=1,y=2

In : or_pattern(Point2(1, 2))
x=1,y=2

In : or_pattern(4)
x=4

In : or_pattern({})
x={}
```

另外在 Python 里是没有表示 AND 关系的 case 语法的。

### AS 模式

AS 模式在早期其实是海象 (Walrus) 模式，后来讨论后发现使用`as`关键字可以让这个语法更有优势:

```python
In : def as_pattern(obj):
...:     match obj:
...:         case str() as s:
...:             print(f'Got str: {s=}')
...:         case [0, int() as i]:
...:             print(f'Got int: {i=}')
...:         case [tuple() as tu]:
...:             print(f'Got tuple: {tu=}')
...:         case list() | set() | dict() as iterable:
...:             print(f'Got iterable: {iterable=}')
...:
...:

In : as_pattern('sss')
Got str: s='sss'

In : as_pattern([0, 1])
Got int: i=1

In : as_pattern([(1,)])
Got tuple: tu=(1,)

In : as_pattern([1, 2, 3])
Got iterable: iterable=[1, 2, 3]

In : as_pattern({'a': 1})
Got iterable: iterable={'a': 1}
```

需要注明一下，这里面的`[0, int() as i]`是一种子模式，也就是在模式中包含模式:`[0, int() as i]`是 case 匹配的序列模式，而其中`int() as i`是子模式，是 AS 模式。

子模式在 match 语法里面是可以灵活组合的。

### 向模式添加条件

另外模式还支持加入 if 判断 (叫做 guard):

```python
In : def go(obj):
...:     match obj:
...:         case ['go', direction] if direction in ['east', 'north']:
...:             print('Right way')
...:         case direction if direction == 'west':
...:             print('Wrong way')
...:         case ['go', _] | _:
...:             print('Other way')
...:

In : go(['go', 'east'])  # 匹配条件1
Right way

In : go('west')  # 匹配条件2
Wrong way

In : go('north')  # 匹配默认条件
Other way

In : go(['go', 'west'])  # 匹配默认条件
Other way
```

这样可以让匹配作进一步判断，相当于实现了某个程度的 AND 效果.