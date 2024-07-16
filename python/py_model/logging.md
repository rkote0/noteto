# python logging日志模块

## 日志级别

> 日志根据重要程度分为5个级别

![image-20240715232602163](assets/image-20240715232602163.png)



**日志级别**	

- DEBUG: debug级别用来记录详细的信息，方便定位问题进行调试，在生产环境我们一般不开启DEBUG
- INFO: 用来记录关键代码点的信息，以便代码是否按照我们预期的执行，生产环境通常会设置INFO级别
- WARNING: 记录某些不预期发生的情况，如磁盘不足
- ERROR: 由于一个更严重的问题导致某些功能不能正常运行时记录的信息
- CRITICAL: 当发生严重错误，导致应用程序不能继续运行时记录的信息



用标准库logging模块记录生成的日志,看个例子,直接导入`logging`模块，然后使用logging提供的日志消息记录方法就可以



![image-20240715232507272](assets/image-20240715232507272.png)

> 从上面代码中可以看到`loging.warging`以后的日志内容都打印在标准输出流，也就是命令行窗口，但是`logging.debug`和`info`记录的日志并没有打印出来。因为默认的日志级别是 warning。

### 如何修改日志级别

如何让debug级别的信息也输出？

当然是修改默认的日志级别，在开始记录日志前可以使用`logging.basicConfig`方法来设定日志级别

```python
import logging
logging.basicConfig( level=logging.DEBUG)
logging.debug("this is debug")
logging.info("this is info")
logging.error("this is error")
```

![image-20240715232918199](assets/image-20240715232918199.png)

### 如何将日志记录到文件

通过配置函数`logging.basicConfig`可以指定日志输出到什么地方, 如果每次运行前要覆盖之前的日志，则需指定 filemode='w'， 这个和 `open` 函数写数据到文件用的参数是一样的。

```python
import logging
logging.basicConfig(filename="test.log", level=logging.INFO)
logging.debug("this is debug")
logging.info("this is info")
logging.error("this is error")
```

### 如何指定日志记录格式

默认输出的格式包含3部分，日志级别、日志记录器的名字、以及日志内容，中间用“:”连接。 如果我们想改变日志格式，例如想加入日期时间、显示日志器名字，我们是可以用`format`参数来设置日志的格式

```python
import logging
logging.basicConfig(format='%(asctime)s %(levelname)s %(name)s %(message)s')
logging.error("this is error")
```

![image-20240715233110548](assets/image-20240715233110548.png)

日志格式化输出提供了非常多的参数，除了时间、日志级别、日志消息内容、日志记录器的名字个外，还可以指定线程名，进程名，日志记录所在行号等等。

![image-20240715233136504](assets/image-20240715233136504.png)

## 记录器（logger）

每一次日志记录其实都是通过一个叫做**日志记录器（Logger）**的实例对象来负责记录的，每个记录器都有一个名称，当我们直接使用`logging`来记录日志时，系统会默认创建一个名为 `root` 的记录器，这个记录器我们称为根记录器。

记录器像树结构一样支持层级，子记录器可以不单独设置日志级别以及Handler（后面会介绍），如果子记录器没有单独设置，则它的所有行为会委托给父级。

