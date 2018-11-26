# [Git](https://learngitbranching.js.org/)

## 基础篇

## Git Commit

> git commit

``` txt
Git 仓库中的提交记录保存的是你的目录下所有文件的快照，就像是把整个目录复制，然后在粘贴一样，但比复制粘贴优雅许多！

Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录。

Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面都有父节点的原因
```

``` dash
git commit
```

## Git Branch

> git branch bugFix

``` txt
Git 的分支也很轻量，它们只是简单地指向某个提交记录。

早建分支！多用分支！

即使创建再多的分支也不会造成储存或内存上的开销，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单的多

* 表示当前所在分支

切换分支

git checkout bugFix

eg: git checkout bugFIx; git commit

如果你想创建一个新的分支同时切换到新创建的分支的话，可以通过 git checkout -b <your-branch-name> 来实现。
```

``` dash
# 新建bugFix分支并切换到bugFix上
git checkout -b bugFix

# 提交
git commit

# 切换到master上
git checkout master
```

## Git Merge

> git merge

``` txt
在Git中合并两个分支时会产生一个特殊的提交记录，它有两个父节点。翻译成自然语言相当于：“我要把这两个父节点本身及它们所有的祖先都包含进来。”

合并分支bugFix到master
git merge bugFix

git checkout bugFix; git merge master
```

``` dash
# 创建新的分支bugFix,并切换到bugFix
git checkout -b bugFix

# 提交
git commit

# 切换到master
git checkout master

# 提交
git commit

# 用git merge 把 bugFix合并到master
git merge bugFix
```

## Git rebase

> git rebase master

``` txt
Git rebase 实际上就是取出一系列的提交记录，“复制它们”，然后在另外一个地方逐个的放下去

Rebase 的优势就是可以创造更线性的提交历史。如果只允许使用Rebase的话，代码库的提交历史将变得异常清晰。
```

``` dash
# 新建bugFix分支
git branch bugFix

# 切换到bugFix
git checkout bugFix

# 提交
git commit

#切换到master再提交一次
git checkout master; git commit

# 再切换到bugFix分支，rebase到master上
git checkout bugFix;
git rebase master
```

## 高级篇

### 在提交树上移动

``` txt
HEAD 是一个对当前检出记录的符号引用 -- 也就是指向你正在其基础上进行工作的提交记录。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 指向开始的

HEAD 通常情况下是指向分支名的（如bugFix），在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。

如果想看 HEAD 指向，可以通过 cat .git/HEAD 查看， 如果 HEAD 指向的是一个引用，还可以用 git symbolic-ref HEAD 查看它的指向
```

### 分离的HEAD

``` txt
分离的 HEAD 就是让其指向了某个具体的提交记录而不是分支名。在命令执行之前的状态如下所示：

HEAD -> master -> C1

HEAD 指向 master， master 指向 C1

# 执行命令
git checkout C1

HEAD -> C1
```

### 相对引用

``` txt
通过指定提交记录哈希值的方式在 Git 中移动不太方便。在实际应用时，并没有像本程序中这么漂亮的可视化提交树供你参考，所以你就不得不用 git log 来查查看提交记录的哈希值。

并且哈希值在真实的 Git 世界中也会更长（译者注：基于 SHA-1，共 40 位）。例如前一关的介绍中的提交记录的哈希值可能是 fed2da64c0efc5293610bdd892f82a58e8cbc5d8。舌头都快打结了吧...

比较令人欣慰的是，Git 对哈希的处理很智能。你只需要提供能够唯一标识提交记录的前几个字符即可。因此我可以仅输入fed2 而不是上面的一长串字符。




#相对引用

使用相对引用的话，你就可以从一个易于记忆的地方（比如 bugFix 分支或 HEAD）开始计算。

相对引用非常给力，这里我介绍两个简单的用法：




# ^
使用 ^ 向上移动 1 个提交记录

把这个符号加在引用名称的后面，表示让 Git 寻找指定提交记录的父提交。master^ 相当于“master 的父节点”。

使用 ~<num> 向上移动多个提交记录，如 ~3





可以直接使用 -f 选项让分支指向另一个提交。例如:

git branch -f master HEAD~3

上面的命令会将 master 分支强制指向 HEAD 的第 3 级父提交。

相对引用为我们提供了一种简洁的引用提交记录 C1 的方式，而 -f 则容许我们将分支强制移动到那个位置。
```

### 撤销变更

``` txt
在 Git 里撤销变更的方法很多。和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。

主要有两种方法用来撤销变更 —— 一是 git reset，还有就是 git revert
```

