# git summary(廖雪峰老师的git教程)

> 分布式和集中式的区别（git and svn）

* 集中式（必须联网）

  集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

* 分布式
  
  分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，两个人之间可以互相推送各自的修改。如果不在同一个局域网中时，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，用来方便大家交换修改。

> 安装 install

``` code
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

``` code
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

``` code
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

  // 显示每一次操作的命令
  git reflog

  // 把工作区的修改全部撤销
  git checkout -- test.html
  * 一种是test.html自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
  * 一种是test.html已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

  // !!!! notice
  git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令

  // 把暂存区的内容回退到工作区
  git reset HEAD test.html

  // 删除
  rm test2.html
  git rm test2.html
  git commit -m 'delete'

  // 删除还原
  git checkout -- test2.html

```

> 工作区和暂存区（Working Directory and Stage）

  * eg. git_repository就是一个工作区，但是.git不算工作区，是git的版本库。

  * Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

  ![图例](https://github.com/Asunny-ren/git/blob/master/git.jpeg)

  * git add相当于把文件修改添加到暂存区中

  * git commit提交更改，把暂存区的所有内容提交到当前分支中

> 本地库与远程库协作

  ``` code
  // 本地与远程关联
  $ git remote add origin git@github.com:Asunny-ren/git_repository.git

  // 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
  $ git push -u origin master

  // 本地作了修改就可以提交到远程
  $ git push origin master
  ```

  ``` code
  // 从远程仓库克隆到本地
  $ git clone git@github.com:Asunny-ren/git_repository.git

  // 也可以通过https协议克隆
  git clone https://github.com/Asunny-ren/git.git
  ```

> 创建和合并分支

  * 在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

  * 一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
  
  ![](https://github.com/Asunny-ren/git/blob/master/git_branch1.png)

  * 每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长：

  * 当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：

  ![](https://github.com/Asunny-ren/git/blob/master/git_branch2.png)

  * Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：

  ![](https://github.com/Asunny-ren/git/blob/master/git_branch3.png)

  * 假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：

  ![](https://github.com/Asunny-ren/git/blob/master/git_branch4.png)

  * Git合并分支也很快！就改改指针，工作区内容也不变！合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：

  ![](https://github.com/Asunny-ren/git/blob/master/git_branch5.png)

  ``` code
  //创建分支dev并切换到dev
  $ git checkout -b dev
  Switched to a new branch 'new'

  // or

  // 加上-b参数表示创建并切换到dev,相当于: 
  $ git branch dev
  $ git checkout dev
  Switched to branch 'dev'

  // 在dev分支上进行修改提交，然后切换到master分支
  $ git checkout master
  Switched to branch 'master'

  // 切换到master分支之后发现，之前在dev分支上修改的东西没了，因为那些修改都在dev分支上，master上看不到，需要我们把dev合并到master上。

  //合并指定分支到当前分支上
  $ git merge dev
  Updating bb79d63..d771531
  Fast-forward
  test.html | 1 +
  1 file changed, 1 insertion(+)

  // 删除dev分支
  $ git branch -d dev
  ```