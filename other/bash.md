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

`read`命令：

```bash
echo -n "Please input some text > "
read text
echo "Your input text：$text"
```

read可以接受用户输入的多个值：`read a b c`。

read命令读取的值，默认是以空格分隔。可以通过自定义环境变量IFS（Internal Field Separator），修改分隔标志。

## 条件判断

`if`条件判断结构：

```bash
if commands; then
  commands
elif commands; then
  commands...
else
  commands
fi
```

if结构的判断条件，一般使用test命令，有三种形式：

1. `test expression`
2. `[ expression ]` (其实`[`也是一个特殊命令，这也是为什么`[`后面要有空格。)
3. `[[ expression ]]` (第三种形式还支持正则判断)

以下表达式用来判断文件状态：（具体见<https://wangdoc.com/bash/condition.html#%E6%96%87%E4%BB%B6%E5%88%A4%E6%96%AD>)

- [ -a file ]：如果 file 存在，则为true。
- [ -b file ]：如果 file 存在并且是一个块（设备）文件，则为true。
- [ -c file ]：如果 file 存在并且是一个字符（设备）文件，则为true。
- [ -d file ]：如果 file 存在并且是一个目录，则为true。
- [ -e file ]：如果 file 存在，则为true。
- [ -f file ]：如果 file 存在并且是一个普通文件，则为true。
- ...

以下表达式用来判断字符串：

- [ string ]：如果string不为空（长度大于0），则判断为真。
- [ -n string ]：如果字符串string的长度大于零，则判断为真。
- [ -z string ]：如果字符串string的长度为零，则判断为真。
- [ string1 = string2 ]：如果string1和string2相同，则判断为真。
- [ string1 == string2 ] 等同于[ string1 = string2 ]。

下面的表达式用于判断整数：

- [ integer1 -eq integer2 ]：如果integer1等于integer2，则为true。
- [ integer1 -ne integer2 ]：如果integer1不等于integer2，则为true。
- [ integer1 -le integer2 ]：如果integer1小于或等于integer2，则为true。
- [ integer1 -lt integer2 ]：如果integer1小于integer2，则为true。
- [ integer1 -ge integer2 ]：如果integer1大于或等于integer2，则为true。
- [ integer1 -gt integer2 ]：如果integer1大于integer2，则为true。

Bash 还提供了((...))作为算术条件，进行算术运算的判断。

`case`结构：

```bash
case expression in
  pattern )
    commands ;;
  pattern )
    commands ;;
  ...
esac
```

## 循环

`while` 循环：

```bash
while condition; do
  commands
done
```

`until` 循环：与while循环恰好相反，只要不符合判断条件，就不断循环执行指定的语句。

```bash
until condition; do
  commands
done
```

`for...in`循环用于遍历列表的每一项：

```bash
for variable in list
do
  commands
done
```

`for`循环还支持 C 语言的循环语法：

```bash
for (( expression1; expression2; expression3 )); do
  commands
done
```

## 函数

Bash 函数定义的语法有两种：

```bash
fn() {
  # codes
}

function fn() {
  # codes
}
```

Bash 函数体内直接声明的变量，属于全局变量，整个脚本都可以读取。这一点需要特别小心。函数里面可以用local命令声明局部变量。

## 数组

数组可以采用逐个赋值的方法创建：

```bash
array[0]=val
array[1]=val
```

也可以采用一次性赋值的方式创建：

```bash
ARRAY=(
  value1
  value2
  value3
)
```

@和*是数组的特殊索引，表示返回数组的所有成员。

数组长度：

```bash
${#array[*]}
${#array[@]}
```

## set命令

Bash 执行脚本时，会创建一个子 Shell。这个子 Shell 就是脚本的执行环境，Bash 默认给定了这个环境的各种参数。`set`命令用来修改子 Shell 环境的运行参数，即定制环境。

- `set -u`：执行脚本时，如果遇到不存在的变量，Bash 默认忽略它。
- `set -x` 用来在运行结果之前，先输出执行的那一行命令。
- `set -e` 使得脚本只要发生错误，就终止执行。（如果脚本里面有运行失败的命令（返回值非0），Bash 默认会继续执行后面的命令。）
- `set -o pipefail` set -e 有一个例外情况，就是不适用于管道命令。set -o pipefail用来解决这种情况，只要一个子命令失败，整个管道命令就失败，脚本就会终止执行。
- ...

set命令的几个参数，一般都放在一起使用，如：

```bash
set -euxo pipefail
```

## mktemp 命令

Bash 脚本有时需要创建临时文件或临时目录。常见的做法是，在/tmp目录里面创建文件或目录，使用mktemp命令是最安全的做法，其支持唯一文件名和清除机制。

直接运行mktemp命令，就能生成一个临时文件。

- -d参数可以创建一个临时目录。
- -p参数可以指定临时文件所在的目录。
- -t参数可以指定临时文件的文件名模板。

`trap`命令用来在 Bash 脚本中响应系统信号。

`trap 'rm -f "$TMPFILE"' EXIT`命令中，脚本遇到EXIT信号时，就会执行`rm -f "$TMPFILE"`。

## Session

用户每次使用 Shell，都会开启一个与 Shell 的 Session。Session 有两种类型：登录 Session 和非登录 Session。

**登录 Session** 一般进行整个系统环境的初始化，启动的初始化脚本依次如下：

- /etc/profile：所有用户的全局配置脚本。
- /etc/profile.d目录里面所有.sh文件
- ~/.bash_profile：用户的个人配置脚本。如果该脚本存在，则执行完就不再往下执行。
- ~/.bash_login：如果~/.bash_profile没找到，则尝试执行这个脚本（C shell 的初始化脚本）。如果该脚本存在，则执行完就不再往下执行。
- ~/.profile：如果~/.bash_profile和~/.bash_login都没找到，则尝试读取这个脚本（Bourne shell 和 Korn shell 的初始化脚本）。

**非登录 Session** 是用户进入系统以后，手动新建的 Session，这时不会进行环境初始化。比如，在命令行执行bash命令，就会新建一个非登录 Session。

非登录 Session 的初始化脚本依次如下：

- /etc/bash.bashrc：对全体用户有效。
- ~/.bashrc：仅对当前用户有效。

`~/.bashrc`通常是最重要的脚本。非登录 Session 默认会执行它，而登录 Session 一般也会通过`~/.bash_profile`调用执行它。

`~/.bash_logout`脚本在每次退出 Session 时执行，通常用来做一些清理工作和记录工作，比如删除临时文件。
