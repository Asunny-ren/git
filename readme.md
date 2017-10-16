# git summary(廖雪峰老师的git教程)

>  分布式和集中式的区别（git and svn）

* 集中式（必须联网）

  集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

* 分布式
  
  分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，两个人之间可以互相推送各自的修改。如果不在同一个局域网中时，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，用来方便大家交换修改。

> 安装 install

```
  可以在终端输入git查看是否安装git
  
  sudo apt-get install git

  查看安装版本
  git --version


  // windows
  https://git-for-windows.github.io

  // config
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
```

> 建立仓库（build repository）

```
  $ mkdir git_repository
  $ cd git_repository
  $ git init

  // 显示当前目录
  $ pwd
  /Users/****/git_repository

  // 查看隐藏文件
  ls -ah

```

> git command

```
  // 查看结果
  git status 

  //查看具体修改了什么
  git diff 

  //添加test.html到版本控制库
  git add test.html 

  //提交到本地版本库中
  git commit -m 'add test.html' 

  // 查看提交日志，加参数--pretty=oneline显示版本号和提交备注
  git log 

  // git中HEAD代表当前版本，上一个版本为HEAD^,依此类推，往上100个版本可以写为HEAD~100
  // 回退
  git reset --hard HEAD^
  // 替换为某个版本
  git reset --hard 某个版本号

  // 显示每一次操作的命令
  git reflog
```

> 工作区和暂存区（Working Directory and Stage）

  * eg. git_repository就是一个工作区，但是.git不算工作区，是git的版本库。

  * Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![图例](https://github.com/Asunny-ren/git/blob/master/git.jpeg)