``` dash
git reset HEAD~1

# Git 把 master 分支移回到 C1；现在我们的本地代码库根本就不知道有 C2 这个提交了。在reset后， C2 所做的变更还在，但是处于未加入暂存区状态。



虽然在你的本地分支中使用 git reset 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的哦！

为了撤销更改并分享给别人，我们需要使用 git revert


# 从当前分支local撤销到最近一次更改，reset只做本地撤销变更
git reset HEAD^

# 从当前分支pushed撤销变更到最近一次更改,并切分享给别人，revert可以将变更分享给远程仓库
git revert HEAD
```

## 移动提交记录

### 整理提交记录

``` txt
如果你想将一些提交复制到当前所在的位置（HEAD）下面的话， Cherry-pick 是最直接的方式了

git cherry-pick <提交号>...
```

``` dash
git cherry-pick C3 C4 C7
```

### 交互式的rebase

``` txt
当你你知道你所需要的提交记录（并且还知道这些提交记录的哈希值）时, 用 cherry-pick 再好不过了 —— 没有比这更简单的方式了。

但是如果你不清楚你想要的提交记录的哈希值呢? 幸好 Git 帮你想到了这一点, 我们可以利用交互式的 rebase —— 如果你想从一系列的提交记录中找到想要的记录, 这就是最好的方法了



交互式 rebase 指的是使用带参数 --interactive 的 rebase 命令, 简写为 -i

如果你在命令后增加了这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。

在实际使用时，所谓的 UI 窗口一般会在文本编辑器 —— 如 Vim —— 中打开一个文件。 考虑到课程的初衷，我弄了一个对话框来模拟这些操作。



当 rebase UI界面打开时, 你能做3件事:

1.调整提交记录的顺序（通过鼠标拖放来完成）
2.删除你不想要的提交（通过切换 pick 的状态来完成，关闭就意味着你不想要这个提交记录）
3.合并提交。允许你把多个提交记录合并成一个。
```

``` dash
git rebase -i HEAD~4

or

git rebase --interactive HEAD~4
```

## 本地栈式提交

``` txt
在开发中经常会遇到的情况：我正在解决某个特别棘手的 Bug，为了便于调试而在代码中添加了一些调试命令并向控制台打印了一些信息。

这些调试和打印语句都在它们各自的提交记录里。最后我终于找到了造成这个 Bug 的根本原因，解决掉以后觉得沾沾自喜！

最后就差把 bugFix 分支里的工作合并回 master 分支了。你可以选择通过 fast-forward 快速合并到 master 分支上，但这样的话 master 分支就会包含我这些调试语句了。你肯定不想这样，应该还有更好的方式……
```

``` dash
git rebase -i

git cherry-pick
```

### 提交技巧 #1

``` txt
你之前在 newImage 分支上进行了一次提交，然后又基于它创建了 caption 分支，然后又提交了一次。

此时你想对的某个以前的提交记录进行一些小小的调整。比如设计师想修改一下 newImage 中图片的分辨率，尽管那个提交记录并不是最新的了。
```

``` dash
#先用 git rebase -i 将提交重新排序，然后把我们想要修改的提交记录挪到最前
git rebase -i

#然后用 commit --amend 来进行一些小修改
commit --amend

#接着再用 git rebase -i 来将他们调回原来的顺序
git rebase -i

#最后我们把 master 移到修改的最前端（用你自己喜欢的方法），就大功告成啦！
```

### 提交技巧 #2

``` dash
git checkout master

git cherry-pick C2

git commit --amend

git cherry-pick C3
```

## Git Tag

``` txt
在某种程度上 —— 因为标签可以被删除后重新在另外一个位置创建同名的标签。它们并不会随着新的提交而移动。
```

``` dash
# 在C1处增加标签V1，并且指向C1
#我们将这个标签命名为 v1，并且明确地让它指向提交记录 C1，如果你不指定提交记录，Git 会用 HEAD 所指向的位置。

git tag V1 C1
```

### Git Describe

``` txt
由于标签在代码库中起着“锚点”的作用，Git 还为此专门设计了一个命令用来描述离你最近的锚点（也就是标签），它就是 git describe！

Git Describe 能帮你在提交历史中移动了多次以后找到方向；当你用 git bisect（一个查找产生 Bug 的提交记录的指令）找到某个提交记录时，或者是当你坐在你那刚刚度假回来的同事的电脑前时， 可能会用到这个命令。
```

``` dash
#<ref> 可以是任何能被 Git 识别成提交记录的引用，如果你没有指定的话，Git 会以你目前所检出的位置（HEAD）

git describe <ref>

它输出的结果是这样的：

<tag>_<numCommits>_g<hash>

tag 表示的是离 ref 最近的标签， numCommits 是表示这个 ref 与 tag 相差有多少个提交记录， hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位。

当 ref 提交记录上有某个标签时，则只输出标签名称
```