### 1 git的创始人

Linus Torvalds



### 2  Git与Svn对比

#### 2.1  Svn

svn是一个版本控制器，缺点：服务器单点故障， 容错性差

#### 2.2 Git 

Git是分布式版本控制器，版本控制，分布式

Git的一般工作流程如下：

1. 从远程仓库cloneGit资源作为本地仓库
2. 从本地仓库中checkout代码然后进行代码修改
3. 在提交前先将代码提交到暂寸区
4. 提交修改，提交到本地仓库，本地仓库修改各个历史版本
5. 在修改完成后，需要和团队成员共享代码时，可以将代码Push到远程仓库



### 3 创建一个版本库

1. git可以管理某一个目录下的代码， 使用git init 创建.git文件



### 4  版本创建与回退

#### 4.1 使用

1. 在目录下创建文件code.txt

2. 使用两条命令创建一个版本，在之前.git/config中配置用户邮箱和用户名，如下

   ```shell
   [user]
           email = "475770828@qq.com"
           name = "wangxing"
   [core]
           repositoryformatversion = 0
           filemode = false
           bare = false
           logallrefupdates = true
   ```

   ```shell
   git add code.txt
   git commit -m "版本号1“
   ```

3. 查看版本记录

   ```shell
   git log
   git log --pretty=oneline  #简短形式显示版本信息
   git log --pretty=oneline --graph #显示合并分支的图
   ```

4. 版本的回退

   ```shell
   git reset --hard HEAD^ #回退到上一个版本,返回前两个为^^, HEAD~1表示当前版本，HEAD~N表示当前版本的前N个版本
   git reset --hard 版本序列号(208b14b)
   git reflog #查看操作记录
   ```

#### 4.2 工作区和暂存区

##### 4.2.1 工作区（Working Directory）

电脑中的目录，比如git test，就是一个工作区

##### 4.2.2 版本库

.git隐藏目录就是版本库

git add 就是把文件添加进去，实际上就是把文件修改添加到暂存区， git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

使用命令查看当前工作区的状态

```shell
	git status
```

git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支，创建版本记录;

#### 4.3 管理修改

git 管理的文件的修改，它只会提交暂存区的修改来创建版本。

#### 4.4 撤销修改 

1. 使用以下命令，丢弃工作区的改动;

```
git checkout -- <文件名> 
```

2. 乱改了工作区的文件，并且加入暂存区，想要丢弃修改，分两步，将暂存区的修改撤销掉，重新放回工作区;

```
git reset HEAD file
git checkout -- <文件名> 
```

3. 已经提交了不合适的修改到版本库时，想要修改本次提交，参考上节回退一节；

#### 4.5 对比文件的不同

（1）对比工作区和某个版本中文件的不同，命令为：

```
git diff HEAD -- 文件名
```

```bash
 -- code.txt
diff --git a/code.txt b/code.txt
index 775856b..1e02225 100644
--- a/code.txt #减号为对比版本的文件
+++ b/code.txt #加号为工作区的文件
@@ -3,3 +3,4 @@ the is the second line
 the is the third line
 the is the four line
 the is the five line
+the new line  #加号表示工作区的code.txt比对比版本多了一行
```

（2）对比两个版本间文件的不同

例子为对比HEAD和HEAD^版本中code.txt的不同，命令为：

```bash
git diff HEAD HEAD^ -- code.txt
```

```bash
diff --git a/code.txt b/code.txt
index 775856b..43a2db2 100644
--- a/code.txt  #减号对应于HEAD版本中的文件
+++ b/code.txt  #加号对应与HEAD版本中的文件
@@ -2,4 +2,3 @@
 the is the second line
 the is the third line
 the is the four line
-the is the five line #减号对应于HEAD版本中的文件独有的内容
```

#### 4.6 删除文件

（1）将目录中的code2.txt 删除，是对工作区的改动

```bash
rm 文件名
```

如果想要恢复该文件，只需要将工作区的改动添加到暂存区，使用以下命令：

```bash
git add 文件名 或者  git rm 文件名
git checkout -- 文件名
```



### 5 分支管理

#### 5.1 概念

分支可以多个人同时在各自的分支上工作，不影响别人的工作，做完后在进行合并;

#### 5.2 创建与合并分支

git创建了一个指针，HEAD指向是先经过分支（master/dev），然后指向相应的版本。创建分支对于工作区没有影响; git合并完分支到master后，可以删除，删除掉后就只剩下master主分支;

