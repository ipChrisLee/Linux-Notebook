# `Overlook`

## 如果要用`gdb`

如果要使用`gdb`，需要在编译的时候就插入调试信息，在`gcc`里即需要在编译命令中插入`-g`参数

之后只需要`gdb $filename`就可以在`debug`模式下执行`filename`了



## `gdb`下的程序

使用`gdb`执行的程序仍然从标准输入输出`stdio`获得数据



# 常用命令



## 一览

| 命令                     | 作用                                 | 简写 |
| ------------------------ | ------------------------------------ | ---- |
| `help`                   | 查看命令帮助                         | `h`  |
| `run`                    | 重新开始运行文件                     | `r`  |
| `start`                  | 单步执行模式运行程序，停在第一句语句 |      |
| `list`                   | 查看源代码                           | `l`  |
| `set`                    | 设置变量的值                         |      |
| `next`                   | 单步调试，跳过函数                   | `n`  |
| `step`                   | 单步调试，进入函数内部               | `s`  |
| `backtrace`              | 查看函数的调用的栈帧和层级关系       | `bt` |
| `info`                   | 查看函数内部局部变量的数值           | `i`  |
| `finish`                 | 结束当前函数，返回函数调用点         |      |
| `continue`               | 继续运行                             | `c`  |
| `print`                  | 打印值及地址                         | `p`  |
| `break+num`              | 在`num`行设置断点                    | `b`  |
| `quit`                   | 退出`gdb`                            | `q`  |
| `info breakpoints`       | 查看当前设置的所有断点               |      |
| `delete breakpoints num` | 删除第`num`个断点                    |      |
| `display`                | 追踪查看具体变量值                   |      |
| `undisplay`              | 取消追踪查看变量                     |      |
| `watch`                  | 设置观察点                           |      |
| `enable breakpoints`     | 启用断点                             |      |
| `disable breakpoints`    | 禁用断点                             |      |
| `x`                      | 查看内存                             |      |



## 流程控制

### `run`、`start`和`starti`

`run`就是直接开始程序，`start`相当于在`main`的一开始设置一个临时断点然后执行`run`。这个临时断点可能不会一开始就触发，比如在`C++`中，程序会先执行一些变量的构造，这时可能会触发断点，但是无论如何`start`设置的临时断点一定会被触发。如果想在程序实际执行的第一行停下，那就用`starti`

可以在这三个命令后面跟命令行参数`argv`



### 命令行参数

除了在`run`、`start`、`starti`的时候传参，还有一种方式：`set args`

我们可以使用`set args $ARGS`的方式将命令行参数设为`$ARGS`，这样之后每次运行时的命令行参数都将是`$ARGS`。如果想要将命令行参数设为空，用`set args`命令即可

也可以调用`show args`来查看命令行参数



### `checkpoint`相关

在`GNU/LINUX`下，`GDB`可以存储下当前程序的状态，并且支持随时返回当前状态（牛逼啊）。

* `checkpoint`

  在当前位置存储一个`checkpoint`，这个`checkpoint`将被赋予一个整数编号

* `info checkpoints`

  列出所有的`checkpoint`

* `restart $checkpoint-id`

  程序回到编号为`$checkpoint-id`的程序状态

* `delete checkpoint $checkpoint-id`

  删除编号为`$checkpoint-id`的`checkpoint`

这玩意看起来真nm屌



### 进阶

