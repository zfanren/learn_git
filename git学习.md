# Git

##1.概念

### 1.1本地仓库

![](image\工作区缓存区版本库.png)

**工作区：**电脑中实际能看到并使用的目录；

**暂存区：**stage或index，一般存放在“.git目录下的index文件中”，也成为索引；

**版本库：**工作区的隐藏目录.git。

HEAD是指向master的游标，可以用master来替换。

### 1.2远程仓库

![](image\本地仓库与远程仓库.png)

## 2.Git命令行操作

###2.1本地仓库初始化

```
$ git init
```

当前目录作为仓库，创建一个git仓库；

```
$ git init nerepo
```

新建nerepo文件夹（自定义），将nerepo作为仓库文件夹，创建一个仓库。

### 2.2设置签名

- 形式

   用户名、email地址

- 作用

  区分身份

- 命令

  项目级别/仓库级别：仅在当前本地仓库范围内可用；

~~~
$ git config user.name tom_go
$ git config user.email wxb@gmail.com
~~~

​	系统用户级别：登录当前操作系统的用户范围；

~~~
$ git config -global user.name tom_go
$ git config -global user.email wxb@gmail.com
~~~

​	优先级：就近原则，优先使用项目级别签名，不允许二者都没有。

###2.3基本操作

####2.3.1状态查看操作

查看工作区、暂存区状态

~~~
$ git status
~~~

```
On branch master #分支

No commits yet #本地库没有任何文件

Untracked files:

  (use "git add <file>..." to include in what will be committed) #可以提交的文件

        good.txt

nothing to commit (create/copy files and use "git add" to track) #暂存区没有任何文件
```

####2.3.2添加操作

添加文件到暂存区

```
$ git add good.txt
```

```
warning: LF will be replaced by CRLF in good.txt. #操作系统将换行符被更改了
The file will have its original line endings in your working directory 
```

####2.3.3提交操作

提交到本地库

```
$ git commit good.txt
```

输入本次提交的备注

```
warning: LF will be replaced by CRLF in good.txt.
The file will have its original line endings in your working directory 
[master (root-commit) f93b99e] my frist file  #分支 行为 版本号 添加备注
 1 file changed, 1 insertion(+) #一个文件被修改了 创建了一个文件
 create mode 100644 good.txt
```

直接输入，不调起vim

```
 $ git commit -m "my secend commint modify good.txt" good.txt
```

### 2.4版本穿梭

####2.4.1查看代码提交记录

```
$ git log
```
​	多行显示操作：空格向下翻页；b向上翻页；q退出

```
commit 132b8cae824c45cd8167b20fdab1148d72d759d7 (HEAD -> master) #哈希值 （指针指向当前版本）
Author: wxb <wxb@gmail.com> #
Date:   Thu Feb 27 10:53:45 2020 +0800

    my secend commint modify good.txt

commit f93b99e7573e8541e50669581092b14a388a2201
Author: wxb <wxb@gmail.com>
Date:   Thu Feb 27 10:44:37 2020 +0800

    my first feil
```

- 显示单行

```
$ git log --pretty=oneline 
```

```
1c3281b24ef1bc4b0b0847ab98e0ab6c28e1831c (HEAD -> master) dada da
4518293b01b39749a34e91e803cd39f28f6e9fa7 da
228ce4a5a7d99e91245d403cf3acfca3aa6954f7 yyyasd
```

- 显示单行 更简洁

```
$ git log --oneline
```

```
1c3281b (HEAD -> master) dada da
4518293 da
228ce4a yyyasd
```

- 显示移动到当前版本的步数

```
$ git reflog
```

```
1c3281b (HEAD -> master) HEAD@{0}: commit: dada da
4518293 HEAD@{1}: commit: da
228ce4a HEAD@{2}: commit: yyyasd
```

####2.4.2前进后退

- 本质

  移动HEAD指针

- 基于索引值操作（推荐）

```
$ git reset --hard 132b8ca
```

- 使用^符号：只能后退

```
$ git reset --hard HEAD^
```

一个^后退一步，n个后退n步

