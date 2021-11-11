# 学习文档

## 				----Git、GitHub、Gitee、GitLab

​				

## 目标：

------------------------------Git-----------------------------------------

Git  介绍  分布式版本控制工具  VS  集中式版本控制工具

Git  安装 

Git  命令 

Git分支	分支特性	分支转换	分支合并	代码合并及冲突解决

------------------------------GitHub-----------------------------------------

创建远程库

代码推送

代码拉取

代码克隆

SSH免密登录

Idea集成GitHub



------------------------------Gitee-----------------------------------------

码云创建远程库

Idea集成Gitee码云

码云连接GitHub进行代码的复制和迁移



------------------------------GitLab-----------------------------------------

GitLab服务器的搭建和部署

Idea集成GitLab



------------------------------End-----------------------------------------

## 第一章	Git	介绍

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。
- **远程库：**基于网络服务器的的远程代码仓库。

工作机制：

![image-20211109180946698](C:\Users\wendo\AppData\Roaming\Typora\typora-user-images\image-20211109180946698.png)

**代码托管中心：**

​		基于网络服务器的的远程代码仓库，一般称为远程库。

​		**局域网：**

​				GitLab

​		**互联网：**

​				GitHub（国外）

​				Gitee	码云	（国内）



## 第二章	Git安装

## 第三章	Git常用命令

|命令|作用|
|----|----|
|git config --global user.name "username"|设置用户签名|
|git config --global user.email "xxxx@outlook.com"|设置用户签名|
|git init|初始化本地库|
|git status|查看本地库状态|
|git add <文件名>|添加到暂存区|
|git commit -m "日志信息"|提交的本地库|
|git reflog|查看历史记录|
|git reset --hard <版本号>|版本穿梭|

### 3.1	设置用户签名

```shell
# 基本语法
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com

# 保存路径
C:\Users\username\.gitconfig

# .gitconfig文件内容：
[filter "lfs"]
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
	required = true
	clean = git-lfs clean -- %f
[user]
	name = username
	email = xxxx@outlook.com
	
# &&注意：这里设置的用户签名和将来登录的GitHub（或其他托管中心）账户没有任何关系
```



### 3.2	初始化本地库

```shell
git init # 初始化本地库

# 实例
$ git init
Initialized empty Git repository in G:/users/wendongzhu/Desktop/git/.git/
# 注意：.git文件默认隐藏


ll -a	# 查看生成.git文件夹（ll -a 可查看隐藏文件）
# 实例
$ ll -a
total 12
drwxr-xr-x 1 wendo 197609 0 Nov  9 19:29 ./
drwxr-xr-x 1 wendo 197609 0 Nov  9 19:02 ../
drwxr-xr-x 1 wendo 197609 0 Nov  9 19:26 .git/	# 初始化后生成文件

git status	# 本地库状态查看
# 实例1
$ git status
On branch master	# 当前本地库再master分支里面
No commits yet		#当前没有提交任何文件，当前是空库
nothing to commit (create/copy files and use "git add" to track)
```

3.2	暂存区文件的添加、删除

```shell
git add  <filename>	# 添加文件到暂存区
# 实例——添加文件
$ git add main.go
warning: LF will be replaced by CRLF in main.go.	# 文件行末换行符CRLF格式转换为LF格式
The file will have its original line endings in your working directory

# 实例——查看状态（创建main.go第一次添加到暂存区）
$ git status
On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)	# git rm --cached用于删除暂存区文件
        new file:   main.go		# main.go文件已存在于暂存区（新建）

# 实例——查看状态（修改main.go再次添加到暂存区）
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   main.go		# main.go文件已存在于暂存区（修改）



git rm --cached <filename>	# 将指定文件从暂存区删除
# 实例——删除暂存区文件
$ git rm --cached main.go
rm 'main.go'


```

提交本地库

