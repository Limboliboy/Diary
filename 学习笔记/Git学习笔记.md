# 廖雪峰Git教程学习笔记

### Git是最先进的分布式版本控制系统

Git 分为工作区、暂存区和版本库，git add 把工作区的文件提交到暂存区，git commit 把当前暂存区中所有的文件提交到版本库。

![image-20200418083924785](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200418083924785.png)

版本库中有一个 HEAD 指针自动指向最新的版本，如下：

![image-20200418083610113](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200418083610113.png)

#### Git 优势:

1、分布式：本地工作不需要考虑远程库，没有联网可以照常工作，有网时本地提交推送到远程就能同步

2、快：git 创建、切换和删除分支速度飞快

---

### Git 常用命令：

git update-git-for-windows ：把windows上的git更新到最新版本

git init ：初始化版本库

git add <file> ：把指定工作区文件<file>加入暂存区

git commit -m "message" ：把<font color=red>所有暂存区文件</font>提交到版本库，message表明哪里被改动

git status ：查看仓库状态，分为3种：上次提交后工作区是否有文件被修改，或者工作区文件是否未加入暂存区，或者暂存区是否有文件待提交

git diff <file> ： 查看指定文件<file>最近两次的修改情况，通过diff方法，比较最后一次和上一次的提交

git diff HEAD -- <file> ：比较工作区和版本库中最新版本的区别

git log 查看 git 日志，按提交时间由近至远显示提交日志

git log --pretty=online ：精简版 git 日志，仅提供版本号+修改信息message

git reflog ：显示每一次git命令，可用于查看版本号

git reset --hard HEAD^ ：退回到最近一次提交的版本。HEAD指向最新版本，HEAD^指向上一个版本，HEAD^^指向上两个版本，HEAD~100指向上100个版本

git reset --hard 1094a ：退回到指定版本，1094a是版本号的前几位

git reset HEAD <file> ：取消文件的暂存，即把放入暂存区的文件恢复至放入之前的状态

git checkout -- <file> ：撤销对工作区文件的修改，-- 代表当前分支；情况1：文件修改后还没有使用 git add <file> 加入暂存区，使用 checkout 撤销修改到和最新版本库一样的状态；情况2：文件加入暂存区后又做了修改，使用checkout 撤销修改回到加入暂存区后的状态；情况3：提交了不合适的修改到版本库中，使用 git reset --hard HEAD^ 回退到之前的版本 

rm <file> ：删除工作区的文件，情况1：确实要从版本库中删除文件，搭配 git rm <file> 使用，并且 git commit -m "delete file" 提交删除日志；情况2：删错了，可以使用 git checkout -- <file> 从版本库中恢复文件

---

### GitHub 远程仓库

GitHub 免费提供仓库托管服务，本地Git仓库和GitHub仓库之间的传输时通过SSH加密的。

1、检查是否存在SSH key，打开 git ，cd ~/.ssh ，如果提示没有该文件夹，或文件夹下没有 id_rsa 和 id_rsa.pub 这两个文件，则在开始界面打开 git 创建 SSH key：

```
$ ssh-keygen -t rsa -C "youremail@example.com" // 一路回车使用默认值即可，无需设置密码
```

你会发现出现了 id_rsa 和 id_rsa.pub 这两个文件，分别是私钥和公钥，私钥不能泄露出去，公钥可以放心告诉别人。如果已存在这两个文件，直接进入第 2 步

2、登录 GitHub 打开设置，选择 SSH and GPG keys >> SSH keys >> New SSH keys >> title 加入提示信息，如 SSH keys 主人名称；Key 赋值粘贴 id_rsa.pub 的内容（记事本打开即可）

SSH keys作用是让 GitHub 识别推送的提交确实是你推送的，不是他人冒充的；GitHub 允许添加多个 Key ，只要把每台电脑的 Key 加入到 GitHub，就可以在家或者学校往 GitHub 推送了。

---
#### 先有本地库，添加远程仓库

你在本地创建了一个 Git 仓库后，需要在 GitHub 创建一个 Git 仓库并让二者远程同步，GitHub仓库可作为备份，也方便其它人写作。

本地创建仓库：

1、在合适的位置创建版本库

```
$ mkdir reponame // 创建版本库
$ cd reponame 
$ git init // 版本库初始化
$ touch readme.txt // 新建 readme
$ vim readme.txt // 编辑 readme
  ……
$ git add readme.txt // 把工作区的 readme 存入暂存区
$ git commit -m "write a readme" // 把暂存区的 readme 提交到版本库
```

2、登录 GitHub 》New 》输入 reponame 和描述信息，Create repositories

3、在本地 repo 仓库运行 github 提示的命令，将本地仓库与远程仓库关联，例如：

```
$ git remote add origin *** // *** 代表 HTTP 或 SSH 协议地址
```

4、把本地库的所有内容推送到远程仓库上：

``` 
$ git push -u origin master // git push 将当前分支 master 推送到远程
```

以后推送或拉取 master 分支可以简化命令

