# linux命令操作



## 基本命令和Shell脚本编程

```bash
ls 		# 列出当前目录所有文件和目录
ls -l 	# 更加详细 == ll
ls -a 	# 展示隐藏文件
ls -d  	# 显示目录
ls -la 

cd 			# 进入目录
cd .. 		# 进入上一层目录
cd ../.. 	# 进入上二层目录
cd - 		# 返回刚才的目录

pwd 	# 打印当前路径

date	# 显示 日期与时间
cal		# 显示日历
bc		# 计算器 ，默认输出整数
	scale=3		# 调整输出精度
	quit		# 退出 bc

cat		# 查看文件内容
head 	# 想看文件开头内容
head --lines=2 README.md # 查看文件开头二行
tail	# 查看尾部内容 ,也有--lines的用法
less	# 查看全文
more	# 和less差不多，在linux中只有向下翻页 
file 	# 查看文件格式等信息
where gcc #查看gcc的位置

echo	# 打印


# shell的for循环
# 如当前目录有week01~week05等文件，批量将week修改为chapter，序号不变
for ff in week??
> do 
> mv $ff chapter${ff#week} # '#'去头操作，'%'去尾操作
> done
# 没有撤销操作要小心，可以先echo打印看看，再操作
```



 



## 快捷键/热键

```bash
1. [tab] # 命令补全，文件补全
	ca[tab][tab] # 显示所有ca开关的命令
	
2. [ctrl]+c # 终止当前指令
3. [ctrl]+d # 键盘输入结束 ，= exit

4. [ctrl]+a # 在命令行中光标至开头
5. [ctrl]+e # 在命令行中光标至结尾
5. [ctrl]+u # 清空当前命令行

6. [shift]+[pageup],[shift]+[pageup] # 翻页

	
```



## 帮助说明 `man` `help`,`info`

1. 知道某个指令，忘记了相关选项与参数，使用`--help`查询 
2. 有任何不知道的指令或文件格式，使用`man`和`info`来查询 
3. 关于一些服务的说明文档，在`/usr/share/doc`

```bash
command --help # 查看命令的帮助文档
man date 	# 更详细的内容说明	 
进入man page 页面后的操作：
	[空格] # 下翻 
	[page up],[page down] # 翻页
	[home]	# 去到第一页
	[end]	# 去到最后一页
	/string # 向下搜索string 
	?string # 向上搜索string  // 利用?或/搜索字符串时，用n,N向下向上搜索
	q	# 离开
	

```

| man page页面的数字标号说明 |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| 数字 标号                  | 代表内容                                                     |
| **1**                      | **用户在`shell`环境下可以操作的指令或执行文件**              |
| 2                          | 系统核心可呼叫的函数与工具等                                 |
| 3                          | 一些常用的函数或函数 库，大部分为c的函数库                   |
| 4                          | 装置文件的说明，通常在/dec下的文件                           |
| **5**                      | **配置文件或者 是某些文件的格式**                            |
| 6                          | 游戏(games)                                                  |
| 7                          | 惯例与协议等，例如linux文件系统，网络协议，`ASCII code`等等的说明 |
| **8**                      | **系统管理员可用的管理的指令**                               |
| 9                          | 和`kernel`有关的文件                                         |



| man page页面的内容说明 |                                                |
| ---------------------- | ---------------------------------------------- |
| NAME                   | 简短的指令数据 名称说明                        |
| SYNOPSIS               | 语法说明                                       |
| DESCRIPTION            | 较为完整的说明                                 |
| OPTIONS                | 针对 SYNOPSIS部分可选参数的说明                |
| COMMANDS               | 程序执行时可执行的选项                         |
| FILES                  | 这个程序或数据所使用或参考或连接到的某些文件   |
| SEE ALSO               | 可以参考的，跟这个 指令或数据 有相关的其他说明 |
| EXAMPLE                |                                                |
|                        |                                                |



关于`info page`

| 按键        | 进行工作                                    |
| ----------- | ------------------------------------------- |
| 空格键      | 下翻一页                                    |
| [page down] | 下翻一页                                    |
| [page up]   | 上翻一页                                    |
| [tab]       | 在node之间移动，有node的地方，通常会以*显示 |
| [enter]     | 当光标在node上面时，按下enter可以进入该node |
| b           | 光标移动到该info画面当中的第一个node处      |
| e           | 移动光标到该info画面当中的最后一个node片    |
| n           | 前往下一个node处                            |
| p           | 前往上一个node处                            |
| u           | 向上移动一层                                |
| s(/)        | 在info page 当中进行搜寻                    |
| h,?         | 显示 求助选单                               |
| q           | 结束 这次的info page                        |







## SSH

**安装ssh服务端**

```bash
sudo apt updata
sudo apt install openssh-server -y
sudo systemctl status ssh # 查看状态
sudo ufw allow ssh # 应对防火墙
```



**连接**

```bash
ssh username@ip  # 基本ssh 连接方法

# 给ip地址取别名，10.22.75.177是ip地址
sudo vim /etc/hosts -> 10.22.75.177 name 

ssh username@ipname
```