```shell
git commit -m "版本信息" <filename>		# 将暂存区文件提交到本地库

# 实例——将暂存区文件提交到本地库（新建文件提交）
$ git commit -m "first commit" main.go	# 不写文件名将提交暂存区所以文件，一般指定文件名
warning: LF will be replaced by CRLF in main.go.
The file will have its original line endings in your working directory
[master (root-commit) 68d969d] first commit		# 版本号68d969d
 1 file changed, 1 insertion(+)
 create mode 100644 main.go

# 实例——将暂存区文件提交到本地库（修改文件提交）
$ git commit -m "second commit" main.go
warning: LF will be replaced by CRLF in main.go.
The file will have its original line endings in your working directory
[master d0c2816] second commit
 1 file changed, 3 insertions(+)



# 实例——将暂存区文件提交到本地库
$ git status
On branch master
nothing to commit, working tree clean

# 实例——查看日志信息（一个版本）
$ git log
commit 68d969d0995cbf14dacadfa12f622787b5305f30 (HEAD -> master)
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 19:54:03 2021 +0800
    first commit

# 实例——查看日志信息（多个版本）
$ git log
commit d0c28161b5d1852a7a92364413a9e45bfb058a02 (HEAD -> master)
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 20:11:08 2021 +0800
    second commit

commit 68d969d0995cbf14dacadfa12f622787b5305f30
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 19:54:03 2021 +0800
    first commit


# 实例——查看日志信息（一个版本）
$ git reflog
68d969d (HEAD -> master) HEAD@{0}: commit (initial): first commit

# 实例——查看日志信息（多个版本）
$ git reflog
d0c2816 (HEAD -> master) HEAD@{0}: commit: second commit	# 当前指向的版本 (HEAD -> master)
68d969d HEAD@{1}: commit (initial): first commit


# 注意
git log可以显示所有提交过的版本信息，不包括已经被删除的 commit 记录和 reset 的操作
git reflog是显示所有的操作记录，包括提交，回退的操作。一般用来找出操作记录中的版本号，进行回退。常用于恢复本地的错误操作。


```

​		

工作区文件创建、修改

```shell
# 实例——查看状态（工作区创建main.go文件）
$ git status
On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        main.go		# main.go文件目前只存在工作区
nothing added to commit but untracked files present (use "git add" to track)

# 实例——查看状态（工作区修改main.go）
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   main.go
no changes added to commit (use "git add" and/or "git commit -a")
```



历史版本

查看历史版本

```shell
# 注意
git log		可以显示所有提交过的版本信息，不包括已经被删除的 commit 记录和 reset 的操作
git reflog	是显示所有的操作记录，包括提交，回退的操作。一般用来找出操作记录中的版本号，进行回退。常用于恢复本			地的错误操作

# 实例——查看日志信息（多个版本
$ git log
commit ea9694fe81f23fb28889116a4b6099d453dc895f (HEAD -> master)
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 20:47:48 2021 +0800
    third commit

commit d0c28161b5d1852a7a92364413a9e45bfb058a02
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 20:11:08 2021 +0800
    second commit

commit 68d969d0995cbf14dacadfa12f622787b5305f30
Author: Alex-wendo <wendongzhu@outlook.com>
Date:   Tue Nov 9 19:54:03 2021 +0800
    first commit

# 实例——查看日志信息（多个版本）
$ git reflog
ea9694f (HEAD -> master) HEAD@{0}: commit: third commit
d0c2816 HEAD@{1}: commit: second commit
68d969d HEAD@{2}: commit (initial): first commit


```

版本穿梭

```shell
git reset <version>

# 穿梭前
$ git reflog
ea9694f (HEAD -> master) HEAD@{0}: commit: third commit
d0c2816 HEAD@{1}: commit: second commit
68d969d HEAD@{2}: commit (initial): first commit

# 穿梭版本
$ git reset --hard d0c2816
HEAD is now at d0c2816 second commit

# 穿梭后
$ git reflog
d0c2816 (HEAD -> master) HEAD@{0}: reset: moving to d0c2816
ea9694f HEAD@{1}: commit: third commit
d0c2816 (HEAD -> master) HEAD@{2}: commit: second commit	# 当前指向的版本
68d969d HEAD@{3}: commit (initial): first commit


# 本地文件夹查看信息
#路径G:\users\wendongzhu\Desktop\git\.git中HEAD文件，查看当前所在分支
#路径G:\users\wendongzhu\Desktop\git\.git\refs\heads中master文件，查看当前指向版本号d0c28161b5d1852a7a92364413a9e45bfb058a02

```