``` 
$ git push origin master // 提交本地 master 分支到远程上
```

---

#### 先有远程库，从远程库克隆到本地

远程创建库：

1、登录 GitHub 》New 》输入 reponame 和描述信息》Initialize with a README 初始化新建README 》Create repositories

2、Clone or download 》Clone with SSH 》复制 SSH 地址

3、本地选择合适位置，克隆远程仓库

``` 
$ git clone < SSH address > // 克隆远程库
```

Git 支持多种协议，包括 HTTP、SSH 等，SSH 比 HTTP 快！

---

### 分支管理

#### 常用命令

```
$ git checkout -b dev // 创建并切换至 dev 分支
$ git branch // 查看分支状态，* 标注当前分支
$ git checkout master // 切换至 master 分支
$ git merge dev // 合并分支
$ git branch -d dev // 删除 dev 分支
/*
  git checkout -b dev 
  等价于
  git branch dev // 创建 dev 分支
  git checkout dev // 切换至 dev 分支
  等价于
  git branch dev
  git switch dev // 切换至 dev 分支
  等价于
  git switch -c dev // 创建并切换至 dev 分支
*/
```

---

#### 解决合并冲突

``` 
$ git switch -c feature1 // 新建并切换至 feature1 分支
$ vim readme.txt
  …… 修改readme.txt末行为Creating a new branch is quick AND simple
$ git add readme.txt
$ git commit -m "AND simple" // feature1 分支 readme 为 AND simple
$ git switch master // 切换至 master 分支
$ vim readme.txt
  …… 修改readme.txt末行为Creating a new branch is quick & simple
$ git add readme.txt
$ git commit -m "& simple" // master 分支 readme 为 & simple
$ git merge feature1 // 执行合并 feature1 分支操作
  …… 报错，分支合并存在冲突！
$ vim readme.txt
  …… 修改readme.txt末行为Creating a new branch is quick and simple
  …… 记得删除多余的冲突痕迹
$ git merge feature1 // 大小写“冲突”将自动合并，向另一个分支看准，即最终 master 分支 readme.txt 内容为 AND simple
$ git branch -d feature1 // 删除 feature1 分支
$ git push origin master // 推送到远程库
$ git log --graph --pretty=online --abbrev-commit // 按图方式查看提交日志 
```

![image-20200420160149591](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200420160149591.png)

![image-20200420134623117](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200420134623117.png)

---

#### --no-ff 方式 git merge

Fast forward 模式在合并分支时会丢失分支信息，如下所示：

![image-20200421194546625](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421194546625.png)

![image-20200421193535878](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421193535878.png)

删除分支后看不出曾做了合并，且版本回退会退回到原dev分支的最后一次提交

--no-ff 禁用 fast forward 模式合并分支，删除分支后也会保留分支信息；版本回退将退回到master分支的上一次提交；--no-ff 合并时产生一次新的提交：

``` 
$ git merge --no-ff -m "merge with no-ff" dev
```

![image-20200421200530252](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421200530252.png)

![image-20200421200828437](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421200828437.png)

![image-20200421202321472](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421202321472.png)

![image-20200421202519597](C:\Users\12657\AppData\Roaming\Typora\typora-user-images\image-20200421202519597.png)

---

#### 三路合并

Git 提供的合并冲突解决方案是<font color=red>“不自作聪明”</font>，它不会尝试自动解决所有问题。

| 共同祖先（Base） | 当前分支（ours） | 待合并分支（theirs） | 合并结果 | 说明                                           |
| ---------------- | ---------------- | -------------------- | -------- | ---------------------------------------------- |
| A                | A                | A                    | A        | 没有改动保持不变                               |
| A                | A                | B                    | B        | 一方修改，选择修改版                           |
| A                | B                | A                    | B        | 一方修改，选择修改版                           |
| A                | B                | B                    | B        | 双方修改相同选择修改版                         |
| A                | B                | C                    | 冲突     | 双方修改不一致合并失败，报告冲突，需要用户解决 |

---

#### Bug 分支

软件开发中经常会出现bug，在Git中每个Bug都可以通过新建临时分支来修复，修复后合并分支再把临时分支删除。当你在dev分支进行开发工作时，收到修复bug-101的命令，但是dev上的工作进行到一半，更改还没有提交，可以使用 <font color=red> git stash </font> 把工作现场储存起来。切回到master，新建issue-101分支修复bug，修复完之后回到dev分支恢复工作环境。

```
git stash list // 查看所有保存的工作现场
git stash apply // 恢复工作现场，保留 stash
git stash drop // 删除 stash，上面两行等价于 git stash pop
```

---

#### Git cherry-pick

如果想在dev上修复同样的bug，重复操作显得比较麻烦，或者使用 git cherry-pick <commit-id>

``` 
|
|		C3（*dev）
|		 |
|		C2
|		/
|	  /
master
```

如上，如果想丢弃 C3 的修改，应用  C2 到 master 上：

