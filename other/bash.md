# Bash

## Shell

Shell 这个词有多种含义:

1. Shell 是一个程序，提供一个与用户对话的环境。接收到用户输入的命令，将命令送入操作系统执行，并将结果返回给用户。
2. Shell 是一个命令解释器，解释用户输入的命令。
3. Shell 是一个工具箱，提供了各种小工具。

Shell 有很多种，只要能给用户提供命令行环境的程序，都可以看作是 Shell：

- Bourne Shell（sh）
- Bourne Again shell（bash）
- C Shell（csh）
- Z Shell（zsh）

## 基本语法

分号（;）是命令的结束符，使得一行可以放置多个命令。
Bash 还提供两个命令组合符&&和||，&&表示如果前一命令运行成功，则继续运行随后命令。||则相反。

type命令用来判断命令的来源。

Ctrl + u：从光标位置删除到行首。
Ctrl + k：从光标位置删除到行尾。
Ctrl + a：光标位置移到行首。
Ctrl + e：光标位置移到行尾。

## Bash变量

环境变量是 Bash 环境自带的变量，进入 Shell 时已经定义好了，可以直接使用。它们通常是系统定义好的。
env命令或printenv命令，可以显示所有环境变量。（包括PATH，HOME，PWD，UID等）。

查看单个环境变量的值，可以使用printenv命令或echo命令。如`printenv PATH`，`echo $PATH`。

自定义变量是用户在当前 Shell 里面自己定义的变量，仅在当前 Shell 可用。

set命令可以显示所有变量（包括环境变量和自定义变量），以及所有的 Bash 函数。

创建变量例子：

```bash
a=z                     # 变量 a 赋值为字符串 z
b="a string and $a"     # 变量值可以引用其他变量的值
c=$(ls -l foo.txt)      # 变量值可以是命令的执行结果
d=$((5 * 7))            # 变量值可以是数学运算的结果
```

读取变量的时候，直接在变量名前加上$就可以了。

一些特殊变量：

- $?为上一个命令的退出码，用来判断上一个命令是否执行成功。返回值是0，表示上一个命令执行成功；如果不是零，表示上一个命令执行失败。
- $$为当前 Shell 的进程 ID。

declare命令可以声明一些特殊类型的变量，为变量设置一些限制。
`declare OPTION VARIABLE=value`，其中一些主要参数（OPTION）如下：

- a：声明数组变量。
- i：声明整数变量。
- r：声明只读变量。
- x：该变量输出为环境变量。

let命令声明变量时，可以直接执行算术表达式。

((...))语法可以进行整数的算术运算。

## 字符串操作

字符串提取子串：
`${varname:offset:length}`

字符串头部的模式匹配:

- 如果 pattern 匹配变量 variable 的开头，删除最短匹配（非贪婪匹配）的部分，返回剩余部分：`${variable#pattern}`
- 如果 pattern 匹配变量 variable 的开头，删除最长匹配（贪婪匹配）的部分，返回剩余部分：`${variable##pattern}`

字符串尾部的模式匹配:

- `${variable%pattern}`
- `${variable%%pattern}`

任意位置的模式匹配:

- 如果 pattern 匹配变量 variable 的一部分，最长匹配（贪婪匹配）的那部分被 string 替换，但仅替换第一个匹配：`${variable/pattern/string}`

- 如果 pattern 匹配变量 variable 的一部分，最长匹配（贪婪匹配）的那部分被 string 替换，所有匹配都替换：`${variable//pattern/string}`

## Bash行操作

Bash 内置了 Readline 库，具有这个库提供的很多“行操作”功能，比如命令的自动补全，可以大大加快操作速度。这个库默认采用 Emacs 快捷键，也可以改成 Vi 快捷键：
`set -o vi`
改回 Emacs 快捷键：
`set -o emacs`

如果想永久性更改编辑模式（Emacs / Vi），可以将命令写在~/.inputrc文件，这个文件是 Readline 的配置文件。
`set editing-mode vi`

随后介绍的快捷键都属于 Emacs 模式。

光标移动：

- Ctrl + a：移到行首。
- Ctrl + b：向行首移动一个字符，与左箭头作用相同。
- Ctrl + e：移到行尾。
- Ctrl + f：向行尾移动一个字符，与右箭头作用相同。
- Alt + f：移动到当前单词的词尾。
- Alt + b：移动到当前单词的词首。

编辑操作：

- Ctrl + d：删除光标位置的字符（delete）。
- Ctrl + w：删除光标前面的单词。
- Ctrl + t：光标位置的字符与它前面一位的字符交换位置（transpose）。
- Alt + t：光标位置的词与它前面一位的词交换位置（transpose）。
- Alt + l：将光标位置至词尾转为小写（lowercase）。
- Alt + u：将光标位置至词尾转为大写（uppercase）。
- Ctrl + k：剪切光标位置到行尾的文本。
- Ctrl + u：剪切光标位置到行首的文本。
- Alt + d：剪切光标位置到词尾的文本。
- Alt + Backspace：剪切光标位置到词首的文本。
- Ctrl + y：在光标位置粘贴文本。

history命令能显示操作历史，即.bash_history文件的内容。
history -c可以清除操作历史。

## 目录堆栈

`cd -`命令可以返回前一次的目录。

如果希望记忆多重目录，可以使用pushd命令和popd命令。它们用来操作目录堆栈。pushd命令的用法类似cd命令，可以进入指定的目录。

第一次使用pushd命令时，会将当前目录先放入堆栈，然后将所要进入的目录也放入堆栈。以后每次使用pushd命令，都会将所要进入的目录，放在堆栈的顶部。

popd命令不带有参数时，会移除堆栈的顶部记录，并进入新的栈顶目录。

dirs命令可以显示目录堆栈的内容，一般用来查看pushd和popd操作后的结果。

## Bash 脚本

脚本的第一行通常是指定解释器。这一行以`#!`字符开头，这个字符称为 Shebang，所以这一行就叫做 Shebang 行。

`#!`后面就是脚本解释器的位置，Bash 脚本的解释器一般是/bin/sh或/bin/bash。

如果没有 Shebang 行，就只能手动将脚本传给解释器来执行。如`bash script.sh`。

env命令总是指向/usr/bin/env文件。`#!/usr/bin/env NAME`这个语法的意思是，让 Shell 查找$PATH环境变量里面第一个匹配的NAME。
`/usr/bin/env bash`的意思就是，返回bash可执行文件的位置，前提是bash的路径是在$PATH里面。

脚本参数：

- $0：脚本文件名，即script.sh。
- $1~$9：对应脚本的第一个参数到第九个参数。
- $#：参数的总数。
- $@：全部的参数，参数之间使用空格分隔。
- $*：全部的参数，参数之间使用变量$IFS值的第一个字符分隔，默认为空格，但是可以自定义。

`getopts`命令用在脚本内部，可以解析复杂的脚本命令行参数。

命令执行结束后，会有一个返回值。0表示执行成功，非0（通常是1）表示执行失败。环境变量`$?`可以读取前一个命令的返回值。

`source`命令用于执行一个脚本，通常用于重新加载一个配置文件。
source命令最大的特点是在当前 Shell 执行脚本，不像直接执行脚本时，会新建一个子 Shell。所以，source命令执行脚本时，不需要export变量。
source有一个简写形式，可以使用一个点（.）来表示。如`. ~/.bashrc`

`alias`命令用来为一个命令指定别名，如`alias ll='ls -l'`。
一般来说，都会把常用的别名写在~/.bashrc的末尾。unalias可以解除别名。