|HEAD|Branch|Version|
|----|----|----|
||hot-fix|first commit|
|head|master|second commit|
|||third commit|
|（指向分支）|（指向具体版本）|（具体版本）|





## 第四章	分支操作

### 4.1	分支的介绍

![image-20211109214709283](C:\Users\wendo\AppData\Roaming\Typora\typora-user-images\image-20211109214709283.png)

分支底层是指针的引用；

![image-20211109222810091](C:\Users\wendo\AppData\Roaming\Typora\typora-user-images\image-20211109222810091.png)

### 4.2	分支优点：

​		同时并行推进多个功能开发，提高开发效率；

​		各个分支开发过程中如果一个分支失败，不会对其他分支有任何影响；失败的分支删除重新开始即可。

### 4.3	分支的操作：

|命令|作用|备注|
|----|----|----|
|git branch <分支名>|创建分支||
|git branch -v|查看分支||
|git checkout <分支名>|切换分支||
|git merge <分支名>|把指定的分支合并到当前分支||
||||



#### 4.3.1	查看分支

```shell
$ git branch -v
* master d0c2816 second commit	# 当前只有master分支

```



#### 4.3.2	创建分支

```shell
$ git branch hot-fix

$ git branch -v
  hot-fix d0c2816 second commit
* master  d0c2816 second commit

```



#### 4.3.3	切换分支

```shell
$ git checkout hot-fix
Switched to branch 'hot-fix'

$ git branch -v
* hot-fix d0c2816 second commit
  master  d0c2816 second commit

$ git reflog
362c3d7 (HEAD -> hot-fix) HEAD@{0}: commit: hot-fix first commit
d0c2816 (master) HEAD@{1}: checkout: moving from master to hot-fix
d0c2816 (master) HEAD@{2}: reset: moving to d0c2816
ea9694f HEAD@{3}: commit: third commit
d0c2816 (master) HEAD@{4}: commit: second commit
68d969d HEAD@{5}: commit (initial): first commit


```



#### 4.3.4	合并分支

```shell
# 在master分支执行git merge hot-fix；是将hot-fix分支合并在master分支
wendo@Alex MINGW64 /g/users/wendongzhu/Desktop/git (master)
$ git merge hot-fix
Updating d0c2816..362c3d7
Fast-forward
 main.go | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)


合并冲突
wendo@Alex MINGW64 /g/users/wendongzhu/Desktop/git (master)
$ git merge hot-fix
Auto-merging main.go
CONFLICT (content): Merge conflict in main.go	#main.go文件存在合并冲突
Automatic merge failed; fix conflicts and then commit the result.	# 合并失败

wendo@Alex MINGW64 /g/users/wendongzhu/Desktop/git (master|MERGING)	#(master|MERGING)正在合并中未成功
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
  
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   main.go
no changes added to commit (use "git add" and/or "git commit -a")


$ cat main.go
1111
2222
3333
<<<<<<< HEAD
master test
=======
hot-fix test
>>>>>>> hot-fix

$ vim main.go
1111
2222
3333
master test
hot-fix test


#合并失败
$ git commit -m "merge test" main.go
fatal: cannot do a partial commit during a merge.

# 合并成功
$ git commit -m "merge test"
[master 60505a4] merge test

# 合并提交时不能带文件名，否则合并失败；如：fatal: cannot do a partial commit during a merge.
# 合并时只会修改合并是分支

# 日志查看
$ git reflog
60505a4 (HEAD -> master) HEAD@{0}: commit (merge): merge test
1c44e41 HEAD@{1}: checkout: moving from hot-fix to master
c525b60 (hot-fix) HEAD@{2}: commit: hot-fix
362c3d7 HEAD@{3}: checkout: moving from master to hot-fix
1c44e41 HEAD@{4}: commit: master test1
12f6d01 HEAD@{5}: checkout: moving from hot-fix to master
362c3d7 HEAD@{6}: checkout: moving from master to hot-fix
12f6d01 HEAD@{7}: commit: master test
362c3d7 HEAD@{8}: checkout: moving from hot-fix to master
362c3d7 HEAD@{9}: checkout: moving from master to hot-fix
362c3d7 HEAD@{10}: merge hot-fix: Fast-forward
d0c2816 HEAD@{11}: checkout: moving from hot-fix to master
362c3d7 HEAD@{12}: commit: hot-fix first commit
d0c2816 HEAD@{13}: checkout: moving from master to hot-fix
d0c2816 HEAD@{14}: reset: moving to d0c2816
ea9694f HEAD@{15}: commit: third commit
d0c2816 HEAD@{16}: commit: second commit
68d969d HEAD@{17}: commit (initial): first commit


```



