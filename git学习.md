# Git Learn

## 创建本地库

进入相对应的文件夹然后输入

```git
git init
```

将一个文件添加到`Git`库中

```git
git add + 'name'
git add + 'name1' 'name2' #提交多个文件
```

将一个文件提交到仓库

```git
git commit -m '一些描述的话'
```

`-m`后边输入的是本次提交的说明

一定要先`add`在`commit`

---

## 时光穿梭机

`git status`可以让我们时刻掌握仓库的当前状态

```git
git status
```

 `git diff`

可以用来查看difference

---

## 版本回退

`git log`

可以告诉我们历史记录

`git reset --hard HEAD^`

回退到上一个版本，有几个`^`代表回到之前的几个版本

也可以直接写成`git reset --hard HEAD~100`

`cat +name`

直接在命令号显示文件

重置之后还是可以复原的，前提是找到之前的版本号

`git reset --hard 版本号的前几位`

`git `会自动的补全

`git reflog`

记录了每一次的命令，里边会有每次版本的版本号

---

## 工作区和暂存区

### 工作区

就是我们看见的目录

### 版本库

工作区有一个隐藏的目录`.git`，这就是版本库

里边最重要的是stage（或者index）的暂存区，还有git为我们创建的第一个分支`master`，以及指向`master`的指针`HEAD`

`git add `实际上就是把文件提交到暂存区，`git commit`把暂存所有的文件提交到当前的分支

---

## 修改管理

通过暂存区我们知道`git`比其他的版本管理软件优秀的原因是，`git`跟踪管理的是修改而不是文件。

`commit `只会把当前在缓存区的内容提交到`master`

如果`add`之后修改的的没有`add`话，只会提交上次`add`到暂存区的那一部分，后边没有添加到暂存区的那一部分是不会被提交的，只要把第二次的修改也`add`到暂存区，然后`commit`就可以了

---

## 撤销修改

`git checkout +文件名`

可以撤销修改，也就是`git checkout`可以丢弃工作区的修改，但是有两种状态

第一种：自行修改，没有放到暂存区，这个时候撤销修改就回到和版本库一模一样的状态

第二种：添加暂存区之后，又有了修改，这个时候撤销会回到添加暂存区之后的状态

这个命令后边`--`很重要，没有这个的话就成了另外的一个命令

如果你已经`add`添加到了暂存区，没有提交修改

`git reset HEAD +name`

可以帮我们把暂存区的内容撤销掉

---

## 清除文件

直接rm清除之后，工作文件和版本文件不一致，这个时候由两个选择

第一个：清除

`git rm +name`直接清除，然后用`git commit -m +说明`提交到分治

第二个：恢复

`git checkout -- +name`恢复到之前的状态

---

## 远程仓库

Git是分布式版本控制系统，同一个Git库可以分布在不同的机器上。

### **`Github`操作部分**

首先在GitHub创建一个仓库repository

在本地生成秘钥

```git
ssh-keygen -t rsa -C "xk20shd@163.com"
```

将这个密钥的`id_rsa.pub`添加到GitHub中，过程如下

1. GitHub中找到setting，然后进入`SSH and GPS keys`
2. 点击`New ssh key`
3. 输入`Title`，`key`部分将`id_rsa.pub`的内容复制到`key`中，注意复制的格式：
   * 文件开头的`ssh-rsa`是有一个换行的
   * 可以直接`cat id_rsa.pub`，输出到命令行然后复制
4. 最后点击`Add SSH key`

### **`终端上的`操作部分**

1. 进入`.ssh`目录，用上边的指令生成密钥
2. 在`.ssh`目录如果有`config`文件，则不用生成，没有的话需要自己手动添加一个`config`文件

