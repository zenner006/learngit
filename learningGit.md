# Git教程

## 一、Git简介

### 1.1 安装Git

​	在三种不同的系统当中安装Git；

​	唯一需要记得的命令是：

​	`git config --global user.name "Your name"`

​	`git config --global user.email "email@example.com"`

​	这是进行全局设置的命令

### 1.2 创建版本库

​	版本库（repository)里的文件都会被Git管理起来。

​	Git中的文件操作：

```visual basic
$ mkdir learngit
$ cd learngit
$ pwd	# 显示当前目录
/Users/michael/learngit
```

​	把当前文件夹变成Git可管理的仓库（repository）

```vb
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

​	添加（add）文件进仓库，可添加好几个文件；然后提交（commit)：可将添加进的全部提交上去

```vb
$ git add learngit.md
$$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

## 二、时光穿梭机

​	要查看仓库中文件的状态可以用`git status`来查看：

```vb
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.md

no changes added to commit (use "git add" and/or "git commit -a")
```

​	上面的信息告诉我们readme.md修改过了但还没提交；给git status可以掌握仓库的状态

​	通过`git diff `查看到底修改了什么：

+ ```vb
  $ git diff
  diff --git a/readme.md b/readme.md
  index b3e526a..0fa1c8f 100644
  --- a/readme.md
  +++ b/readme.md
  @@ -1,4 +1,6 @@
  
   Git is a distributed version control system.
   Git is free software.
  -```
  \ No newline at end of file
  +```
  +
  
  - This is my readme.
    \ No newline at end of file
  ```

		把readme.md`git add`后还可以再通过`git status`对仓库状态进行查看，可看到readme.md待提交；

### 2.1 版本退回

​	可以查看自己的提交历史：

```vb
$ git log
commit a554c5921ca18f1f5d6be884ebe465b7ab74c4af (HEAD -> master)
Author: bingyi <juyi00622@gmail.com>
Date:   Tue Nov 20 11:32:17 2018 +0800

    append GPL

commit 521a1f542bc4fd37ab516d0d51f643bd9003b3ae
Author: bingyi <juyi00622@gmail.com>
Date:   Tue Nov 20 11:27:04 2018 +0800

    some modify

commit 61b3b241888b899d38fd3aab4778b589876673a1
Author: bingyi <juyi00622@gmail.com>
Date:   Tue Nov 20 11:16:38 2018 +0800

    readme

commit eed607f98c488e016a0230b6bb97ac2fa934e04d
Author: bingyi <juyi00622@gmail.com>
Date:   Tue Nov 20 11:14:03 2018 +0800

    first commit
```

​	嫌输出太多，可加上参数  `--pretty=online`	

```vb
$ git log --pretty=oneline
a554c5921ca18f1f5d6be884ebe465b7ab74c4af (HEAD -> master) append GPL
521a1f542bc4fd37ab516d0d51f643bd9003b3ae some modify
61b3b241888b899d38fd3aab4778b589876673a1 readme
eed607f98c488e016a0230b6bb97ac2fa934e04d first commit
```

​	git中的当前版本    用`HEAD`表示；那么当前版本的上个版本就是`HEAD^` ，上上个版本是`HEAD^^`;那么网上100个版本就不能写100个^，而是 `HEAD~100`;

​	

​	要进行版本退回要使用的命令是     `git reset`;然后通过  `cat readme.md`查看

```vb
$ git reset --hard HEAD^
HEAD is now at 521a1f5 some modify
$ cat readme.md
​```
Git is a distributed version control system.
Git is free software.
​```

        This is my readme.
```

​	果然 第二行后面的词不见了；原文件也自动变为了这一版；

​	若是想回到最新版中，趁命令行窗口没有关可以用他的commit id（前几位就可以了，git会自动去找）

```vb
$ git reset --hard a554
HEAD is now at a554c59 append GPL
```

​	但是当命令行关闭后，还想回到之前的最新版本，还找不到commit id，这时候还可以   `git reflog`

```vb
$ git reflog
a554c59 (HEAD -> master) HEAD@{0}: reset: moving to a554
521a1f5 HEAD@{1}: reset: moving to HEAD^
a554c59 (HEAD -> master) HEAD@{2}: commit: append GPL
521a1f5 HEAD@{3}: commit: some modify
61b3b24 HEAD@{4}: commit: readme
eed607f HEAD@{5}: commit (initial): first commit
```

​	找到之前的所有流程

### 2.2 工作区和暂存区

​	工作区：就是自己对文件进行修改编辑的地区；也就是文件夹内

​	暂存区：就是  `git add`命令之后文件存放的地区

​	版本库：在创建Git版本库时，就仅仅创建了一个master分支，

​	master分支：  `Git commit`命令就是把暂存区中的内容添加到master分支上。

​	目前在这里看到master分支，也就是说之后能自己开辟自己的分支喽？

### 2.3 管理修改

​	git的管理针对的是修改；所谓修改就是添加删除，只要对文件有改动都乐意算作修改。即使是创建了一个新文件也会算作是一次修改。

​	于是添加到暂存区的也是修改，提交到分支上的也是修改。把一个修改添加到暂存区，再对这个文件进行修改，后边的这次修改没有被添加进去。通过状态查看还是有改动，需要再次添加。

### 2.4 撤销修改

​	再对一项进行修改后，要将其撤销。用git checkout -- file来将其撤销到最近一次git add或git commit后的状态。也就是刚修改完，还没添加或者提交，可以恢复修改

```vb
$ git checkout -- file	# "--"很重要，没有它命令是另外的意思。
```

​	要是不小心把错误修改的文件已经添加到了暂存库；需要撤销的命令是

```vb
$ git reset HEAD file
```

​	reset命令可以回退版本也可以把暂存区的修改退回到工作区。用HEAD表示最新的版本。这时再查看状态，暂存区是干净的，工作区有修改。那么即可使用上第一条命令进行修改清除。

### 2.4 删除文件

​	对一个文件的删除可以用linux中普通的删除命令，但是也要用git中的删除命令删除，并且提交。

```vb
$ git add test.txt
$ git commit -m "add test.txt"
[master bcd5447] add test.txt
 2 files changed, 1 insertion(+)
 create mode 100644 test.txt
$ rm test.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   learningGit.md
        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
[master 983a74a] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt
```

## 三. 远程仓库

​	可以把自己的代码克隆到远程库github当中去。需要的操作是在电脑与github间建立一个ssh 加密，让github知道某一次添加是你自己添加的。

​	通过下面的这个命令生成一个ssh 密钥，生成一个私钥一个公钥；

```vb
$ ssh-keygen -t rsa -C "youremail@example.com"
```

​	然后登陆github，再setting中新建ssh把公钥的内容添加进去；

​	当然可以在不同的电脑上添加不同ssh密钥。于是就可以共同和github协作。

### 3.1 添加远程库

​	在这一小节学习在github上建库。github上的库既可以往本地克隆，也可以把本地库与之关联。

​	把本地库关联到远程库中的命令：

```vb
$ git remote add origin git@github.com:zenner006/gitskill.git

# 上面这句有可能报错
fatal: remote origin already exists.
# 用下面这句来解决：
$ git remote rm origin	# origin到底是一个怎样的存在呢？
```

​	接着把本地库所有内容推送都远程库中：

```vb
$ git push -u origin master
```

​	由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。 

​	从现在起，只要本地作了提交，就可以通过命令： 

```vb
$ git push origin master
```

### 3.2 从远程库克隆

​	想要啥包，就从github上直接下

```vb
$ git clone git@github.com:michaelliao/gitskills.git
```

​	clone后面的这部分地址在github上可以直接拿到



​	



