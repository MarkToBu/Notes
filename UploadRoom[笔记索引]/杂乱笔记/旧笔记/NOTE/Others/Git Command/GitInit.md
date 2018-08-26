# GitHub  详解
## 1. 什么是 GitHub
*GitHub 是为开发者提供 Git 仓库的托管服务。这是一个让开发者与
朋友、同事、同学及陌生人共享代码的完美场所。*


== **专栏：GitHub 与 Git 的区别
在此讲解一下 GitHub 与 Git的区别。
GitHub 与 Git 是完
全不同的两个东西。本书中，自始至终都以 GitHub 和 Git 的方
式区分描述。
在 Git 中，开发者将源代码存入名叫“Git 仓库”的资料库中
并加以使用。而 GitHub 则是在网络上提供 Git 仓库的一项服务。
也就是说，GitHub 上公开的软件源代码全都由 Git 进行管
理。理解 Git，是熟练运用 GitHub 的关键所在。Git 的相关知识，
我们将在第 2 章中为您详细讲解。**  ==

###  使用 GitHub 会带来哪些变化
     ● 协作形式变化
     ● 在开发者之间引发化学反应的 Pull Request
     ● 对特定用户进行评论
     ●文字输入功能的变革
     ● 能看到更多其他团队的软件
     ●  与开源软件相同的开发模式

###  关于社会化编程
       是软件开发方法的一种变革。任何人都可以更容易地获得源码，修改源码。为软件开发者的世界带来了真正意义上的“民主”，让所有人都平等地拥有了更改源代码的权利。

### GitHub 提供的主要功能
      ● Git 仓库
      ● Organization
      ● Issue    [是将一个任务或问题分配给一个 Issue 进行追踪和管理的功能。]
      ● Wiki [通过 Wiki 功能，任何人都能随时对一篇文章进行更改并保存，因此可以多人共同完      成一篇文章。该功能常用在开发文档或手册的编写中。]
      ● Pull Request  [开发者向 GitHub 的仓库推送更改或功能添加后，可以通过 Pull
        Request 功能向别人的仓库提出申请，请求对方合并。
    	Pull Request 送出后，目标仓库的管理者等人将能够查看 Pull
    	Request 的内容及其中包含的代码更改。
    	同时，GitHub 还提供了对 Pull Request 和源代码前后差别进行讨论
    	的功能。]

## 2. Git认识

	Git 属于分散型版本管理系统，是为版本管理而设计的软件。
### 什么是版本管理

       版本管理就是管理更新的历史记录。它为我们提供了一些在软件开发过程中必不可少的功能，例如记录一款软件添加或更改源代码的过程，回滚到特定阶段，恢复误删除的文件等。
###  集中型与分散型
       * 集中型 * 将所有数据集中存放在服务器当中，有便于管理的优点。但
       是一旦开发者所处的环境不能连接服务器，就无法获取最新的源代码，
       开发也就几乎无法进行。服务器宕机时也是同样的道理，而且万一服务
       器故障导致数据消失，恐怕开发者就再也见不到最新的源代码了。
        * 分散型 *  分散型拥有多个仓库，相对而言稍显复杂。不过，由于
    	本地的开发环境中就有仓库，所以开发者不必连接远程仓库就可以进行
    	开发。集中型和分散型双方都有优缺点，不过git的使用会占绝大多数。
### Git安装和初始化设置

        安装：下一步，默认即可
_ _ _
###   Git初始化设置    
1. 		● 设置姓名和邮箱地址
`$ git config --global user.name "Firstname Lastname"`
`$ git config --global user.email "your_email@example.com"`


   这个命令，会在“~/.gitconfig”中以如下形式输出设置文件。
>  [user]
>   name = Firstname Lastname
>   email = your_email@example.com 

想更改这些信息时，可以直接编辑这个设置文件。这里设置的姓名
和邮箱地址会用在 Git 的提交日志中。由于在 GitHub 上公开仓库时，这
里的姓名和邮箱地址也会随着提交日志一同被公开

     	● 提高命令输出的可读性