（1）查看当前分支，前面有*号的为当前分支

```bash
git branch
```

（2）创建分支，并切换到所创建的分支

```bash
git checkout -b dev
```

（3）修改文件，并且提交;

```bash
git commit -m "提交信息"
```

（4）切换分支

```bash
git checkout 分支名
```

(5) 合并分支到当前分支

```bash
git merge 合并分支 #快速合并(fast-forward)
```

（6）删除分支

```bash
git branch -d 分支名
```

#### 5.3 解决冲突

（1）创建一个新分支 dev

（2）冲突产生的时间：在两个分支上面都有了新的提交，并且编辑的是同一文件，就会起冲突。git 告诉我们，code.txt文件存在冲突，**必须手动解决**冲突后再提交。

（3）然后删除分支

```bash
git branch -d 分支名
```

#### 5.4 分支管理策略

1. **通常，合并分支时，如果可能，git会用fast forward模式，但是有些快速合并不能成功，而且合并时没有冲突，这个时候合并之后再做一次提交。这种模式下，删除分支，会丢掉分支信息。**

（1）创建并切换到dev分支上

（2）在dev分支上，新建一个文件code3.txt，并做一次commit

（3）切换到master分支，编辑code.txt，并进行一次commit

（4）合并dev分支的内容到master

2. 如果强制禁用fast forward 模式，git就会在merge时生成一个新commit，这样，从分支历史上就可以看出分支信息。

   准备合并dev分支，禁用fast foreard请注意在提交时，使用如下命令：

   ```bash
   git merge --no-ff -m "禁用fast-forward" 分支名
   ```

#### 5.5 Bug 分支

软件开发中，bug很常出现，有了bug就需要修复，在git中，由于分支比较强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

git提供了一个stash功能，可以把当前工作现场“储存”起来，等以后恢复现场后继续工作。

```bash
git stash
```

然后切换到bug分支，并增加新的修复分支（bug-001），在bug-001分支上进行修改，并且提交commit，然后切换到bug分支，并且合并，使用如下命令：

```bash
git merge --no-ff -m "修复bug-001" bug-001
```

然后删除修复分支（bug-001）

```bash
git branch -d bug-001
```

修复完成后，切换到dev分支工作，恢复工作区现场使用如下命令：

```bash
git checkout dev
git stash pop
```

**小结**：修复bug时，我们通过创建新的bug分支进行修复，然乎合并，最后删除;

当手头工作没有完成时，先把工作区现场 **git stash** 一下，然后去修复bug，修复后，再  **git stash pop**，回到工作区。



### 6 github

#### 6.1 创建仓库

在创建仓库的时候，可以添加需要忽略的文件，.gitignore文件。

#### 6.2 添加ssh账户

（1）点击账户头像后下拉三角，选择“setting”

如果某台机器需要与github上仓库交互，那么就要把这台机器的ssh公钥添加到这个github账户上。

<img src="/home/wx/图片/2021-11-27 13-49-45屏幕截图.png" alt="2021-11-27 13-49-45屏幕截图" style="zoom:50%;" />

在这里添加ssh key的密钥，

（2）获取密钥在主目录下的隐藏文件中设置，使用如下命令：

```bash
cd
vim .gitconfig #设置用户的密码和邮箱
```

![2021-11-27 13-51-27屏幕截图](/home/wx/图片/2021-11-27 13-51-27屏幕截图.png)

（3）使用如下命令生成ssh密钥;

```bash
ssh-keygen -t rsa -C "注册邮箱"
```

![x](/home/wx/图片/2021-11-27 13-58-16屏幕截图.png)

(4)获取公钥

![2021-11-27 13-59-58屏幕截图](/home/wx/图片/2021-11-27 13-59-58屏幕截图.png)

#### 6.3 克隆项目

```bash
git@github.com:MrWXfaster/Notes-Learning.git
```

#### 6.4 上传分支

（1）创建自己的分支，并在上面开发

（2）推送分支，就是把该分支上所有本地提交推送到远程库，推送时要指定本地分支，这样，git就会把该分支推送到远程库对应的远程分支上。

```bash
git push origin 分支名称 #origin表示远程的意思
git push origin wangxing#(分支名)
```



#### 6.5 将本地的分支跟踪服务器分支

```bash
git branch --set-upstream-to=origin/远程分支名称 本地分支名称
#例如：
git branch --set-upstream-to=origin/wangxing wangxing
```

#### 6.6 从远程分支拉取分支

```bash
git pull origin wangxing
```

