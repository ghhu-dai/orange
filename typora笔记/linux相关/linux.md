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







