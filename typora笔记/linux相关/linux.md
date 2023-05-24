# linux相关操作

## 13个基本命令和Shell脚本编程

```bash
ls 		# 列出当前目录所有文件和目录
ls -l 	# 更加详细 == ll
ls -a 	# 展示隐藏文件
ls -la 

cd 			# 进入目录
cd .. 		# 进入上一层目录
cd ../.. 	# 进入上二层目录
cd - 		# 返回刚才的目录

pwd 	# 打印当前路径

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







