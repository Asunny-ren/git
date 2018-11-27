# Git 远程

## 远程仓库

``` dash
git clone
```

### 远程分支

``` txt
Git clone在我们的本地仓库多了一个名为 o/master 的分支, 这种类型的分支就叫远程分支。由于远程分支的特性导致其拥有一些特殊属性。

远程分支反映了远程仓库(在你上次和它通信时)的状态。

远程分得有一个特别的属性，在你检出时自动进入分离 HEAD 状态。Git 这么做是出于不能直接在这些分支上进行操作的原因, 你必须在别的地方完成你的工作, （更新了远程分支之后）再用远程分享你的工作成果。


程分支有一个命名规范 —— 它们的格式是:

<remote name>/<branch name>

如果你看到一个名为 origin/master 的分支，那么这个分支就叫 master，远程仓库的名称就是 origin(git默认的仓库名)。


如果检出远程分支会怎么样呢？

git checkout o/master; git commit

Git 变成了分离 HEAD 状态，当添加新的提交时 o/master 也不会更新。这是因为 o/master 只有在远程仓库中相应的分支更新了以后才会更新。
```

### Git Fetch

``` txt
git fetch 完成了仅有的但是很重要的两步:

从远程仓库下载本地仓库中缺失的提交记录

更新远程分支指针(如 o/master)

远程分支反映了远程仓库在你最后一次与它通信时的状态，git fetch 就是你与远程仓库通信的方式了
git fetch 通常通过互联网（使用 http:// 或 git:// 协议) 与远程仓库通信。
```

**git fetch 并不会改变你本地仓库的状态。它不会更新你的 master 分支，也不会修改你磁盘上的文件。**

``` dash
git fetch
```

### Git Pull

``` txt
当远程分支中有新的提交时，你可以像合并本地分支那样来合并远程分支。也就是说就是你可以执行以下命令:

git cherry-pick o/master

git rebase o/master

git merge o/master

由于先抓取更新再合并到本地分支这个流程很常用，因此 Git 提供了一个专门的命令来完成这两个操作

git pull

git pull 就是 git fetch 和 git merge <just-fetched-branch> 的缩写！
```

``` dash
git pull
```

### Git Push

``` txt
git push 负责将你的变更上传到指定的远程仓库，并在远程仓库上合并你的新提交记录

注意 —— git push 不带任何参数时的行为与 Git 的一个名为 push.default 的配置有关。它的默认值取决于你正使用的 Git 的版本




假设你周一克隆了一个仓库，然后开始研发某个新功能。到周五时，你新功能开发测试完毕，可以发布了。但是 —— 天啊！你的同事这周写了一堆代码，还改了许多你的功能中使用的 API，这些变动会导致你新开发的功能变得不可用。但是他们已经将那些提交推送到远程仓库了，因此你的工作就变成了基于项目旧版的代码，与远程仓库最新的代码不匹配了。

这种情况下, git push 就不知道该如何操作了。如果你执行 git push，Git 应该让远程仓库回到星期一那天的状态吗？还是直接在新代码的基础上添加你的代码，异或由于你的提交已经过时而直接忽略你的提交？

因为这情况（历史偏离）有许多的不确定性，Git 是不会允许你 push 变更的。实际上它会强制你先合并远程最新的代码，然后才能分享你的工作。

git fetch; git rebase o/master; git push
我们用 git fetch 更新了本地仓库中的远程分支，然后用 rebase 将别人的工作移动到最新的提交记录下，最后再用 git push 推送到远程仓库。


git pull 就是 fetch 和 merge 的简写
git pull --rebase 就是 fetch 和 rebase 的简写！
```

``` dash
git push


```