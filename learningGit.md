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

