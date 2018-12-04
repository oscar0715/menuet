---
title: Git_Learning
tags:
  - Git
  - Tool
categories:
  - Technology
  - Tool
date: 2016-05-23 11:39:07
---
廖雪峰Git教程
参考: {% link 史上最浅显易懂的Git教程 http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 史上最浅显易懂的Git教程 %} 


<!-- more -->

***

# 创建版本库
初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：
1. 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
2. 第二步，使用命令git commit，完成。

# 时光机穿梭
要随时掌握工作区的状态，使用`git status`命令。

如果git status告诉你有文件被修改过，用`git diff`可以查看修改内容。

## 版本回退
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数,`git log --pretty=oneline`

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 工作区和暂存区
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

1. 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
2. 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

## 管理修改
为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

每次修改，如果不add到暂存区，那就不会加入到commit中。

## 撤销修改

- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


`git checkout -- file`命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

## 删除文件
命令`git rm`用于删除一个文件。 rm 与 add类似，将删除添加到暂存区

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

# 远程仓库
此处注册一个github账号，给电脑添加一个rsa key,然后将公钥提交到github上

## 添加远程仓库
在Github上添加一个新的叫做learngit的远程仓库

用以下命令将远程仓库与本地库关联。
`git remote add origin git@github.com:oscar0715/learngit`

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
从现在起，只要本地作了提交，就可以通过命令：
`git push origin master`

## 从远程仓库clone
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

# 分支管理
## 创建合并分支
截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD(指针)指向的就是当前分支。

{% codeblock lang:sh %}
// 首先，我们创建dev分支，然后切换到dev分支：
$ git checkout -b dev
Switched to a new branch 'dev'

//这一条语句等价于
// 新开dev分支
git branch dev
// 切换到dev分支 
git checkout dev

// 合并分支, 合并dev分支到当前分支
`git merge dev` 
{% endcodeblock %}

小结

Git鼓励大量使用分支：
- 查看分支：git branch
- 创建分支：git branch <name>
- 切换分支：git checkout <name>
- 创建+切换分支：git checkout -b <name>
- 合并某分支到当前分支：git merge <name>
- 删除分支：git branch -d <name>

## 解决冲突
查看分支合并情况 git log --graph --pretty=oneline --abbrev-commit

删除分支
`git branch -d feature1`


## 分支管理策略
1. 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
2. 干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
3. 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

* 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

## Bug分支
当你收到一个代号101的bug任务时，自然想要创建一个分支`issue01`来修复它，但是现在正在`dev`上进行的工作还没有提交。然而工作进行到一半，并不能提交。

Git提供了`stash`功能。
`git stash` 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

用`git stash list`命令看看刚才的工作现场.

Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
另一种方式是用`git stash pop`，恢复的同时把stash内容也删了。

## feature 分支
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

## 多人协作
要查看远程库的信息，用`git remote`
或者，用`git remote -v`显示更详细的信息

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
- master分支是主分支，因此要时刻与远程同步；
- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

多人协作的工作模式通常是这样：

1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

# 标签管理
发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

## 创建标签

1. 命令`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个commit id；
2. `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
3. `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；
4. 命令`git tag`可以查看所有标签。

## 操作标签

# 自定义git
## 忽略特殊文件
- 忽略某些文件时，需要编写.gitignore；
- .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

## 配置别名alias
常用配置
{% codeblock lang:sh %}
// git lg
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

// git last
{% endcodeblock %}

配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中：

***

# 参考
{% link 史上最浅显易懂的Git教程 http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 史上最浅显易懂的Git教程 %} 

{% link Git Cheat Sheet http://pan.baidu.com/s/1jGxjQL4#path=%252Fpub%252Fgit Git Cheat %} 