顺便一提，将 color.ui 设置为 auto 可以让命令的输出拥有更高的可
读性。
```
$ git config --global color.ui auto
 
“~/.gitconfig”中会增加下面一行。
> [color]
> ui = auto

这样一来，各种命令的输出就会变得更容易分辨。


# 使用GitHub的前期准备

## 前期准备
1.     创建账户
2.     设置头像
3.     设置SSH Key
       [GitHub 上连接已有仓库时的认证，是通过使用了 SSH 的公开密钥
		认证方式进行的。创建公开密钥认证所需的 SSH Key，并将其添加至 GitHub。] 
			创建方法：
			运行下面的命令创建 SSH Key。

```
    $ ssh-keygen -t rsa -C "your_email@example.com"
    nerating public/private rsa key pair.
    ter file in which to save the key
    Users/your_user_directory/.ssh/id_rsa): 按回车键
    ter passphrase (empty for no passphrase): 输入密码
    ter same passphrase again: 再次输入密码
     
     “your_email@example.com”的部分请改成您在创建账户时用的邮箱地址。密码需要在认证时输		入，请选择复杂度高并且容易记忆的组合。
    	输入密码后会出现以下结果。
Your identification has been saved in /Users/your_user_directory/.ssh/id_rsa.
Your public key has been saved in /Users/your_user_directory/.ssh/id_rsa.pub.
The key fingerprint is:
fingerprint值 your_email@example.com
The key's randomart image is:

		+--[ RSA 2048]----+
		| .+ + |
		| = o O . |
		略
*id_rsa 文件是私有密钥，id_rsa.pub 是公开密钥。*
● 添加公开密钥
GitHub 中添加公开密钥，今后就可以用私有密钥进行认证了。
点击右上角的账户设定按钮（Account Settings），选择 SSH Keys 菜
单。点击 Add SSH Key 之后，会出现如图 3.2 的输入框。在 Title 中输入
适当的密钥名称。Key 部分请粘贴 id_rsa.pub 文件里的内容。id_rsa.pub
的内容可以用如下方法查看。
```
$ cat ~/.ssh/id_rsa.pub
ssh-rsa 公开密钥的内容 your_email@example.com
```

添加成功之后，创建账户时所用的邮箱会接到一封提示“公共密钥添加完成”的邮件。
完成以上设置后，就可以用手中的私人密钥与 GitHub 进行认证和通信了。 
```
$ ssh -T git@github.com
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is fingerprint值 .
Are you sure you want to continue connecting (yes/no)? 输入yes
```

实际动手使用 　　出现如下结果即为成功。
Hi hirocastest! You've successfully authenticated, but GitHub does not
provide shell access.

  ● 使用社区功能
     Follow别人：在创建账户后不妨试试
		Follow（关注）别人。在用户信息页面的右上角点击Follow 按钮即可。
	
		这样一来，您所 Follow 的用户的活动就会显示在您的控制面板页面
		中。您可以通过这种方法知道那个人在 GitHub 上都做了些什么。
		对于仓库，也可以使用 Watch 功能获取最新的开发信息。如果您经
		常使用的某个软件正在 GitHub 上进行开发，不妨去 Watch 一下。
#### 实际动手

##### 创建仓库

		实际创建一个公开的仓库。点击右上角工具栏里的 New repository 
 		图标，创建新的仓库。
		
		Repository name
参考图 3.5，在 Repository name 栏中输入仓库的名称。这里我们输
入 Hello-World。
 设置=》》》》
 Description
Description 栏中可以设置仓库的说明。这一栏不是必需项，可以留空。
 Public、Private
在这一栏可以选择 Public 还是 Private。这里我们选择 Public，创建
公开仓库，仓库内的所有内容都会被公开。
选择 Private 可以创建非公开仓库，用户可以设置访问权限，但这项
服务是收费的。
 Initialize this repository with a README
在 Initialize this repository with a README 选项上打钩，随后
GitHub 会自动初始化仓库并设置 README 文件，让用户可以立刻
clone 这个仓库。如果想向 GitHub 添加手中已有的 Git 仓库，建议不要
勾选，直接手动 push。
 Add .gitignore
下方左侧的下拉菜单非常方便，通过它可以在初始化时自动生
成.gitignore文件
A 。这个设定会帮我们把不需要在Git仓库中进行版本管
理的文件记录在 .gitignore 文件中，省去了每次根据框架进行设置的麻
烦。下拉菜单中包含了主要的语言及框架，选择今后将要使用的即可。
由于本书中我们并不使用任何框架，所以不做选择。
 Add a license
右侧的下拉菜单可以选择要添加的许可协议文件。如果这个仓库中
包含的代码已经确定了许可协议，那么请在这里进行选择。随后将自动
生成包含许可协议内容的 LICENSE 文件，用来表明该仓库内容的许可
协议。
输入选择都完成后，点击 Create repository 按钮，==完成仓库的创建==。

● 连接仓库
下面这个 URL 便是刚刚创建的仓库的页面。
https://github.com/用户名/Hello-Word

