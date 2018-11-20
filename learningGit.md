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

```vb
$ git diff
diff --git a/readme.md b/readme.md
index b3e526a..0fa1c8f 100644
--- a/readme.md
+++ b/readme.md
@@ -1,4 +1,6 @@
 ```
 Git is a distributed version control system.
 Git is free software.
-```
\ No newline at end of file
+```
+
+       This is my readme.
\ No newline at end of file
```

​	把readme.md`git add`后还可以再通过`git status`对仓库状态进行查看，可看到readme.md待提交；

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

​	嫌输出太多，可加上参数`--pretty=online`	

```vb
$ git log --pretty=oneline
a554c5921ca18f1f5d6be884ebe465b7ab74c4af (HEAD -> master) append GPL
521a1f542bc4fd37ab516d0d51f643bd9003b3ae some modify
61b3b241888b899d38fd3aab4778b589876673a1 readme
eed607f98c488e016a0230b6bb97ac2fa934e04d first commit
```

​	git中的当前版本    用`HEAD`表示；那么当前版本的上个版本就是`HEAD^` ，上上个版本是`HEAD^^`;那么网上100个版本就不能写100个^，而是 `HEAD~100`;

​	

​	要进行版本退回要使用的命令是   `git reset`;然后通过  `cat readme.md`查看

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