进一步的简化

```bash
vim ~/.ssh/config
	Host l1
	HostName linx
	Port 22
	User dai
	
# 连接
ssh l1
```



## 关于`linux`正确的关机方法

1. 观察系统的使用状态

   ```bash
   who # 查看在线使用人员
   netstat -a # 查看网络的联机状态
   ps -aux # 查看背景执行的程序 
   ```

2. 通知在线使用者关机的时刻

   ```bash
   # 使用shutdown的特别指令，给在线的使用者一些时间结束他们的工作
   ```

3. 正确的关机指令使用

   ```bash
   shutdown , reboot
   ```

其他相关指令：

1. `sync`：

   ```bash
   sync # 数据同步写入磁盘
   ```

   































# vim

## 基本操作/vi

### **移动光标**：

1. h(左移) 	j(下移)	k(上移)	l(右移)
2. 多次移动: `30j`向下移动30次
3. 屏幕向下/向上移动一页 `[ctrl]+[f]  /  [ctrl] + [b]`半页是`u/d`
4. 移动到行首/行尾：`0,$`
5. 移动到第一行,最后一行`gg,G`
6. 移动到n行：`nG`
7. 向下移动n行：`n[enter]`

### **搜索与代替**

1. 向光标之下寻找一个名为`word`的字符串：`/word`
2. 向光标之上寻找一个名为`word`的字符串：`?word`
3. 继续向下搜索：`n`，继续 向上搜索：`N`
4. 在某个范围内找到字符串并替换：`:n1,n2s/word1/word2/g`
   例 ：从第一列到最后一列寻找word1字符串，并将该字符串取代为word2，在取代前提示
   `:1,$s/word1/word2/gc` 

### **删除，复制，粘贴 **

1.  向前删除：`X(当前光标覆盖的不会删除)`,向后删除：`x(删除当前光标所覆盖)`,多删`nx/nX`

2. 删除光标所在行：`dd`

3. 删除从光标开始的n行：`nd`

4. 删除光标所在行到第n行的数据 ：`dnG`

5. 删除光标所在行到最后一行的数据 ：`dG`

6. 删除光标处到行首的字符 ：`d0`

7. 删除光标和到行尾的字符 ：`d$`

   ---

8. 复制光标所在行：`yy`

9. 复制光标向下的n行：`nyy`

10. 复制光标所在行到第n行的所有数据 :	`ynG`

11. 复制光标所在行到最后一行的数据 ：`yG`

12. 复制光标所在处到行首：`y0`

13. 复制光标所在处到行尾：`y$`

    

    ---

14. 粘贴到上一行：`P`,粘贴 到下一行：`p`

15. 将光标所在行与下一行合并：`J`，有空格分隔

16. 撤销操作：`u`,反撤销：`[ctrl]+r`（关于ctrl + r 其实我不太懂）

17. 重复操作：`.`

    

### **进入插入或取代的编辑模式**

1. 进入插入模式：`i(从目前光标处插入)，I(在目前所在行的第一个非空格符处开始插入，也就是在开头插入)`

2. 进入插入模式：`a(从光标所在的下一个字符处开始插入)，A(从光标所在行的最后一个字符处开始插入)`

3. 进入插入模式：`o(从下一行开始插入)，O(从上行开始插入)`

4. 进入取代模式：`r(只会取代光标所在的字符一次),R(一起取代光标所在的字符，直到[ESC])`

   

### **指令行命令**

1. 将编辑的数据另存新档：`:w [filename]`
2. 读入另一个文件的数据，加到当前行的后面：`:r [filename]`
3. 将n1到n2的数据 存储到`filename`文件：`:n1,n2 w [filename]`
4. 执行`shell`命令`:!command`
5. 显示 行号，取消行号：`set nu / set nonu`

## 多文件编辑

1. 编辑下一个文件：`:n`，编辑上一个文件：`:N`

2. 列出vim开启的所有文件：`:files`

   

   

## 多窗口编辑

1. 开户一个新的窗口：`sp(垂直方向)，vs（水平方向）`
2. 切换窗口：`[ctrl]+w+j/k 向下切换窗口/向上切换窗口`



## vim的补全功能

1. 根据当前文件的数据为基础作补全：`[ctrl]+x -> [ctrl]+n`

2. 以扩展名作业语法补充，以vim内建的关键词予以补全：`[ctrl]+x -> [ctrl]+o`

   

## vim环境设定与记录：`~/.vimrc`, `~/.viminfo`

1. 设定/取消行号：`set nu / set nonu`
2. 高亮度搜索：`set hlsearch / set nohlsearch`
3. 自动缩排：`set autoindent / set noautoindent`
4. 语法检验：`syntax on/off`
5. 设置颜色色调：`set bg=dark/light`















# git相关：
### 为git添加key
1. 设置git的username和useremail
```Git
$ git config --global user.name "dai_linux"
$ git consig --global user.email "dai_linux.email"
```
2. 生成密钥

    `ssh-keygen -t rsa -C "dai_linux.email"`
3. 复制公钥在github中添加即可