**使用 GitHub 后，很多文档都需要用 Markdown 来书写。也就是说，
全世界有大量程序员都在使用 Markdown，因此掌握这种语法已经成为
程序员的标准技能之一。请各位也务必学会 Markdown 语法。**

##### clone 已有仓库

接下来我们将尝试在已有仓库中添加代码并加以公开。首先将已有
仓库 clone 到身边的开发环境中。clone 时指定的路径 位于 仓库的右下角。
```
$ git clone git@github.com:hirocastest/Hello-World.git
Cloning into 'Hello-World'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
$ cd Hello-World

```

这里会要求输入 GitHub 上设置的公开密钥的密码。认证成功后，
仓库便会被 clone 至仓库名后的目录中。将想要公开的代码提交至这个
仓库再 push 到 GitHub 的仓库中，代码便会被公开。
    在克隆的目录下新编写 代码，查看状态：
 	$ git status
  	On branch master
 	 Untracked files:
  (use "git add <file>..." to include in what will be committed)
  nothing added to commit but untracked files present (use "git add" to track)

##### 提交

将 hello_word.php 提交至仓库。这样一来，这个文件就进入了版本
管理系统的管理之下。今后的更改管理都交由 Git 进行。
```
$ git add hello_world.php
$ git commit -m "Add hello world script by php"
[master d23b909] Add hello world script by php
1 file changed, 3 insertions(+)
create mode 100644 hello_world.php
```
通过 git add命令将文件加入暂存区
A ，再通过 git commit命令
提交。
添加成功后，可以通过 git log命令查看提交日志。
```
$ git log
commit d23b909caad5d49a281480e6683ce3855087a5da
Author: hirocastest <hohtsuka@gmail.com>
Date: Tue May 1 14:36:58 2012 +0900
Add hello world script by php
```

##### 进行 push

之后只要执行 push，GitHub 上的仓库就会被更新。
```
$ git push
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 328 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:hirocastest/Hello-World.git
46ff713..d23b909 master -> master
```

这样一来代码就在 GitHub 上公开了。可以实际连接 http://github.
com/ 用户名 /Hello-World 查看一下。



## 3. Git 基本设置

      1. 配置用户名：  git config --global user.name "name"
    
     	2. 邮箱：              git config --global user.email "...@..."
    
     	3. 其他：              git config --global merge.tool "kdiff3"
    
         ​                         #用来比较不同的插件，没有就不用装 
    
         ​			 git config --global core.autocrlf  false 
    
         ​                        #让git不用管Windows/Unix换行符转换。      

> git编码设置

​       git  config --global gui.encoding	 utf-8   # 避免git gui的中文乱码

​       git config --global core.quotepath  off       # 避免 git status 显示中文名字的乱码

​    windows上，需要设置： git config  --global core.ignorcase false



​     查看安装情况：  git --version

  #### git ssh key pair 配置

1. 在Linux 的命令行下，或者window的Git Bash命令行窗口中键入： 

​            ssh-keygen -t rsa -C "632318927@qq.com"  

​       一路回车，不输入任何密码，生成ssh key pair

2. ssh -add ~/.ssh/id_rsa        如果保存，先=>eval 'ssh-agent'  ,最后用ssh -add -l查看

3. cat ~/.ssh/id_rsa.pub

      复制内容

4.  在web上配置ssh秘钥

查看安装情况：  git --version



##4. Git 常用命令

1. 切换分支  git checkout 分支名
2.  拉取   git pull
3.  提交    git push





### 基本操作

###git init——初始化仓库
要使用 Git 进行版本管理，必须先初始化仓库。Git 是使用 git
init命令进行初始化的。请实际建立一个目录并初始化仓库。
$ mkdir git-tutorial
$ cd git-tutorial
$ git init
Initialized empty Git repository in /Users/hirocaster/github/github-book
/git-tutorial/.git/
如果初始化成功，执行了 git init命令的目录下就会生成 .git 目
录。这个 .git 目录里存储着管理当前目录内容所需的仓库数据。
$ git status
### On branch master

### Initial commit


nothing to commit (create/copy files and use "git add" to track)
###git status——查看仓库的状态

git status命令用于显示 Git 仓库的状态。这是一个十分常用的命令，请务必牢记。

工作树和仓库在被操作的过程中，状态会不断发生变化。在 Git 操
作过程中时常用 git status命令查看当前状态，可谓基本中的基本。
下面，就让我们来实际查看一下当前状态

``` 
	$ git status
	# On branch master
	#
	# Initial commit
	#
	nothing to commit (create/copy files and use "git add" to track)

```

