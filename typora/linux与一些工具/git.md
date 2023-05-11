# 常用操作
## 拉取与配置
* 从git服务器拉取代码`git clone https:……`
* 配置开发者用户名和邮箱`git config user.name/user.email  ..`
---
## 分支相关
* 创建一个`daily/0.0.0`的日常开发分支`git branch daily/0.0.0`
* 重命名分支`git branch -m daily/0.0.0 daily/0.0.1`
* 查看当前项目分支列表`git brancyh`
* 删除`daily/0.0.1`分支`git branch -d daily/0.0.1`
* 切换到`daily/0.0.1`分支`git checkout daily/0.0.1`

---
## 文件相关
* 查看文件变动状态`git status`
* 将文件添加到暂存区`git add 文件名`
* 添加所有文件到暂存区`git add .`
* 提交文件变动到版本库`git commid -m "这里写提交描述"`
---
## 远程相关
* 将本地代码推送到服务器`git push origin daily/0.0.1`
   
   * origin指代的是当前的git服务器地址（远程仓库地址）
   
* `remote`链接

   ```bash
   git remote -v ; # 查看当前 仓库关联的远程仓库相关信息
   git remote add <remote_name> <remote_url>; # 添加远程仓库
   git remote rename <old_name> <new_name> # 重命名远程仓库
   git remote remove/rm <remote_name> # 删除远程仓库
   
   ```

* 将服务器上的代码拉取到本地`git pull origin daily/0.0.1`

* 查看版本提交记录`git log`

* 为项目标记里程碑
```
git tag publis/0.0.1
git push origin publish/0.0.1
```

* 利用配置文件`.gitignore`设置不需要推送到服务器的文件，`.gitignore`不是git命令，只是一个文件
```c
touch .gitignore
// 如果.gitignore的文件内容如下：
demo.html
build/
// 则说明Git将忽略demo.html文件和build/目录
```