## 第五章	Git 团队协作机制

5.1	团队协作





```shell
$ git remote add git-demo https://github.com/Alex-wendo/Git.git

wendo@Alex MINGW64 /g/users/wendongzhu/Desktop/git (master)
$ git remote -v
git-demo        https://github.com/Alex-wendo/Git.git (fetch)
git-demo        https://github.com/Alex-wendo/Git.git (push)



$ git push git-demo master
Enumerating objects: 21, done.
Counting objects: 100% (21/21), done.
Delta compression using up to 4 threads
Compressing objects: 100% (7/7), done.
Writing objects: 100% (21/21), 1.57 KiB | 9.00 KiB/s, done.
Total 21 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), done.
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/Alex-wendo/Git/pull/new/master
remote:
To https://github.com/Alex-wendo/Git.git
 * [new branch]      master -> master


$ git push git-demo hot-fix
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'hot-fix' on GitHub by visiting:
remote:      https://github.com/Alex-wendo/Git/pull/new/hot-fix
remote:
To https://github.com/Alex-wendo/Git.git
 * [new branch]      hot-fix -> hot-fix


$ git pull git-demo hot-fix
From https://github.com/Alex-wendo/Git
 * branch            hot-fix    -> FETCH_HEAD
Already up to date.


$ git pull git-demo master
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 674 bytes | 4.00 KiB/s, done.
From https://github.com/Alex-wendo/Git
 * branch            master     -> FETCH_HEAD
   60505a4..f108ec4  master     -> git-demo/master
Updating 60505a4..f108ec4
Fast-forward
 main.go | 3 +++
 1 file changed, 3 insertions(+)

```











Git只是一个命令行工具，一个分布式版本控制系统，正是它在背后管理和跟踪你的代码历史版本；

​		GitHub是一个面向开源及私有软件项目的托管平台，因为只支持`git`作为唯一的版本库格式进行托管，故名GitHub。主要服务是将你的项目代码托管到云服务器上，而非存储在自己本地硬盘上。

​		作用：

​				1、托管代码、历史版本管理

​				2、搜索开源项目

​				3、使用GitHub Pages服务，你可以免费搭建一个博客网站

​				4、学习、分享、提升

```
addr:https://github.com/
name:Alex-wendo
email:wendongzhu@outlook.com
账号：wendongzhu@outlook.com
密码：z706865946
```

