

### `makefile`介绍(h3)



#### makefile`编写规则 

```makefile
target... : prerequisites...
	recipe
	...
	...
```

`target` : 可以是一个目标文件(`object file`)，也可以是一个可执行文件，还可以是一个标签(label)

`prerequisites`: 生成该target所依赖的文件

`recipe`：要执行的命令(`shell`)

**`prerequisites`中如果 有一个以上的`target`文件要新的话，`recipe`所定义的命令就会被执行**

示例1：

```makefile
# 3个头文件，8个c文件
edi : main.o kbd.o command.o display.o \ # \ 是换行符，继续符号
		insert.o search.o files.o utils.o
	cc -o edit main.o kbd.o command.o display.o \
			insert.o search.o files.o utils.o


main.o : main.c defs.h
	cc -c main.c
kbd.o : kbd.c defs.h
	cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o			
```

完毕之后 可以用`make clearn`清除不必要的文件



#### make`是如何工作的

**输入`make`后：**

1. 在当前目录找文件`Makefile/makefile`
2. 找到`makefile`中的第一个目标文件`edit`，将其作为最终目标
3. 如果`edit`不存在，或其依赖文件修改时间更新，执行`edit`的生成命令
4. 如果依赖文件不存在，则找到对应的规则生成依赖文件

**make只会针对 依赖条件报错，定义的命令错误，make不会反馈**





#### 在`makefile`中使用变量

对于示例 1而言，不易维护，添加.o文件需要更改多个地方,使用变量进行优化

```makefile
objects = main.o kbd.o display.o \
			insert.o search.o files.o utils.o

edit : $(objects)
	cc -o edit $(objects)
	
main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit $(objects)
```

此时如果 有新的.o文件，只需在`objects`中添加即可



#### 让`make`自动推导

`make`隐式规则之一自动推导：遇到.o文件，自动把其.c文件回到依赖，同时会自动推导出他们间的命令

示例3：

```makefile
objects = main.o kbd.o command.o display.o \
			insert.o search.o files.o utils.o
edit : $(objects)
	cc -o edit $(objucts)

main.o : defs.h
kbd.o : defs.h command.h
command.o : defs.h command.h
display.o : defs.h buffer.h
insert.o : defs.h buffer.h
search.o : defs.h buffer.h
files.o : defs.h buffer.h command.h
utils.o : defs.h

.PHONY : clean # .PHONY表示 clearn是个伪目标文件
clean :
    -rm edit $(objects) # - 表示也许有些文件会出现 问题，不管继续 
```





#### makefile`有什么

1. 显示规则 ：目标文件，依赖文件，命令
2. 隐式规则 ：自动推导的功能
3. 变量的定义
4. 指令
5. 注释： #

*`makefile`中的命令必须以`tab`键开始*









#### 包含其他`makefile`

```makefile
inlcude <filenames>...

# 如：
include foo.make *.mk $(bar)
```

关于找寻`include`包含的其他`makefile`:

1. 首先寻找当前路径

2. 如果 make执行时，有`-I `或`--include-dir`参数，会按指定目录寻找（不建议使用）

3. 接下来按顺序寻找目录：`<prefix>/include`一般是 `/usr/local/bin`,`/usr/gnu/include`,`/usr/local/include`,`/usr/include`

   

环境变量`.INCLUDE_DIRS`包含当前`make`会寻找的目录列表

如果想让`make`不理会无法读取的文件：

```makefile
-include <filenames>...
```

关于环境变量：`MAKEFILES`会对所有的`makefile`文件有效（不建议使用，可以用来查错）



#### `make`的工作方式

1. 读入所有的`makefile`
2. 读入`Include`和其他`makefile`
3. 初始化文件中的变量
4. 指导隐式规则 ，分析所有规则 
5. 为所有目标文件创建依赖关系链
6. 根据依赖关系，决定哪些目标重新生成
7. 执行生成命令

变量只有在要使用时，才会在内部展开







### 书写规则 

例 ：

```makefile
foo.o : foo.c defs.h # defs.h 是 foo.c  的 头文件
#１ foo.c 依赖于 defs.h foo.c 如果 defs.h foo.c 其中一个更新或foo.c不存在，生成foo.o文件
```

#### 在规则 中使作通配符

* `*`：表示任意多个字符
* `?`：表示一个任意的单个字符 
* `~`：在文件名中表示目录 

#### 文件搜寻

```makefile
# 指定找寻依赖关系文件
1. VPATH变量
VPATH = src:../headers # 目录由：分隔，当前目录优先级最高

2.vpath关键字
vpath <pattern> <directories>  # 为符合<pattern>的文件指定搜索路径为<dierctories>
vpath <pattern>				  # 清除符合模式<pattern>的文件的搜索目录
vpath						 # 清除所有设置好的文件搜索目录

# 如：
vpath %.h ../headers	# 如果某文件没有在当前目录找到，到../headers目录下找.h结尾的文件

vpath %.c foo
vpath %
vpath %.c bar # 表示.c结尾的文件先后在foo,blish.bar目录找寻

vpath %.c foo:bar
vpath % blish  # 表示.c结尾的文件，先后在foo,bar.blish目录的找寻
```



#### 伪目标

```makefile
# 如：clearn 是一个伪目标
clean: 
	rm *.o temp 
# 为应对clean 和文件重名的情况：用.PHONY显式指明伪目标
.PHONY : clean 
clean:
	rm *.o temp
	

# 利用伪目标生成多个可执行文件
all : prog1 prog2 prog3
.PHONY : all

prog1 : prog1.o utils.o
    cc -o prog1 prog1.o utils.o

prog2 : prog2.o
    cc -o prog2 prog2.o

prog3 : prog3.o sort.o utils.o
    cc -o prog3 prog3.o sort.o utils.o

```



#### 多目标

```makefile
# 多个目标同时依赖一个文件，但执行命令不是同一个
bigoutput littleoutput : text.g
	generate text.g -$(subst output,,$@) > $@
# 等价于：
bigoutput : text.g
    generate text.g -big > bigoutput
littleoutput : text.g
    generate text.g -little > littleoutput
```

#### 静态模式

```makefile
<targets ...> : <target-pattern> : <prereq-patterns ...>
	<commands>
	...
	
# 如：
objects = foo.o bar.o
all : $(objects)

$(objects) : %.o %.c
	$(cc) -c $(CFLAGS) $< -o $@ # $<表示第一个依赖文件，$@表示目标文件

# 再如：
files = foo.elc bar.o lose.o

$(filter %.o, $(files)) : %.o %.c
	$(cc) -c $(CFLAGS) $< -o $@
$(filter %.elc,$(files)) : %.elc %.el
	emacs -f batch-byte-compile $<

```

#### 自动生成依赖性

```makefile
# 自动寻找.c头文件并生成.o依赖关系：
gcc -MM main.c
-> main.o : main.c defs.h

# 在makefile中利用模式规则生成.d文件，.d文件存放对应.c文件的依赖关系
%.d : %.c
	@set -e; rm -f $@; \
	$(gcc) -MM $(CPPFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' <$@.$$$$ > $@; \
	rm -f $@.$$$$
```

