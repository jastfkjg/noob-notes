# makefile

一个工程中的源文件不计数，其按类型、功能、模块分别放在若干个目录中。

makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至于进行更复杂的功能操作。

编译和链接：

编译器需要的是语法的正确，函数与变量的声明的正确。

链接时需要明显地指出中间目标文件名，这对于编译很不方便，所以，我们要给中间目标文件打个包，在Windows下这种包叫库文件（Library File)，也就是.lib 文件，在UNIX下，是Archive File，也就是.a文件。

makefile规则：
```
target : prerequisites
        command
```

make会比较target文件和prerequisites文件的修改日期，如果prerequisites文件的日期要比target文件的日期要新，或者target不存在的话，那么，make就会执行后续command的命令。

例：
```
main.o : main.c defs.h
         cc -C main.c
```

make是如何工作的：
1. make会在当前目录下找名字叫Makefile或makefile的文件
2. 找文件中的第一个目标文件（target），并把这个文件作为最终的目标文件
3. 如果目标文件不存在，或是所依赖的后面的文件的修改时间更新，那么就会执行后面所定义的命令来生成目标文件
4. 如果目标文件所依赖的.o文件不存在，那么make会在当前文件中找目标为.o文件的依赖性，如果找到，则根据对应的依赖规则生成.o文件
5. 如果被依赖的文件找不到，那么make就会直接退出，并报错

为了makefile的易维护，在makefile中我们可以使用变量。如：

```
objects = main.o utils.o search.o files.o

target : $(objects)
         cc -o target $(objects)
```

如果有新的.o 文件加入，我们只需简单地修改一下objects 变量就可以了。

make可以自动推导文件以及文件依赖关系后面的命令，因此第一个例子可以简化为：
```
main.o : defs.h
```

每个Makefile中都应该写一个清空目标文件（.o和执行文件）的规则，这不仅便于重编译，也很利于保持文件的清洁。

```
clean:
    rm target $(objects)
```

在Makefile使用include关键字可以把别的Makefile包含进来，这很像C语言的#include，被包含的文件会原模原样的放在当前文件的包含位置。

make工作时大致执行步骤：
1. 读入所有的Makefile
2. 读入被include的其它Makefile
3. 初始化文件中的变量
4. 推导隐晦规则，并分析所有规则
5. 为所有的目标文件创建依赖关系链
6. 根据依赖关系，决定哪些目标要重新生成
7. 执行生成命令

Makefile文件中的特殊变量VPATH，如果没有指明这个变量，make只会在当前的目录中去找寻依赖文件和目标文件。如果定义了这个变量，那么，make就会在当前目录找不到的情况下，到所指定的目录中去寻找文件了。


在一些大的工程中，我们会把我们不同模块或是不同功能的源文件放在不同的目录中，我们可以在每个目录中都书写一个该目录的Makefile。

例如，我们有一个子目录叫subdir，这个目录下有个Makefile文件，来指明了这个目录下文件的编译规则。那么我们总控的Makefile可以这样书写：

```
subsystem:
        cd subdir && $(MAKE)
```

- make all:
    这个伪目标是所有目标的目标，其功能一般是编译所有的目标。
- make clean:
    这个伪目标功能是删除所有被make创建的文件。
- make install
    这个伪目标功能是安装已编译好的程序，其实就是把目标执行文件拷贝到指定的目标中去。
- make print
    这个伪目标的功能是例出改变过的源文件。
- make tar
    这个伪目标功能是把源程序打包备份。也就是一个tar文件。
- make dist
    这个伪目标功能是创建一个压缩文件，一般是把tar文件压成Z文件。或是gz文件。