![img](https://www.runoob.com/wp-content/uploads/2015/02/0D32F290-80B0-4EA4-9836-CA58E22569B3.jpg)

Git 与 SVN 区别点：

- **1、Git 是分布式的，SVN 不是**：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS 等，最核心的区别。
- **2、Git 把内容按元数据方式存储，而 SVN 是按文件：**所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。
- **3、Git 分支和 SVN 的分支不同：**分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。
- **4、Git 没有一个全局的版本号，而 SVN 有：**目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。
- **5、Git 的内容完整性要优于 SVN：**Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

## 一、Git 安装配置

### 1、Windows 平台上安装

```shell
#安装包下载地址：
https://gitforwindows.org/
#官网慢，可以用国内的镜像：
https://npm.taobao.org/mirrors/git-for-windows/
```

安装包下载地址：https://gitforwindows.org/

官网慢，可以用国内的镜像：https://npm.taobao.org/mirrors/git-for-windows/

### 2、Linux 平台上安装

#### Debian/Ubuntu

```shell
$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
  libz-dev libssl-dev

$ apt-get install git

$ git --version
git version 1.8.1.2
```

#### Centos/RedHat

```shell
$ yum install curl-devel expat-devel gettext-devel \
  openssl-devel zlib-devel

$ yum -y install git-core

$ git --version
git version 1.7.1
```

#### 源码安装

```shell
#最新源码包下载地址：https://git-scm.com/download
########## Centos/RedHat ##########
$ yum install curl-devel expat-devel gettext-devel \
  openssl-devel zlib-devel

########## Debian/Ubuntu ##########
$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
  libz-dev libssl-dev
  
#解压安装下载的源码包：
$ tar -zxf git-1.7.2.2.tar.gz
$ cd git-1.7.2.2
$ make prefix=/usr/local all
$ sudo make prefix=/usr/local install
```

### 3、Git 配置

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。
- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

在 Windows 系统上，Git 会找寻用户主目录下的 .gitconfig 文件。主目录即 $HOME 变量指定的目录，一般都是 C:\Documents and Settings\$USER。

此外，Git 还会尝试找寻 /etc/gitconfig 文件，只不过看当初 Git 装在什么目录，就以此作为根目录来定位。

#### 用户信息

配置个人的用户名称和电子邮件地址：

```shell
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

如果用了 **--global** 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

#### 文本编辑器

设置Git默认使用的文本编辑器, 一般可能会是 Vi 或者 Vim。如果你有其他偏好，比如 Emacs 的话，可以重新设置：:

```shell
$ git config --global core.editor emacs
```

#### 差异分析工具

还有一个比较常用的是，在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话：

```shell
$ git config --global merge.tool vimdiff
```

Git 可以理解 kdiff3，tkdiff，meld，xxdiff，emerge，vimdiff，gvimdiff，ecmerge，和 opendiff 等合并工具的输出信息。

当然，你也可以指定使用自己开发的工具，具体怎么做可以参阅第七章。

#### 查看配置信息

要检查已有的配置信息，可以使用 git config --list 命令：

```shell
$ git config --list
http.postbuffer=2M
user.name=runoob
user.email=test@runoob.com
```

有时候会看到重复的变量名，那就说明它们来自不同的配置文件（比如 /etc/gitconfig 和 ~/.gitconfig），不过最终 Git 实际采用的是最后一个。

这些配置我们也可以在 **~/.gitconfig** 或 **/etc/gitconfig** 看到，如下所示：

```shell
vim ~/.gitconfig 
```

显示内容如下所示：

```shell
[http]
    postBuffer = 2M
[user]
    name = runoob
    email = test@runoob.com
```

也可以直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可，像这样：

```shell
$ git config user.name
runoob
```

4、Git 工作流程

本章节我们将为大家介绍 Git 的工作流程。

一般工作流程如下：

- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

下图展示了 Git 的工作流程：

![img](https://www.runoob.com/wp-content/uploads/2015/02/git-process.png)

5、Git 工作区、暂存区和版本库

## 基本概念

我们先来理解下 Git 工作区、暂存区和版本库概念：

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![img](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

- 图中左侧为工作区，右侧为版本库。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
- 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
- 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
- 当对工作区修改（或新增）的文件执行 **git add** 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
- 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
- 当执行 **git reset HEAD** 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
- 当执行 **git rm --cached <file>** 命令时，会直接从暂存区删除文件，工作区则不做出改变。
- 当执行 **git checkout .** 或者 **git checkout -- <file>** 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
- 当执行 **git checkout HEAD .** 或者 **git checkout HEAD <file>** 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

6、

## 