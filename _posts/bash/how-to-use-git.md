---
title: 如何使用GIT
date: 2017-01-26 23:45:10
tags: git
category: bash
---
鉴于自己删词忘记 git 的用法，特意总结一下 GIT 的入门用法（我这智商只用得到这么多）

有关资料 [runoob 的 GIT 教程](http://www.runoob.com/git/git-tutorial.html)  [GIT 简明教程](http://www.runoob.com/manual/git-guide/)  [GIT5 分钟教程](http://www.runoob.com/w3cnote/git-five-minutes-tutorial.html)  还有 [CSDN 的 SSH KEY 生成](http://blog.csdn.net/hustpzb/article/details/8230454/)  总之感谢万分

相信你已经装好了 GIT ( [我就没有](http://www.runoob.com/manual/git-guide/#2) )

在你想要 GIT 的目录右击并 git bash here
<!-- more -->
## 生成你的本地密钥

（以前有的话直接在 C:Users 用户名.ssh 中找）

`ssh-keygen -t rsa -C “xxx@gmail.com”`

直接三次回车，默认文件名（id_rsa 和 id_rsa.pub），默认不设置密码，不需要重复密码（对自己电脑的加密有信心的话）

## 添加密钥到 ssh

`ssh-add id_rsa`（没有pub后缀的这个私钥）

右击用 notepad 等高级编辑器打开 id_rsa.pub 就是给网站的公钥，不要用 Publisher（office）打开都行

复制里面的内容给你需要用到的地方就好了 ，那个网站就认得你了

不论有没有仓库都可以先

`git clone it clone https://github.com/xxx/xxx.git`

再切换到那个目录`cd xxx`

或者本地干`git init`

## 初始化本地目录

然后愉快的添加文件来同步了`git status`

查看同步状态（添加了的都会是红色）

使用`git add *` //一次性添加全部

`git add filename `//添加一个叫filename的文件

添加一个描述信息（要各个文件不同的话请在 git add 之后输入以改变新 add 的文件的描述）（add *之后是改变全部）

`git commit -m "new files"` //new files改成你要的

这样就可以推送了`git push origin master`  //推送到master

等待缓慢的上传结束

## more

```
git branch test //使用branch命令创建一个新的分支test
git checkout test  //进入分支test
git checkout master  //滚回主分支
git checkout -b test  //创建一个叫做“test”的分支，并切换过去
git branch -d test  //删除test分支
git push origin test  //推送test分支到服务端
git merge test   //合并分支
git rm file  //删除文件file
git pull  //更新你的本地仓库至最新改动
```

## anything else

```
Git常用命令
查看、添加、提交、删除、找回，重置修改文件
 
git help <command> # 显示command的help
 
git show # 显示某次提交的内容 git show $id
 
git co -- <file> # 抛弃工作区修改
 
git co . # 抛弃工作区修改
 
git add <file> # 将工作文件修改提交到本地暂存区
 
git add . # 将所有修改过的工作文件提交暂存区
 
git rm <file> # 从版本库中删除文件
 
git rm <file> --cached # 从版本库中删除文件，但不删除文件
 
git reset <file> # 从暂存区恢复到工作文件
 
git reset -- . # 从暂存区恢复到工作文件
 
git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
 
git ci <file> git ci . git ci -a # 将git add, git rm和git ci等操作都合并在一起做　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　git ci -am "some comments"
 
git ci --amend # 修改最后一次提交记录
 
git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象
 
git revert HEAD # 恢复最后一次提交的状态
 
查看文件diff
 
git diff <file> # 比较当前文件和暂存区文件差异 git diff
 
git diff <id1><id1><id2> # 比较两次提交之间的差异
 
git diff <branch1>..<branch2> # 在两个分支之间比较
 
git diff --staged # 比较暂存区和版本库差异
 
git diff --cached # 比较暂存区和版本库差异
 
git diff --stat # 仅仅比较统计信息
 
查看提交记录
 
git log git log <file> # 查看该文件每次提交记录
 
git log -p <file> # 查看每次详细修改内容的diff
 
git log -p -2 # 查看最近两次详细修改内容的diff
 
git log --stat #查看提交统计信息
 
tig
 
Mac上可以使用tig代替diff和log，brew install tig
 
Git 本地分支管理
 
查看、切换、创建和删除分支
 
git br -r # 查看远程分支
 
git br <new_branch> # 创建新的分支
 
git br -v # 查看各个分支最后提交信息
 
git br --merged # 查看已经被合并到当前分支的分支
 
git br --no-merged # 查看尚未被合并到当前分支的分支
 
git co <branch> # 切换到某个分支
 
git co -b <new_branch> # 创建新的分支，并且切换过去
 
git co -b <new_branch> <branch> # 基于branch创建新的new_branch
 
git co $id # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
 
git co $id -b <new_branch> # 把某次历史提交记录checkout出来，创建成一个分支
 
git br -d <branch> # 删除某个分支
 
git br -D <branch> # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)
 
 分支合并和rebase
 
git merge <branch> # 将branch分支合并到当前分支
 
git merge origin/master --no-ff # 不要Fast-Foward合并，这样可以生成merge提交
 
git rebase master <branch> # 将master rebase到branch，相当于： git co <branch> && git rebase master && git co master && git merge <branch>
 
 Git补丁管理(方便在多台机器上开发同步时用)
 
git diff > ../sync.patch # 生成补丁
 
git apply ../sync.patch # 打补丁
 
git apply --check ../sync.patch #测试补丁能否成功
 
 Git暂存管理
 
git stash # 暂存
 
git stash list # 列所有stash
 
git stash apply # 恢复暂存的内容
 
git stash drop # 删除暂存区
 
Git远程分支管理
 
git pull # 抓取远程仓库所有分支更新并合并到本地
 
git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
 
git fetch origin # 抓取远程仓库更新
 
git merge origin/master # 将远程主分支合并到本地当前分支
 
git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支
 
git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上
 
git push # push所有分支
 
git push origin master # 将本地主分支推到远程主分支
 
git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
 
git push origin <local_branch> # 创建远程分支， origin是远程仓库名
 
git push origin <local_branch>:<remote_branch> # 创建远程分支
 
git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
 
Git远程仓库管理
 
GitHub
 
git remote -v # 查看远程服务器地址和仓库名称
 
git remote show origin # 查看远程服务器仓库状态
 
git remote add origin git@ github:robbin/robbin_site.git # 添加远程仓库地址
 
git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址) git remote rm <repository> # 删除远程仓库
 
创建远程仓库
 
git clone --bare robbin_site robbin_site.git # 用带版本的项目创建纯版本仓库
 
scp -r my_project.git git@ git.csdn.net:~ # 将纯仓库上传到服务器上
 
mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库
 
git remote add origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址
 
git push -u origin master # 客户端首次提交
 
git push -u origin develop # 首次将本地develop分支提交到远程develop分支，并且track
 
git remote set-head origin master # 设置远程仓库的HEAD指向master分支
 
也可以命令设置跟踪远程库和本地库
 
git branch --set-upstream master origin/master
 
git branch --set-upstream develop origin/develop

```

## 命令集合

![](https://img.totoro.pub/images/2017/06/24/TAEQ.png)