见[GDB-Running](https://sourceware.org/gdb/onlinedocs/gdb/Running.html#Running)



## 断点和暂停

`info program`显示程序状态

网络文档[地址](https://sourceware.org/gdb/onlinedocs/gdb/Breakpoints.html#Breakpoints)

### 断点分类

总共有三种断点：

* `breakpoint`：最基础的断点，在被打断点的语句执行前会中断程序
* `watchpoint`：`watchpoint`会存储一个表达式，这个表达式的值改变时中断程序
* `catchpoint`：当程序发生某个行为时中断程序



### `Breakpoints`相关

* `break $location`

  在`$location`处打一个断点。关于`location`可以是什么见[Specifying a Location](https://sourceware.org/gdb/onlinedocs/gdb/Specify-Location.html#Specify-Location)

* `break`

  这个指令我不太懂。。。即使翻了翻译我也看不懂什么意思，但是可能是：在当前位置打一个断点

* `break $location if $cond`

  在`$location`打一个条件断点，只有当`$cond`为真时，才会中断程序

  一个`$location`可能对应多个地方（比如一个函数可能在不同地方声明过），对于这些不同的地方而言，`$cond`中的变量不一定都存在，此时：

  * 在所有地方`$cond`都有问题，此时所有断点都打不上。可以在`if`前加上`-force-condition`来强制要求所有断点都生效
  * 在某些地方`$cond`是可以的，此时所有断点都会打上，但是如果`$cond`不合法，在那个位置断点不会生效

* `tbreak $args`

  临时断点，`$args`和`break`的一样

* `hbreak args`

  硬件断点，这里不做了解

* `rbreak $regex`

  对于**所有**满足`$regex`正则表达式的函数设置断点

  还可以使用`rbreak $file:$regex`来限制作用范围



### `Watchpoint`相关

先说好，这里我没太看懂[网页文档上的介绍](https://sourceware.org/gdb/onlinedocs/gdb/Set-Watchpoints.html#Set-Watchpoints)，特别是"when the expression expr is written into by the program"和"when the value of expr is read by the program"

这里需要先介绍一些前置知识

这里有一个函数：

```c
int main(){
    int x=0;int * p=&x;int y=1;
}
```

如果我们使用`GDB`在进入这个函数的时候调用指令`info local`，可以发现`x`、`p`、`y`已经被定义且有值了！这是局部变量的性质

此外，`watchpoint`还有一个重要的作用是监视地址变化。我们可以监视某一个变量或者某一个地址是否被程序读出、写入，这是很重要的一个功能

* `watch [-l|-location] $expr`

  设置一个监视点，表达式为`$expr`。当这个表达式的值改变时、“当程序写入表达式`$expr`”（原文如此，我看不懂。。。）时会中断程序

* `rwatch [-l|-location] $expr`

  设置一个监视点，表达式为`$expr`，当程序读取`$expr`的内容时，中断程序

* `awatch [-l|-location] $expr`

  设置一个监视点，表达式为`$expr`，当程序读写（`access`）`$expr`的内容时，中断程序

上文的`-l`都是将`$expr`解释成指针，没有`-l`可以把`$expr`解释成引用



### `Catchpoint`相关

[自己看](https://sourceware.org/gdb/onlinedocs/gdb/Set-Catchpoints.html#Set-Catchpoints)



### 整体操作

下面的断点都是`Breakpoint`、`Watchpoint`和`Catchpoint`

* `info breakpoints`｜`info break`

  显示所有的断点。注意这里的断点包括`Watchpoint`和`Catchpoint`

* `disable [breakpoints] [$list]`

  禁止某些断点（或者全部，如果`$list`没有内容的话）

  可以用`dis`代替`disable`

* `enable [breakpoints] [$list]`

  开启某些断点（或者全部）

  * `enable [breakpoints] once $list`：开启一次
  * `enable [breakpoints] count $count $list`：开启`$count`次
  * `enable [breakpoints] delete $list`：开启一次，完事后删了

* `clear`

  删除下一个指令上的断点

* `clear $location`

  删除某个位置上的断点。可以使用`clear $filename:$location`来特定某一个文件

* `delete [breakpoints] [$list]`

  删除某些断点，或者全部

* `save breakpoints [$filename]`

  存储目前的所有断点



## 继续和步进

### `continue`

* `continue [$ignore-count]`｜`c [$ignore-count]`｜`fg [$ignore-count]`

  继续程序，跳过`$ignore-count`次断点



### `step`

* `step`｜`s`

  继续程序，在下一个源文件行停下（注意不是下一个分号语句，就是下一行）。如果下一个是函数，就会在函数里面第一个语句的行停下。注意函数的右括号会被算作一个语句。比如函数`void fun(){}`会被看作只有一个空语句，`step`进去的时候会在`}`所在行停下

  注意，如果一个函数的源文件在编译的时候没有`-g`也就是插入`debug`信息，那么仍然会`step`进这个函数，但是无法看到函数的信息

* `step $count`｜ `s $count`

  `step`程序`$count`次，如果碰到了断点或者“a signal not related to stepping occurs before countsteps”，就会提前停下



### `next`

* `next [$count]`｜`n [$count]`

  和`step`类似，只不过不会进入到函数



### `finish`

* `finish`｜`fin`

  继续执行程序，直到当前函数返回。根据设置会输出返回值/不输出返回值

* `set print finish [on|off]`

  设置`finish`命令是/否输出返回值。默认情况下是“是”

* `show print finish`

  显示返回值显示的设置



### `until`

* `until`｜`u`

  执行程序直到某个当前语句之后的一行语句被执行

  说人话就是，当你的程序在一个循环的末端时，`until`将会让你的程序一直执行直到跳出循环，而`next`将会继续这个循环，进入循环头

  对于`for`循环，需要注意的时，实际上“循环末端”是在`for`那行，所以当程序在`for`行暂停的时候再`until`才会退出循环

* `until $location`｜`u $location`

  继续执行程序直到：要么到了`$location`、要么函数返回了

  它使用临时断点的形式实现的，所以比无参数`location`快

  一般用在递归函数的暂停上



### `stepi`和`nexti`

* `stepi [$count]`｜`si [$count]`

  和`step`一样，不过`step`针对的是行，`stepi`针对的是机器指令

* `nexti [$count]`｜`ni [$count]`

  和`next`一样，不过`next`针对的是行，`nexti`针对的是机器指令



## 函数控制

### `skip`相关

* `skip [$options]`

  其作用是跳过一些你不感兴趣的函数，这样使用`step`的时候就不会进入这些函数

  其中`$options`可以是

  * `-file $file`｜`fi $file`

    `$file`文件里所有的函数

  * `-gfile $file-glob-pattern`｜`-gfi $file-glob-pattern`

    所有满足`$file-glob-pattern`正则表达式的文件都会被跳过

    例子：`skip -gfi utils/*.c`

  * `-function $linespec`｜`-fu $linespec`

    跳过`$linespec`行的函数

  * `-rfunction $regexp`｜`-rfu regexp`

    跳过满足`$regexp`正则表达式的函数

  * 空

    跳过当前正在执行的函数

* `info skip [$range]`

  输出所有`$range`范围内（如果没有`$range`范围就是所有）的`skip`信息

* `skip delete [$range]`

  删除某个`skip`。如果`$range`没有指定，删除所有的`skip`

* `skip enable [$range]`

  开启某个`skip`。如果`$range`没有指定，开启所有的`skip`

* `skip disable [$range]`

  关闭某个`skip`。如果`$range`没有指定，关闭所有的`skip`



## 栈分析

[GDB信息页](https://sourceware.org/gdb/onlinedocs/gdb/Stack.html#Stack)

遇到的时候去学



## 源文件分析

### `list`

* `list $linenum`

  输出当前文件`$linenum`行附近的内容

* `list $function`

  输出`$function`开头附近的内容

* `list`｜`list +`

  接着上次输出的内容输出

* `list -`

  输出上次输出内容之前的内容



### 源文件搜索

* `forward-search $regexp`｜`search $regex`

  在当前文件正则搜索`$regexp`匹配的内容，**从文件头开始向文件尾一个个显示**，`fo`显示下一个

* `reverse-search $regexp`

  在当前文件正则搜索`$regexp`匹配的内容，**从文件尾开始向文件头一个个显示**，`rev`显示下一个



### 机器码

* `info line [$location]`

  打印`$location`位置（若无`$location`，就打印当前行或者接着之前的`info line`打印）的开始`PC`和结束`PC`

  特别的，如果`$location`是`*0x63ff`一样的地址，那就会输出包含这个地址的语句的开始结束`PC`

* `disassemble`

  打印反汇编代码

  有几种模式：

  * `/m`｜`/s`

    同时输出源文件内容和机器码内容。在被优化代码、内联函数存在时，`/m`会不准确，所以`/s`更被推荐

  * `/r`

    同时输出机器码十六进制内容

  可以加上范围：`$start,$end`和`$start,+$length`都会指明范围。注意这里的范围都是机器地址，比如`0x32c4`、`&main+10`、`$pc - 8`



## 数据分析

就是`print`，关于更多细节可以见[GDB介绍](https://sourceware.org/gdb/onlinedocs/gdb/Data.html#Data)



## 宏分析

[GDB关于macro的介绍](https://sourceware.org/gdb/onlinedocs/gdb/Macros.html#Macros)



## 记录和回放

[GDB的内容页](https://sourceware.org/gdb/onlinedocs/gdb/Process-Record-and-Replay.html#Process-Record-and-Replay)

