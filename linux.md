# `Ubuntu`

## 系统信息

选择的是`amd64`的`20.04Ubuntu`

下载地址：[网易的Ubuntu20.04](http://mirrors.163.com/ubuntu-releases/20.04/)



## 中文输入法

见`Download Input Sources In Ubuntu`，来源：[在Ubuntu中配置中文输入法](https://blog.csdn.net/nanhuaibeian/article/details/85851335)

只需要前几步就行了





## `apt`

更换源方法，来源[apt换源方法](https://www.cnblogs.com/chenfuhai/p/13140529.html)

按照`ICS2021`的方式用了`TsinghuaU`的源

下面语句会帮助更新包

```shell
sudo apt-get update
```





## 安装包

```shell
sudo apt-get install build-essential    # build-essential packages, include binary utilities, gcc, make, and so on
sudo apt-get install man                # on-line reference manual
sudo apt-get install gcc-doc            # on-line reference manual for gcc
sudo apt-get install gdb                # GNU debugger
sudo apt-get install git                # revision control system
sudo apt-get install libreadline-dev    # a library used later
sudo apt-get install libsdl2-dev        # a library used later
sudo apt-get install llvm-11            # llvm project, which contains libraries used later
sudo apt-get install vim
sudo apt-get install tmux
sudo apt-get install flex bison
sudo apt install vim-gtk # 这个vim支持系统剪切板
sudo apt install ncurses-base  # 这个让命令行可以用左右键
sudo apt install cloc  # 用来统计行数的
sudo apt install xclip  # 命令行剪切板，在命令行做任务时比较有用
sudo apt-get install tree # 树形显示文件夹结构
```

可以手动安装`vimcat`包，用来在`terminal`格式化、语法高亮地`cat`，参考[`vimcat`的`github`](https://github.com/ofavre/vimcat)



## `VPN`

见页面下的`Use Clash In Ubuntu.pdf`



我们编写了一个`shell`用来半自动化地打开/关闭VPN，编写`shell`地方法来自[链接](https://vitux.com/how-to-write-a-shell-script-in-ubuntu/)，这个`shell`的位置在`~/shells`

具体来说，这个`shell`的原理大概是：

````shell
gnome-control-center network
cd ~/clash
./clash -d .
````

它会自动打开`clash`，并且打开设置页面

也有一个自动关闭的`shell`

之后打开`VPN`的方式：`~/shells/openVPN.sh`，关闭`~/shells/closeVPN.sh`



下面的语句可以检测`VPN`有没有坏

```shell
ping www.pixiv.com -c 4
```

（`google`的防火墙会屏蔽`ping`的内容）



## `python`安装

`sudo apt-get install python`即可



## 文件编码及转换

使用`file`命令可以查看文件的编码

使用`iconv`命令可以改变文件编码，生成新文件，比如将一个`UTF-8`编码的文件转换成`GBK`编码：`iconv -f UTF-8 -t GBK file1 -o file2`



## 应用程序

一般情况下，使用`apt-get`之类的应用程序时可以自动安装应用程序到合理的位置，但是在安装一些其他应用时，需要自定义位置

比如在`pycharm`的安装中就要求将程序安装在`{installation home}/bin`下，其中`installation home`是自定义安装`pycharm`的位置

我们将所有的应用程序装在了`~/Applications/`下，之后在`~/Applications/shells`里写一些脚本，比如`AppPycharm`，然后再在`~/.zshrc`里加上`export PATH=$PATH:~/Applications/shells`，之后就可以在任意地方打开终端执行`AppPyCharm`来启动程序了

此外，可以在`pycharm`左下角设置将`pycharm`图标放在`application desktop`上



# 命令行

## `zsh`

我们选择用`zsh`来代替原先的命令行`bash`，参考[教程](https://www.tecmint.com/install-zsh-in-ubuntu/)，这个教程也有换回`bash`的方法

`zsh`需要`oh-my-zsh`来配合使用，见[教程](https://www.tecmint.com/install-oh-my-zsh-in-ubuntu/)

`oh-my-zsh`的[官网](https://ohmyz.sh)，安装插件一般是需要通过`Github`，[这里](https://github.com/unixorn/awesome-zsh-plugins)是插件列表

（其实好想没啥区别，主要是高亮）

一些插件：[by胡方运](https://hufangyun.com/2017/zsh-plugin/)

我们安装的插件：

```
copydir k git zsh-autosuggestions zsh-syntax-highlighting
```

* `k`

  可以在`terminal`输入`k`，来得到文件夹权限、git状态、修改时间、大小之类的

* `autojump`

  快速跳转，不需要再输入路径

  一个使用方法：输入`d`显示一些常用路径，然后输入数字，就可以进入相应路径了

`zsh`也有一些主题，[这里](https://github.com/ohmyzsh/ohmyzsh/wiki/themes)提供了一些主题预览



在`bash`转`zsh`的时候，会出现环境变量失效的情况，此时需要参考[文章](https://blog.csdn.net/catchaobject/article/details/102974929)的办法，把这些变量从`.bashrc`中迁移到`.zshrc`中

`zsh`自带的功能：







## 常用命令

* `cd`

  打开某个路径

  * `cd path`

    打开某个路径/文件夹，其中`path`是一个参数，如果是`/`开始的，表示绝对路径；如果不是则表示相对路径

* `ls`

  列出文件

  * `ls`

    仅列出当前目录可见文件

  * `ls -l`

    列出当前目录可见文件详细信息

  * `ls -hl`

    列出详细信息并以可读大小显示文件大小

  * `ls -al`

    列出所有文件（包括隐藏）的详细信息
    
  * `ls -d $regex`

    根据正则`$regex`匹配输出文件

    例如`ls -d *.c`输出所有`.c`文件

  在`linux`中，所有以`.`开头的文件/文件夹均为隐藏的

* `pwd`

  * `pwd`

    返回当前工作目录的名字，是绝对路径

* `mkdir`

  创建文件夹

  * `mkdir folder`

    创建文件夹`folder`，如果文件夹已经存在会被识别出来

  * `mkdir -p folder/subfolder`

    创建多级文件夹，如果`folder`已经存在则不会新建一个`folder`，如果不存在则新建一个

* `rm`

  即`remove`，删除文件

  * `rm filename`

    删除`filename`

  * `rm -i filename`

    删除`filename`前提示，若多个文件则每次提示

  * `rm -rf folder/subfolder`

    递归删除`subfolder`下所有文件及文件夹，包括`subfolder`自身

    直接`rm -rf folder`原理一样

  * `rm -d folder`

    删除**空**文件夹

  `rm`可以有多个参数，可以一次删除多个文件

* `touch`

  创建文件

  * `touch NEWFILE`

    创建空文件

* `cp`

  即`copy`，复制文件

  下面的`dest`只能有一个，`source`可以有多个，如果是文件夹就会将所有源文件复制过来，如果是文件名就会将源文件复制过来的同时改名

  * `cp source dest`

    将`source`复制到`dest`

  * `cp folder/* dest`

    将`folder`下所有文件(不含子文件夹中的文件)复制到`dest`

  * `cp -r folder dest`

    将`folder`下所有文件（包含子文件夹中的所有文件）复制到`dest`

* `mv`

  即`move`，移动文件，也承担了更改文件名的作用

  * `mv source folder`

    将`source`移动到`folder`下，完成后则为`folder/source`

  * `mv -i source folder`

    在移动时，若文件已存在则提示是否覆盖

  * `mv source dest`

    在`dest`不为目录的前提下，重命名`source`为`dest`

* `cat`

  输出文件内容到`Terminal`。如果文档过长，会展示最后布满屏幕的内容，前面的内容不可见

  * `cat filePath`

    输出`filePath`文件的内容

  * `cat -n filePath`

    输出`filePath`的内容并显示行号

* `hd`

  输出文件十六进制内容到`Terminal`

* `more`

  逐行输出文件内容到`Terminal`

  * `more filePath`

    一行行输出`filePaht`文件的内容

  * `more +100 filePath`

    从`100`行开始输出内容

* `less`

  和`more`差不多，但是支持上下滚动

  * `less filePath`
  * `less +100 filePath`

* `poweroff`

  关机，需要管理员权限

* `reboot`

  重启，需要管理员权限

* `man`

  帮助命令，比如`man ls`可以看`ls`的使用方法

* `su`

  即`switch user`

  * `su -`

    转入`root`用户，注意要`sudo su -`才能在非`root`用户下执行。。。

* `find`

  查找命令，比如`sudo find ./ -name "*isa*"`就表示查找当前文件夹下所有包含`isa`这个字符串的文件/文件夹

  `sudo find / -name "isa"`则表示查找文件夹下所有包含`isa`这个字符串的文件/文件夹

  `sudo find ./ -size 1G`则表示查找当前文件夹下大小大于`1G`的文件
  
* `man`

  帮助命令，我们一般在`Vim`里使用`Man`命令替代

  `man -k $keyword`可以搜索以`$keyword`作为关键词的帮助



## 重定向

[ref](https://askubuntu.com/questions/420981/how-do-i-save-terminal-output-to-a-file)

`>`只重定向`stdout`并且会清除文件内容，`>>`只重定向`stdout`并且会追加内容到文件末尾

`&>`会将`stdout`和`stderr`都重定向，`&>`原理同`>>`

更多：

```txt
          || visible in terminal ||   visible in file   || existing
  Syntax  ||  StdOut  |  StdErr  ||  StdOut  |  StdErr  ||   file   
==========++==========+==========++==========+==========++===========
    >     ||    no    |   yes    ||   yes    |    no    || overwrite
    >>    ||    no    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
   2>     ||   yes    |    no    ||    no    |   yes    || overwrite
   2>>    ||   yes    |    no    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
   &>     ||    no    |    no    ||   yes    |   yes    || overwrite
   &>>    ||    no    |    no    ||   yes    |   yes    ||  append
          ||          |          ||          |          ||
 | tee    ||   yes    |   yes    ||   yes    |    no    || overwrite
 | tee -a ||   yes    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    || overwrite
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
|& tee    ||   yes    |   yes    ||   yes    |   yes    || overwrite
|& tee -a ||   yes    |   yes    ||   yes    |   yes    ||  append
```



## `man`

`man`各个编号对应的内容：

<img src="ref/截屏2022-02-24 17.02.45.png" style="zoom:50%;" />







## 一些`shell`命令

### 统计代码行数

* 一般的：

  `find . -name "*\.[c|h]" | xargs cat | grep -v ^$$ | wc -l`

* 去除空行的：

  `find . -name "*\.[c|h]" | xargs cat | grep -v ^$$ | wc -l`



## 自己写命令

[使用`alias`的方式参考](https://blog.csdn.net/weixin_30245867/article/details/101102166)

[使用`shell`编写`.sh`文件](https://www.shellscript.sh/index.html)（也可以参考[菜鸟教程](https://www.runoob.com/linux/linux-shell.html)），然后按照下面的方法：

先保证`~/.zshrc`里面有`export PATH=$PATH:"~/shells"`这句话

然后把`.sh`文件放在`~/shells`里面

有的文件会缺少权限，需要使用[`chmod`命令](https://www.runoob.com/linux/linux-comm-chmod.html)来给权限

写了一个脚本`__all_upd.sh`可以在执行后自动复制出一份没有`.sh`后缀的脚本文件

搞完之后需要重启`zsh`

一个成功的脚本：（其中`while getopts`见[介绍](https://www.baeldung.com/linux/use-command-line-arguments-in-bash-script)）

```shell
# 在DIR下所有文件夹里递归地选择满足PATTERN的文件名的文件中的CONTENT内容
DIR="./"
while getopts p:c:d:h flag
do
	case $flag in 
		p) PATTERN=$OPTARG;;
		c) CONTENT=$OPTARG;;
		d) DIR=$OPTARG;;
		h) 
			echo "[-p] : Regex pattern for filenam mattching."
			echo "[-c] : String content to be found in files."
			echo "[-d] : Root direction."
			echo "[-h] : Show find Information and exit."
			exit
	esac
done

if [ -z "$CONTENT" ] || [ -z "$PATTERN" ] ; then
	echo "Lack of Parameters!"
fi

find "$DIR" -type f -name "$PATTERN" | xargs grep "$CONTENT"
```



可能用到的一些资料：

* `shell`自带的字符串剪切：[链接](https://stackoverflow.com/questions/4168371/how-can-i-remove-all-text-after-a-character-in-bash)
* `shell`里字符串比较：[链接](https://www.jb51.net/article/56559.htm)
* `shell`里改字体颜色（`python`脚本）：[链接](https://blog.51cto.com/u_15127626/2734711)
* 在`script`的第一行加上`#!bin/zsh`



# `tmux`

`tmux`可以快速地分屏、转移窗格

资料：

* [一文助你打通 tmux - zempty的文章 - 知乎](https://zhuanlan.zhihu.com/p/102546608)





## 页面介绍

<img src="noteSrc/截屏2021-12-06 18.35.11.png" style="zoom:50%;" />

`[2]`所在的`session`

`0: 1: 2:`打开的窗口，`*`表示当前所在窗口



## 快捷键设置

先`cd ~`

然后`vim .tmux.conf`

就可以进入`tmux`的设置页面了

目前我们的`tmux`的内容有：

```shell
bind-key c new-window -c "#{pane_current_path}"
bind-key % split-window -h -c "#{pane_current_path}"
bind-key '"' split-window -c "#{pane_current_path}"
bind-key k select-pane -U
bind-key j select-pane -D
bind-key h select-pane -L
bind-key l select-pane -R
```



## `session`命令

* 新建`session`：`tmux new -s <sname>`

  创建一个名为`sname`的`session`

* 挂起`session`：`tmux detach`

  将当前所在的`session`挂起，快捷键：`C+b d`

* 进入`session`：`tmux attach -t <sname>`

  进入名为`sname`的`session`，这个命令也适用于从终端直接进入`session`

* 关闭`session`：`tmux kill-session -t <sname>`

  杀死名为`sname`的`session`。快捷键`C+d`也可以关闭当前的`session`

* 切换`session`：`tmux switch -t <sname>`

  从当前的`session`进入名为`sname`的`session`

* 重命名`session`：`tmux rename-session -t <old-sname> <new-sname>`

  将名为`old-sname`的`session`改名成`new-sname`

* 查询`session`列表：`tmux ls`

  快捷键：`C+b s`，这个快捷键也可以用于查询各个`session`有的`window`和窗口下的`pane`



## `window`命令

* 新建`window`：`tmux new-window`

  名字为数字编号，快捷键`C+b c`

* 新建有名`window`：`tmux new-window -n <wname>`

  创建名为`wname`的`session`

* 切换`window`：`tmux select-window -t <wname>`

  切换到`wname`这个窗口

* 重命名`window`：`tmux rename-window <new-wname>`

  为当前窗口重命名

* 删除`window`：`tmux kill-window -t <wname>`

  这个命令没有`-t`之后的内容可以删除当前`window`；快捷键`C+b &`可以关闭当前的`window`



## `pane`命令

* 将当前`pane`拆成左右两个：`tmux split-window -h`

  快捷键：`C+b %`

* 将当前`pane`拆成上下两个：`tmux split-window`

  快捷键：`C+b "`

* 删除`pane`：`tmux kill-pane -t <pname>`

  这个命令没有`-t`之后的内容可以删除当前`pane`；快捷键`C+b x`可以关闭当前的`pane`

* 快速重新布局`pane`：快捷键：`C+b Space`

  每一次都会尝试一个布局
  
* 重调大小

  `C-b :`调出命令窗口，之后输入`resize-pane -`，跟`URDL`选择调整方向，然后输入数字表示调整大小

* 顺时针调整`pane`

  `C-b`后跟`C-o`将会顺时针调整窗口内容

* 显示`pane`编号并且快速跳转

  `C-b q`会显示所有的`pane`的编号，之后输入编号就可以直接跳转相应的`pane`



## 窗格命令

`C-b`（`ctrl+b`）是“起手式”，任何快捷键都需要先键入这个命令，再进行操作

`C-b c`表示新建一个窗口，在底下可见

`C-b %`表示将当前窗口分割成左右两部分

`C-b "`表示将当前窗口分割成上下两部分

`C-b kjhl`分别表示将光标移往四个位置的窗口

`C-b &`表示关闭当前窗口

`C-b {`当前窗格和上一个窗格交换位置

`C-b }`当前窗格和下一个窗格交换位置

`C-b :`可以调出命令窗口，之后输入`resize-pane -[DIR] x`可以将当前窗口向`DIR`方向移动`x`







# `vim`

可以见图片了解一些用法

## 配置文件所在地

`~/.vimrc`



## 配置`vim`作为`manpage`的阅读器

只要在`.vimrc`里添加：

```shell
runtime! ftplugin/man.vim
```

即可，之后在`runtime`的`vim`里输入`:Man <>`就可以阅读`ManPage`了

这么直接调用的`manpage`往往占页面比比较小，可以用`:res 90`来调整



## 保存、退出、进入模式等

`:w`可以保存

`:wq`就是退出了

`:q!`强制退出，不保存（交换文件都不会存在）

`i`进入插入模式，光标位置不变

`a`进入插入模式，光标在当前位置之后

`I`进入插入模式，光标位置到行首

`A`进入插入模式，光标在当前行最后



## 复制粘贴、内容移动

`vim`内复制：

`v`进入`visual`模式，然后`hjkl`选择复制的区间，然后输入`y`，就可以了

`yy`可以复制整行

但是，如果是需要往`vim`外复制，则需要输入`"+y`



`vim`内剪切

`v`进入`visual`模式，然后`hjkl`选择复制的区间，然后输入`d`，就可以了

但是，如果是需要往`vim`外复制，则需要输入`"+d`

可以`Ctrl+v`进入`visual block`模式，这种模式对于“删除多行的行首注释”很有用



`p`可以在当前位置之后黏贴，`P`则是在当前位置之前黏贴

如果不想要用`vim`自带的格式化，可以用`:set paste`来设置成粘贴模式，之后要用`:set nopaste`来设置成非粘贴模式

也可以用`"+p`来粘贴，这么做格式似乎并不会乱



`:m +x`当前行下移动`x`行

`:m -x`当前行上移动`x`行



## 删除内容

`:d`可以删除从当前行开始的一些行



## 查找与跳转

`:n`可以直接去`n`行

`o`可以在当前行下面插入一行并且进入`insert`模式，`O`则是在上面插入

`0`或者`^`可以跳转到当前行首

`$`可以跳到当前行尾，`n$`可以跳到`n`行后的行尾

`w`跳转到下一个单词，`W`跳到下一个空字符之后的单词首

`b`跳转到上一个单词，`B`跳到上一个空字符之后的单词首



`:%s#<s1>#<s2>#<tr>`：用`s2`替换`s1`，在`<tr>`范围内（如果是本文件就是`g`）



`:/<Content>`：全文查找`Content`这个单词

`gd`可以查找并高亮光标所在的单词

查找类的都是通过`n`查找下一个、`N`查找上一个，输入`:noh`来关闭查找的高亮



`C-u`向上翻半页

`C-d`向下翻半页

`H`去往屏幕顶行

`L`去往屏幕底行



## 重做和撤销重做

`u`是重做

`C-r`是撤销重做



## 折叠

`za`折叠/撤销折叠当前语义块

这玩意有个问题，如果在被折叠的语意块之前输入`{}`这种会自动格式化的东西，折叠就会被展开



## 缩进调节

`$NUM`+`>>`可以调节当前行的缩进向右`$NUM`个`tab`

`$NUM`+`<<`是向左

如果想要多行一起调的话，可以先在`VISUAL`模式下选中多行，然后调节



## 界面

[界面颜色配置](https://blog.csdn.net/u013595419/article/details/54898490)







# `Git`

## `Git`键仓库方式



## `NEMU`

本节课的`Git`设置

```shell
git config --global user.name "19211332-ChrisLee"
git config --global user.email "19211332@bjtu.edu.cn"
git config --global cor.editor vim
git config --global color.ui true
```



# `GDB`

[参考网页](https://blog.csdn.net/awm_kar98/article/details/82840811)

## 常用命令



### 一览

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



### `list`











# `GCC`

[C/C++专题—gcc g++ 参数详解](https://zhuanlan.zhihu.com/p/104197249)





# `Make`

`make -j4`可以用4核编译

一篇[知乎文章](https://zhuanlan.zhihu.com/p/47390641)



# `valgrind`

这玩意`CS61C`介绍了，这里就先记一下简单用法，之后再学怎么深入使用



`valgrind $filepath`可以对可执行文件做分析

`--leak-check=full`可以增加内存检查