结果显示了我们当前正处于 master 分支下。关于分支我们会在不久
后讲到，现在不必深究。接着还显示了没有可提交的内容。所谓提交
（Commit），是指“记录工作树中所有文件的当前状态”。
尚没有可提交的内容，就是说当前我们建立的这个仓库中还没有记
录任何文件的任何状态。这里，我们建立 README.md 文件作为管理对
象，为第一次提交做前期准备。
```
	$ touch README.md
	$ git status
	# On branch master
	#
	# Initial commit
	## Untracked files:# (use "git add <file>..." to include in what will
	be committed)#
	# README.md
	nothing added to commit but untracked files present (use "git add" to
	track)
```
	可以看到在 Untracked files 中显示了 README.md 文件。类似地，
	只要对 Git 的工作树或仓库进行操作，git status命令的显示结果就
	会发生变化。

#### ● git add——向暂存区中添加文件
	如果只是用 Git 仓库的工作树创建了文件，那么该文件并不会被记
	入 Git 仓库的版本管理对象当中。因此我们用 git status命令查看
	README.md 文件时，它会显示在 Untracked files 里。
	要想让文件成为 Git 仓库的管理对象，就需要用 git add命令将其
	加入暂存区（Stage 或者 Index）中。暂存区是提交之前的一个临时区域。
```
	$ git add README.md
	$ git status
	# On branch master
 	#
	# Initial commit
	#
	# Changes to be committed:
	# (use "git rm --cached <file>..." to unstage)
	#
	# new file: README.md
	#
 
```
	将 README.md 文件加入暂存区后，git status命令的显示结
	果发生了变化。可以看到，README.md 文件显示在 Changes to be
	committed 中了。



#  ---在Git中如何撤销commit前的add操作？

 

[http://stackoverflow.com/questions/348170/how-to-undo-git-add-before-commit](https://link.jianshu.com/?t=http://stackoverflow.com/questions/348170/how-to-undo-git-add-before-commit)

## 问题：

我错误的使用了下面的命令添加了文件：

```
git add myfile.txt
```

现在我还没有运行`git commit`命令。有办法撤销它，从而不让这些文件包含到commit中吗？

## 高票答案1:

你寻找的是这个：

```
git rm --cached <added_file_to_undo>
```

原因:
当我是个新手的时候，我首先试了试

```
git reset .
```

（来撤销我整个的初始化添加），结果却只得到了这条（没那么）有用的信息：

```
fatal: Failed to resolve 'HEAD' as a valid ref.
```

（译者注：在[Git 1.8.2](https://link.jianshu.com/?t=https://git.kernel.org/cgit/git/git.git/tree/Documentation/RelNotes/1.8.2.txt#n179)中已经修正了此问题）

看起来是因为HEAD指针在首次提交之前是不存在的。换句话说，你将会陷进和我一样的初学者问题中去，加入你的工作流和我差不多，那么就会发生以下事情：

1. cd到你伟大的新项目目录下，想要试试Git这个新的热门的东西
2. `git init`
3. `git add .`
4. `git status`
   ...一大波乱七八糟的提示滚了过去...
   => 艹！我不想把那些玩意儿都添加进去！
5. google一下关键字`undo git add`
   => 找到 Stack Overflow - 吼吼
6. `git reset .`
   => fatal: Failed to resolve 'HEAD' as a valid ref.

貌似看起来在有人因为抗议这个没用的提示，在邮件列表中[记录了一个bug]。然后正确的解决方案（是的，我当做垃圾信息忽略了）就藏在git status命令输出的提示中

```
...
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
...
```

最终，我们的解决方案是使用`git rm --cached FILE`.

但是有个别处的提醒需要告诉你：`git rm`会删除你工作区域的文件副本，除非你使用了`--cached`参数。这里是`git help rm`中提取出的信息：

> **--cached** Use this option to unstage and remove paths only from the index. Working tree files, whether modified or not, will be left.

> **--cached** 使用该选项来只从暂存区移除路径和状态信息。工作区文件，无论是否改动，将会保留。

那我继续使用

```
git rm --cached .
```

来移除所有的东西，重新开始。没用，因为`add .`命令是递归的，所以`rm`命令需要`-r`参数来同样保证递归。唉...

```
git rm -r --cached .
```

好了，我们现在回到了刚开始的地方。下一次我要使用`-n`参数来做一次预演，看看什么将会被添加进去:

```
git add -n .
```

以防万一，我把所有的东西压缩打包到一个安全的地方，毕竟谁也没法保证`git help rm`中关于`--cached`的描述是否100%可信（或者万一我漏掉了什么呢）。