- 使用~符号：

```
$ git reset --hard HEAD~3
```

####2.4.3reset命令的三个参数

- -soft参数

  仅在本地库移动HEAD指针

- -mixed参数

  在本地库移动HEAD指针

  重置暂存区

- -hard

  在本地库重置HEAD指针

  重置暂存区

  重置工作区

#### 2.4.4如何找回删除后的文件

- 前提：删除前，文件存在时的状态提交到了本地库。
- 操作：git reset --hard[指针位置]
  - 删除操作已经提交到本地库：指针位置指向历史记录
  - 删除操作尚未提交到本地库：指针位置用HEAD

#### 2.4.5比较文件

```
$ git diff [文件名] #比较工作区与暂存区进行
$ git diff HEAD [文件名] #比较工作区与本地库
$ git diff -cached [文件名] #比较暂存区与本地库
```

不选择文件名时，展示全部文件

### 2.5分支管理

#### 2.5.1为什么有分支

版本控制过程中，使用多条线同时推进多条任务

![](image\分支.png)

#### 2.5.2分支好处

- 同时进行多个功能开发，提高效率
- 各个分支在开发过程中，如果某一个分支失败，不会有任何影响，失败的分支删除即可

#### 2.5.3分支操作

- 创建分支

```
$ git branch [分支名]
```

- 查看分支

```
$ git branch -v
```

- 切换分支

```
$ git checkout [分支名]
```

- 合并分支
  1. 切换到接受修改的分支
  2. 执行merge合并命令

```
$ git check out [分支名]
$ git merage [分支名]
```

- 解决冲突

  - 冲突的表现

  ![](image\分支冲突.png)

  - 冲突的解决
    1. 编辑文件，删除特殊符号
    2. 把文件修改到满意的程度，保存退出
    3. git add 文件名
    4. git commoit -m "日志信息" ，不能带文件名

##3.git基本原理

###3.1Hash（哈希）算法

![](image\哈希算法.png)

###3.2git保存版本的机制

####3.2.1集中式版本控制

![](image\集中式版本控制原理.png)

####3.2.2git的版本控制

![](image\git版本控制原理.png)

## 4.远程库

### 4.1推送

将本地库上传到远程库

```
$ git push [仓库地址] [分支]
```

###4.2为仓库地址设置别名

- 查看已有的别名

```
$ git remote -v
```

- 新增别名

```
$ git remote add [别名] [地址]
```

###4.3克隆

- 克隆把完整的远程仓库下载到本地
- 创建origin远程地址别名
- 初始化本地库

### 4.4拉取

- pull=fetch+merge

```
$ git pull [远程库地址的别名] [分支]
$ git fetch [远程库地址的别名] [分支]
$ git merge [远程库地址的别名]/[分支]
```

### 4.5解决冲突

- 要点
  - 如果不是最新仓库做的修改，必须先拉取。
  - 拉取下来如果进入冲突，按照“分支冲突”操作解决。

### 4.6跨团队协作

- fork
- 本地修改，推送到远程
- pull requests

### 4.7ssh免密

```
$ ssh-kegen -t rsa -C "wangxubo@seer-robotics.com"
```

## 5.其他

add(stage)  将代码添加到stage暂存区

 rm 指定删除文件

 stash 能够将所有未提交的修改（工作区和暂存区）保存至堆栈中，用于后续恢复当前工作目录。 

git stash list 查看当前内容

git stash pop 将堆栈清空并应用到当前分支

git stash apply 将堆栈保留并应用到当前分支

 commit 将暂存区的内容提交到本地库

 fetch/pull pull=lfetchl+merge

 push 推送到远程库 

log 版本记录

diff 与暂存区文件比较  diff HEAD与本地库进行比较

merge/rebase merge直接将最新的commit合并两个分支；rebase现将之前的所有提交整理成一个一个commit，再合并两个分支，保证了master或者说最终合并的分支的线性结构。 

branch 创建分支

checkout 切换分支&以最新的存储时间节点为参照，覆盖工作区对应文件file 

 reset  版本切换

