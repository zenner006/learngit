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

## 四、分支管理

​	分支管理就是合作时，把总线分开。一个在对文件做一件事，另一个对文件做另外一件事，最后将这两个分支可以整合到一起来。（还不是很理解为什么要这样子做）

### 4.1 创建与合并分支

​	Git里主分支叫做master。HEAD指向master。master是指向提交的的。所以HEAD指向得是当前分支。

​	当创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上。

​	但在此之后，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往后移动一步，而master不变；

​	在dev上的工作完成，把dev合并到master上。Git的合并就是把master直接指向dev的当前提交，完成合并。

​	合并完成后，要删除dev分支，就直接把dev指针删除就可以。

​	

​	创建dev分支，然后切换到dev分支:

```vb
$ git checkout -b dev
Switched to a new branch 'dev'
```

​	`git checkout`命令加上`-b`参数表示创建并切换，相当于一下命令：

```vb
$ git branch dev	# 创建分支
$ git checkout dev	# 切换分支
Switched to branch 'dev'
```

​	在一个文件下进行改动，进行添加并提交后就提交到了dev分支。切换回master分支以后，

```vb
$ git checkout master
Switched to branch 'master'
```

​	可以看到刚刚改动的不见了，也就是说，你新立了一个路径，但master很沉稳，他不为所动。除非你把已经改动的进行合并；

```vb
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

​	这里提示是fast-forward，就是直接改动指针，这样很快。

​	然后可以删除分支：

```vb
$ git branch -d dev		# 删除分支
Deleted branch dev (was b17d20e).
$ git branch	# 查看分支
* master
```

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 4.2 解决冲突

​	新建分支，修改文件并提交到该分支上。再在master分支上进行修改；

![1543027370160](C:\Users\bingyi\AppData\Roaming\Typora\typora-user-images\1543027370160.png)

​	于是就有上图的情况master和feature1都有新的提交，这种情况下无法直接合并两个分支，必须解决冲突。

```vb
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

​	然后需要在文件中手动修改冲突，得到最后版本后，进行提交这次也就是下图：

![1543027692314](C:\Users\bingyi\AppData\Roaming\Typora\typora-user-images\1543027692314.png)

​	用`git log`查看分支合并情况

```vb
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

​	最后删除分支；

### 4.3 分支管理策略

​	Git用`Fast forward`模式下，删除分支，会丢掉分支信息。在强制禁用

​	若是有了一个分支master，再创建一个分支dev。我们在dev下进行修改，如果把dev上的修改向master分支上进行合并，用`Fast forward`模式则会将dev分支消除掉。那么我们想要强制禁用`Fast forward`，也可以用`--no-ff -m`参数，表示禁用`Fast forward`

```python
$ git checkout -b dev	# 创建新的分支dev
Switched to a new branch 'dev'

$ git add readme.txt 	# 修改文件后进行提交
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
    
$ git checkout master	# 切回到master
Switched to branch 'master'

$ git merge --no-ff -m "merge with no-ff" dev	# 这里把dev合并了

$ git log --graph --pretty=oneline --abbrev-commit 	#通过log看到dev处的提交
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed

$ git branch	# 可以看到dev分支还是存在的
  dev
* master
```

​	**分支策略**：master通常都是非常稳定的，用来发布新版本。通常干活都在dev分支上。

### 4.4 Bug分支

​	Bug是软件开发过程中的常事。在GIt中每一个Bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后把临时分支删除。

​	场景：想要修复bug（从master上分支一个bug分支），但发现分支dev上进行的工作还没有提交

​	没关系，用`git stash`命令，可以保存dev现场

```vb
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge

$ git status	# 是干净的~！
On branch dev
nothing to commit, working tree clean
```

​	然后切回到master，并从master创建临时分支修改Bug：

```vb
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

​	对其进行修改以后就可以进行提交：

```vb
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

​	切回master，完成合并，最后删除`issue-101`:

```vb
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git branch -d issue-101
Deleted branch issue-101 (was 7f8fb80).
```

​	修复完Bug后就可以切回dev,，检查状态。

```vb
$ git checkout dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean	# 状态是干净的
```

​	发现状态是干净的，需要看储存状态里有没有：

```vb
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

​	需要将工作现场恢复。有两个办法：

​	`git stash apply`: 可以恢复，但stash list中还仍然保存着。需要用`git stash drop`来删除。

​	`git stash pop` : 回复的同时把stash内容也删除。

​	`git stash apply stash@{0}`：另外一种使用