- 在 dev 分支上，使用 git log --oneline 查看 C2 commit 的 id（ISH码） // 或使用 git log dev --oneline
- git switch master 切换至主分支
- git cherry-pick <C2_id>，无冲突则如下图

``` 
C4
| \
|   \	C3（*dev）
|	  \	 |
|		C2
|	   /
|	 /
C1(master)
```

C4 = C1 + ( C2 - C1 )，git cherry-pick 同样是三路合并策略

cherry-pick 与 merge 相比，不是把整个分支的所有修改都应用在 master 上，而是把某一次提交所做的修改应用在 master 上

``` 
git cherry-pick [<options>] <commit-ish> ……
常用 options :
	--quit 结束cherry-pick序列,不影响多个提交中已经成功的
	--abort 取消cherry-pick序列，包括多个提交中已经成功的，恢复当前分支
    --continue 继续当前cherry-pick序列，或者下一个cherry-pick
git cherry-pick 发生冲突后需要先解决冲突，后git add.标记解决冲突，最后git cherry-pick --continue
常见 error ：
The previous cherry-pick is now empty, possibly due to conflict resolution.
  上次cherry-pick冲突解决后，导致cherry-pick内容为空，不需要进行cherry-pick，解决办法：
  1.git cherry-pick --abort 取消上次操作
  2.git cherry-pick --allow-empty 允许空提交
```

选取多个修改，用空格分开 commit-sha

```
git cherry-pick commit-1 commit-2
```

选取一个范围内的多次修改，不包含左边界，左开右闭

``` 
git cherry-pick <start-commit>..<end-commit>
```

选取一个范围内的多次修改，包含左边界，左右都闭

``` 
git cherry-pick <start-commit>^..<end-commit>
```

---

#### Feature分支

开发一个新feature、新功能，最好新建一个分支；丢弃一个还没有合并的分支，可以使用 git branch <font color=red>-D</font> <branchname> 来强行删除

---

#### 多人协作

``` 
$ git remote -v // 显示抓取和推送origin的地址
$ git push origin master // 把本地master分支推送到远程库对应的远程master分支上
$ git push origin dev // 把本地master分支推送到远程库对应的远程dev分支上
```

抓取分支（克隆远程仓库）

``` 
$ git clone <ssh_address>
```

当你从远程克隆仓库时，默认只能看到master分支，如果有其它分支，必须先创建其它分支对应的本地分支

``` 
$ git branch
  * master // 只有master分支
$ git switch -c dev origin/dev // 创建本地dev分支
  // 此时dev分支还没有和远程dev分支关联，git pull会失败；默认只会关联远程和本地的master分支
$ git pull // 失败
$ git branch --set-upstream-to=origin/dev dev // 把远程仓库的dev与本地dev分支关联起来
$ git pull // 成功
```

**多人协作工作模式**

1. 首先尝试用 git push origin <branch-name> 推送自己在某一分支上的修改；
2. 如果推送失败，是因为远程分支比本地更新，先使用 git pull 试图合并远程与本地的修改；
3. 如果 git pull 有冲突，解决本地冲突，在本地提交；
4. 没有冲突或解决掉冲突后，再用 git push origin <branch-name> 推送修改，成功！

**Rebase**

1. 

---

### 标签管理

tag 是一个让人易于记住的有意义的名字，一个不可移动的、指向 commit 的指针

#### 创建标签

切换至需要打标签的分支上，默认给最新的 commit 打上标签

``` 
$ git tag v1.0 // git tag <version_id>
$ git tag -a v0.1 -m "version 0.1 released" 1094adb // -a 指定标签名，-m 指定说明文字 
```

给历史提交的 commit 打标签

``` 
$ git tag v0.9 f52c633 // git tag <version_id> <commid_sha> 
```

查看所有标签

``` 
$ git tag // 按字母排序列出 tag 标签
  v0.9 
  v1.0
$ git show v0.9 // git show <tag_name> 查看标签信息
```

#### 操作标签

删除标签

``` 
本地删除（未推送到远程）
  $ git tag -d v0.1 // git tag -d <tag_name> 本地删除（未推送到远程）
推送到远程，删除标签
  $ git tag -d v0.9 // 先删除本地标签
  $ git push origin :refs/tags/v0.9 // 再使用 push 删除远程标签，注意格式 
```

推送标签

``` 
$ git push origin <tag_name> // 创建的 tag 标签都存储在本地，不会自动推送到远程
$ git push origin --tags // 一次性推送全部未推送到远程的本地标签
```



---

### <font color=red>注意！</font>

1、git add 提交多个文件

```
1、git add file1 file2 file3 // 空格间隔开
2、git add file1
  git add file2
  git add file3 // 多次提交
```

2、每次修改文件后记得 git add <file>，把它先提交到暂存区，最后 git commit -m "message" 一次性提交全部已修改的文件

3、如果文件已经被提交到版本库，则不用担心误删，但是你只能恢复文件到最新版本，如果你在误删文件后还有其它修改，版本回退会丢失最后一次提交之后的修改