```git
Host github.com
User xk20shd@163.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

然后输入下边两句进行激活

```git
git config --global user.name shenhongdeng
git config --global user.email xk20shd@163.com
```

3. 把本机的`git`仓库和这个`SSH key`关联上，可以用如下命令在本地仓库中执行

```git
ssh-add 自己的id-rsa文件地址
```

然后可以用`ssh git@github.com`进行验证，出现下边的话说明你成功了

```git
Hi shenhongdeng! You've successfully authenticated, but GitHub does not provide shell access.
```



### **链接远程仓库**

```git
git remote add origin git@github.com:shenhongdeng/learngit.git
```

远程仓库的名字一般都会叫做`origin`我们也可以自己改名字，但是`origin`一看就知道是远端的名字

### **把本地仓库PUSH到GitHub**

```git
git push -u origin master
```

实际上是把当前分支的`master`推送到远端，我们增加参数`-u`的目的不仅仅是为了将本地的`master`推送到远端，还是为了将本地的`master`和远端的`master`关联起来，这样我们后边推送或者拉去时命令就会就可以简化。

现在我们将本地提交到github只需要写

```git
git push origin master
```

### 清除远端的库

在清除远端的库之前，我们要先用`git remote -v`查看远程的库信息

然后根据名字进行清除c

```git
git remote rm origin
```

分布式的好处在于，没有网的时候也可以正常的工作，等到有网了再提交一次推送就完成了同步

### 从远端克隆

不用链接，直接输入下边

```git
git clone git@github.com:shenhongdeng/learngit.git
```



---

## 分支管理

我们可以在master这个主分支下边开一个自己的分支，这样在自己没有开发完全的时候就不会影响到别人工作，等到自己开发完了，可以再一次性合并到原来分支上边

`git`分支的优点就是在于快

---

## 创建于合并分支

每次提交，Git都把他们串联成为一条时间线，这条时间线就是一个分支。到目前为止，只有一条时间线主线，在Git里，这个分支叫主分支，就是`master`分支，`HEAD`严格上来说并不是指向提交的，而是指向`master`的，`master`才是指向提交的。`HEAD`指向的是当前的分支.

Git创建一个新的分支是很快的。只需要增加一个新的`dev`指针，修改一下`HEAD`执行`dev`，从现在开始，我们的提交都提交到`dev`分支上，我们新提交一次之后`dev`向后边位移一个，而`master`指针不变。

当我们在`dev`上完成了工作，就可以吧`dev`合并到`master`上，Git的合并很简单，直接把`master`指向`dev`的当前提交

### 创建分支

```git
git checkout -b dev
```

`git checkout -b`中`-b`表示切换分支，相当于如下的两条命令

```git
git branch dev
git checkout dev
```

`git branch`

查看当前的分支

`git checkout +name 或者 git switch +name`

切换分支

`git merge dev`

把`dev`分支的工作成果合并到`master`上

`git branch -d +name`

清除分支，清除的时候要位于master分支上

----

## 解决冲突

我们在分支上做了修改，同时在`master`上也做了修改，这个时候，我们把分支合并到`master`上，这个时候就会爆发冲突，这个时候我们只能手动的来更正冲突。

`git log --graph --pretty=oneline --abbrev-commit`

这个命令可以看到分支合并图

---

## 分支管理策略

一般情况下，Git用`Fast Forward`模式，但是这种模式下，清除分支之后，会丢掉分支信息。我们可以用 `--no-ff`的蚕食禁用`Fast Foward`模式。

`git merge --no-ff -m "merge with no-ff" dev`

中间`-m "+内容"`是说明部分

### 分支策略

master一般非常的稳定，仅仅用来发布新的版本，而不是在上边干活。

一般干活都在`dev`上，要用的时候，直接把`dev`合并到`master`上。

---

## bug分支

我们可以临时把工作现场储存起来，等以后回复现场之后继续工作

`git stash`

`git stash list`

查看我们储存的现场在哪

回复现场的两个办法

* `git stash apply` 但是stash的内容并没有清除，要用`git stash drop` 清除
* `git stash pop`恢复现场并且清除

但是我们修改的是`master`上bug，并没修改我们在`dev`分支上的bug，用下面的语句可以将`dev`上对应的部分也修改

```git
git cherry-pick 40a01d5
```

注意后边的`40a01d5`是提交bug修改分支时的标号。

---

## Feature分支

添加新的功能，一般最好新建一个`feature`分支，在上边开发，完成后合并，最后清除该分支。

如果正常合并的话和我们修改`bug`的命令一样，如果要撤销修改，并且清除全部信息。可以用下边的语句来强行清除分支

```git
git branch -D +分支名称
```



---

## 多人协作

`git remote`

查看远程连接的仓库

`git remote -v`查看详细信息

### 推送分支

把该分支上的所有的本地提交推送到远程库中。推送的时候要指定本地的分支，这样Git会把该分支推送到远程指定的分支上。

多人协作的模式

* 首先识图用`git push origin +分支名`推送自己的分支
* 如果推动失败，因为远程分支比你本地的更加的新，需要先试用`git pull`试图合并
* 如果合并有冲突的话，则解决冲突，并且在本地提交
* 没有冲突或者解决冲突之后，再用`git push origin +分支名`推送

---

## 标签管理

标签（`tag`）可以理解为版本库的一个快照，在发布一个版本的时候，我们通常在版本库中打一个标签。这样就唯一确定了打标签时刻的版本。将来要取那个标签的版本，就是把那个打标签的时刻的历史版本取出来。

实际上一个标签就是指向某一个commit的指针。和分支很像，但是分支是可以移动的，但是标签不能移动。

`tag`可以理解为`commit`之后一个方便人记住的名字。

### 创建标签

```shell
git tag v1.0
```

### 查看标签

注意这里边的标签不是按照时间排列的，是按照字母序排列的

```shell
git tag
```

### 给之前的补加标签

``` shell
git tag v0.9 f52c633 # 后边的是版本号
```

### 查看标签对应信息

这个会显示对应`tag`的版本的信息

```shell
git show v1.0 # 后边的是版本号
```

### 删除标签

```shell
git tag -d tag_name 
```

---

## 自定义Git

### 让Git显示颜色

```shell
git config --global color.ui true
```



