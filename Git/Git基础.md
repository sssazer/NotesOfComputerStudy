# 1. Git基础

## 1.1 什么是Git

Git是一个分布式版本管理系统。起初是为了更好地管理Linujx内核开发而创立

## 1.2 数据库

Git会维护一个数据库，用于记录文件或目录状态，存储内容修改地历史记录

数据库分为：

- 远程数据库
- 本地数据库

## 1.3 修改记录的提交

把文件或目录 的 添加和变更保存到数据库时，就需要进行提交

提交以时间顺序被保存到数据库中

执行提交时，系统会要求输入提交信息，必须输入提交信息，如果提交信息空白执行提交会失败

```
Git标准注释：
第一行：提交修改内容的概要
第二行：空行 
第三行开始：修改的理由
```

## 1.4 工作树和索引

工作树：在Git管理下大家实际操作的目录

索引：在工作树和数据库之间有索引，索引是一个向数据库提交作准备的区域

要提交文件，首先需要把文件加入到索引区域中。这样可以避免工作树中不必要的文件提交

# 2. 本地操作Git

## 2.1 安装与基本配置

[Git下载地址](https://git-scm.com/download/) 

下载安装完成之后，在开始菜单中搜索Git Bash并启动

下面是一些基本配置：

可以使用`git config --global --list`来查看所有配置

- 配置用户信息，这些信息将作为提交者信息显示在更新历史中

  ```
  $ git config --global user.name "用户名"
  $ git config --global user.email "电子邮件"
  ```

- 为Git命令设定别名

  ```
  用co代替checkout命令
  $ git config --global alias.co checkout
  ```

- 在Windows中将含非ASCII字符的文件名正确显示

  `$ git config --global core.quotepath off`

- 制定commit时默认打开的文本编辑器

  `$ git config --global core.editor vim`

## 2.2 创建本地数据库

创建一个文件夹作为数据库，之后cd 到该目录下，输入

`git init`

会显示

`Initialized empty Git repository in 指定目录`

## 2.3 在本地提交文件

将想要提交的文件放在数据库目录下

1. 执行`git status`

​	会显示Untracked files，这说明这些文件目前不是历史记录对象

2. 添加索引：

   需要先把文件加入索引，以追踪它的变更

   `git add filename`

   `git add .`可以把所有文件加入索引

3. 提交文件

   `git commit -m "test.txt"`
   
   执行后系统会打开一个编辑器让输入提交信息

可以通过`git log`查看提交记录，输入q退出，输入`gitk`打开GUI界面

# 3. 远程共享数据库

push：将本地数据库修改记录共享到远程数据库

clone：下载远程数据库的全部内容

pull：把远程数据库的内容更新到本地数据库

## 3.1 下载GitHub CLI

[GitHub CLI](https://docs.github.com/zh/get-started/getting-started-with-git/caching-your-github-credentials-in-git)

安装GitHub CLI之后，在命令行中输入`gh auth login`，之后按照提示进行操作

## 3.2 连接远程数据库

打开git bash，先cd到本地Git仓库下

**使用remote指令操作远程数据库**

- `git remote`

  列出已经存在的远程数据库

- `git remote show name`

  显示名字为name数据库的信息

- `git remote add name url `

  添加远程数据库，指定远程数据库的名称name和数据库地址url

  例如：`git remote add hello-world https://github.com/sssazer/hello-world`

- `git remote remove name`

  删除远程数据库

- `git remote rename old_name new_name`

  修改数据库名称

- `git remote set-url name new_url`

  将指定名称的数据库地址改为new_url

- `git remote get-url name`

  查看数据库的地址

## 3.3  向数据库push文件

`git push -u name branch`

向名字为name的数据库的branch分支中push文件

如果指定-u选项，则下一次推送时可以省略分支名称

**push时遇到的问题**

- push前记得要先将修改过的本地文件提交

- 要确保push的分支是本地打开的分支，否则报错

  Git默认推送分支是master，而GitHub默认主分支是main

  ```shell
  // 错误信息
  error: src refspec master does not match any
  error: failed to push some refs to 'https://github.com/sssazer/hello-world'
  ```

- 在执行pull之后，进行下次push之前，如果这期间有其他人往数据库进行push，那么你的push将被拒绝，因为如果直接push，就会覆盖掉别人的push

  ```shell
  // 错误信息：
  ! [rejected]        master -> master (fetch first)
  error: failed to push some refs to 'https://github.com/sssazer/hello-world'
  hint: Updates were rejected because the remote contains work that you do
  hint: not have locally. This is usually caused by another repository pushing
  hint: to the same ref. You may want to first integrate the remote changes
  hint: (e.g., 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
  ```

  这种情况下，需要先将数据库内容pull下来，解决冲突并合并，再push（pull下来之后会先尝试自动合并，失败了再让你解决冲突）

- Connection was reset

  可能是代理的问题，换个代理或者开关一下代理或者打开全局，多试几次就好了

  ```shell
  // 错误信息
  fatal: unable to access 'https://github.com/sssazer/hello-world/': Recv failure: Connection was reset
  ```

## 3.4 clone数据库

即 将整个远程数据库文件下载到本地

`git clone name`

或 `git clone url`

## 3.5 从远程数据库pull

`git pull name branch`

# 4. 分支的运用

## 4.1 什么是分支

从某个提交创建分支相当于：我想基于这个提交以及它所有的父提交进行新的工作

## 4.2 本地分支操作

### 4.2.0 提交点

当执行`git commit`之后，Git会自动创建一个提交点，并将分支指向该提交点。每个提交点都有一个独一无二的哈希值。

### 4.2.1 创建分支

`git branch 分支名`

直接输入`git branch`可以查看所有分支

还可以`git fetch origin :分支名`来创建分支

### 4.2.1 删除分支

`git branch -d branch_name`

### 4.2.2 切换分支

`git checkout 分支名`

也可以直接创建并切换到分支

`git checkout -b 分支名`

### 4.2.3 合并（提交）分支

**merge**

假如现在在main分支中，执行

`git merge bugFix`

会创建一个新的提交点，这个提交点同时继承于main和bugFix，同时main分支指向新的提交点，原bugFix分支不动

```shell
如果再切换回bugFix分支，执行合并main分支的命令
git checkout bugFix
git merge main
由于main继承于bugFix，因此Git只是简单的将bugFix移动到main
```

**rebase**

使用rebase将创造线性的提交历史。它会将两个分支的提交点合并在一条线上，得两个分支看起来像是线性开发的。

比如此时在main分支下，执行

`git rebase bugFix`

会新建一个结点，这个结点显示上只继承于bugFix，实际上保存了bugFix和main合并后的内容，之后将main分支移至新结点（main分支原来指向的结点依然存在）。

`git rebase --interactive HEAD~3` 或 `git rebase -i HEAD~3`

用于重新选择和排列当前分支上提交记录

加上-i参数时，Git会打开一个UI界面（实际上是一个文本编辑器），列出从HEAD本身开始往前数三个提交记录。你可以移动或删除这些记录，之后Git会根据你的选择和顺序来创建一个新的分支并将当前分支移动到新分支上

### 4.2.4 HEAD

HEAD是一个对当前检出记录的符号引用——其实就是指向了当前正在操作的提交记录

HEAD默认指向分支，当切换分支或者执行提交时，HEAD会跟着分支移动

**查看HEAD**

可以通过`cat .git/HEAD `查看当前HEAD的指向

如果HEAD指向分支名，也可以用`git symbolic-ref HEAD`查看指向。

如果HEAD处于分离状态（即指向某个具体的提交记录而不是分支）那么只能用`cat .git/HEAD`（即查看.git目录下的HEAD文件）来查看HEAD指向，且HEAD的指向是一串哈希值

**分离HEAD**

HEAD默认指向分支，分离HEAD就是让HEAD指向某个具体的提交记录而不是分支名，执行

`git checkout 哈希值`

可以分离HEAD，之后完之后会提示`You are in 'detached HEAD' state`。哈希值只用输入前几位即可

再执行`git checkout 分支名`

就可以让HEAD再次指向分支

### 4.2.5 在提交树中移动

通过哈希值指定提交记录很不方便，所以Git引入了相对引用，通过相对引用我们可以操作HEAD在提交树中自由移动

使用相对引用可以从一个起点（某个分支或者HEAD当前位置）开始，向前寻找指定提交点

- `^num`向上移动一个提交记录，在有多个父提交的情况下，num用来指明选择第几个父提交**（不知道顺序是按照什么）**
- `~num` 向上移动num个提交记录，不写num就是向前移动一个

比如，`git checkout master^`，将HEAD移动至master分支的前一个提交记录

`git checkout HEAD~4`，将HEAD向前移动4个提交记录

### 4.2.5 强制移动分支

`git branch -f main HEAD~3`

将main分支强制移动至HEAD往前3个提交记录的位置

如果分支名不存在，会创建对应分支并指向指定位置

移动完分支之后原有的提交点都还会保留

### 4.2.6 撤销变更

**本地reset**

假设当前在main分支下，使用

`git reset main~1`

将main分支reset到main的前一个提交点（即撤销main最近一次的提交

撤销之后，后面的提交点并没有删除，只是处于未加入索引的状态

这种撤销只能在本地使用，无法同步到远程，如果想撤销远程数据库的提交，需要使用revert

**远程revert**

`git revert main`

表示撤销main分支当前提交

会创建一个新的提交记录，这个提交记录保存的是main分支的前一个提交记录，然后会将main分支移动到新的提交记录上

### 4.2.7 复制提交

当前在main分支下

`git cherry-pick bugFix side~`

此时会在main分支下创建两个新的提交，其中前面的提交复制bugFix分支的当前提交，后面的提交复制side分支的上一个提交。main分支指向后面的提交，原bugFix和side分支不受影响

### 4.2.8 tag

通过给提交加tag，可以将某个特定的提交命名为里程碑，然后就可以像分支一样引用了

tag不会随着新的提交而移动，也不能切换到某个标签上进行修改提交，tag就像是提交树上的一个锚点，标识了某个特定的位置

**添加tag**

`git tag tag名 要添加tag的提交点`

不写提交点默认是在当前HEAD的位置添加tag

**定位最近的tag**

`git describe ref`

寻找离指定ref引用最近的tag

ref可以是分支、tag，不指定的话默认是HEAD当前位置

输出结果为：`<tag>_<numCommits>_g<hash>`

- tag：表示离ref最近的标签
- numCommits：表示ref与tag相差多少个提交记录
- hash：表示给定的ref所表示的提交记录的哈希值的前几位

如果ref上刚好有某个标签，则只输出标签名称

## 4.3 远程仓库操作

向Github仓库中远程操作时，需要给GitBash设置代理

`git config --global http.proxy http://127.0.0.1:7890`

`git config --global https.proxy https://127.0.0.1:7890`

取消git bash的代理：

`git config --global --unset http.proxy`

`git config --global --unset https.proxy`

### 4.3.0 远程分支

**什么是远程分支**

当我们要对远程仓库进行操作时，其实是对远程仓库中的某一个分支进行操作，所以我们执行操作时需要指定远程分支(**upstream branch**)。用远程仓库/分支名的格式指定，比如`origin/main`表示远程仓库origin中的main分支

我们可以将本地的main分支和远程仓库的main分支关联起来，就可以不用每次都指定要操作的远程分支，产生以下效果：

- 当我们在本地的main分支上pull时，就会拉取远程仓库的main分支的内容到本地main分支，
- 当我们在本地的main分支上push时，就会将本地main分支新的提交点同步到远程的main分支上

**远程分支的作用**

当建立起本地分支`main`与远程分支`origin/main`的连接之后，Git会在本地仓库创建一个与远程分支同名的锚点`origin/main`，用来保存上次通信时远程仓库对应分支的状态，称为**remote-tracking branch**，这样的话下次再通信时就知道从哪里开始检查本地仓库与远程仓库的差异

当在本地commit时，只有本地main分支会移动，而origin/main分支不会移动，同样的，也无法使用branch -f命令强制移动远程分支。实际上，当使用`git checkout origin/main`命令时，会直接进入HEAD分离状态，也就是说本地的`origin/main`只是一个锚点而并不是一个分支。

当执行push或pull操作时，相当于与远程仓库进行了一次通信。于是远程分支会移动，去记录本次通信时远程仓库的状态

**建立本地分支与远程分支的联系**

- `git clone`

- `git push --set-upstream origin main`

  将本地main分支与远程仓库的main分支做关联，并执行push操作

- `git branch --set-upstream-to=origin/master master`

  将本地master分支与远程仓库的master分支做关联

- `git branch -u origin/main master`

  让本地master分支与远程分支origin/main关联

- `git checkout -b bugFix origin/bugFix`

  新建并切换到bugFix分支，并且让side分支跟踪远程分支origin/bugFix

可以通过`git branch --unset-upstream branchname`来解除分支的关联关系，不写branchname默认是当前所在分支

### 4.3.1 git clone

当使用`git clone`之后，会将远程仓库的所有提交点和所有分支都复制到本地仓库，并将本地仓库分支与远程仓库对应分支建立联系。

### 4.3.2 git fetch

**无参数用法**

使用无参数的`git fetch`之后，会更新所有本地对应远程的锚点

但是本地分支不会改变，也就是说fetch只是将远程仓库中更新的数据下载下来了，但是并没有修改本地文件，没有将本地仓库进行同步

可以通过`git cherry-pick origin/main`、`git rebase o/main` 、`git merge o/main`等操作手动移动本地main分支，进行手动同步

**带参数用法**

使用`git fetch origin main`可以只更新origin/main分支

`git fetch origin <source>:<destination>`，从远程仓库的source分支下载到本地destination分支，这样的话会直接移动本地的destination分支，并且不会移动锚点

### 4.3.3 git pull

`git pull` = `git fetch` + `git merge origin/main`

假如main关联了origin/main，则在分支main上执行git pull之后发生的操作是这样的

- 将origin/main记录的位置（上次与远程仓库通信时远程仓库的状态） 和 当前origin仓库中main分支的位置（当前远程仓库的状态）之间的所有提交点下载下来，然后将origin/main移动到最新的位置
- 将本地的main分支 与 origin/main分支 执行merge操作

`git pull` 默认是使用merge进行合并，可以使用`git pull --rebase`来在第二步中使用rebase的方式合并

**无参数用法**

当不指定参数时，执行命令时所在的分支必须已经关联远程分支，比如现在 在main分支，且main分支已经关联origin/main分支

**带参数用法**

`git pull origin main`

**可能出现的错误**

- `fatal:refusing to merge unrelated histories`

  当要拉取的远程分支和目前所在的本地分支没有任何关系时会报错。比如你在本地新建分支并进行了一些提交，想要拉取一个远程分支，而那个远程分支本身也有提交。那么这两个分支就毫无关系。

  解决办法：在pull的末尾加上`--allow-unrelated-histories`

  `git pull --allow-unrelated-histories`

### 4.3.4 git push

**无参数用法**

直接执行`git push`，需要执行命令时在本地的一个分支上

- 如果该分支有关联的**远程分支**——会同步到远程仓库中与该分支关联的**远程分支**上
- 如果该分支没有关联的远程分支——会报错：`fatal: The current branch readme-edits has no upstream branch.`

**带参数用法：**

`git push <remote> <place>`

当push携带参数时，当前可以在任意分支或提交点上

- `git push origin main`

  要提交的本地分支 和 要提交到的远程仓库中的分支 名相同时使用

  表示 将本地main分支的内容push到远程仓库origin中的main分支上

- `git push origin <source>:<destination>`

  要提交的本地分支 和 远程仓库的分支名不同时使用

  表示将本地source分支push到远程仓库中destination分支上

  ```
  source实际上可以是任何一个Git能识别的位置
  比如：分支main，或任意一个提交点（比如main~1）
  
  destination可以是一个在远程仓库中不存在的分支，这样的话远程仓库会帮你创建该分支并推送
  ```

  示例：`git push origin bugFix~1 main`

  表示将本地仓库bugFix分支的上一个提交点 push到 远程仓库

  origin的main分支中

以上两种用法只能成功push到指定位置，但是不会帮你创建并追踪远程分支，可以加上`--set-upstream`来创建本次提交的分支关联关系。

比如`git push --set-upstream origin bugFix`，此时会在本地创建origin/bugFix分支，并让本地的bugFix分支追踪它

### 4.3.5 删除远程分支

`git push origin :foo`删除远程仓库的foo分支
