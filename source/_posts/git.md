---
title: git常用指令总结(By wiky)
tags: [git]
---

cd  dir    --------跳转目录
git init   -------初始化本地仓库目录；
git add filename  ------添加文件
注：如果修改的内容没有add到暂存区，则就不会加入commit里面
git commit -m "operation declaration"  -----提交文件，可从commit恢复,
git status --------查看那些文件被修改（工作区master）
git diff    ------文件做了哪些修改
git log     ------显示最近到最远的提交日志
git log -pretty=oneline ------显示修改版本号和操作说明
git reset --hard HEAD^  ------回退一个版本,commit或者reset才回产生版本号
git reset --hard 版本号前几位   -----回到某个版本（实际上是在移动HEAD指针）
git reflog  -----查看命令历史
cat filename -----显示文本信息
<!--more-->

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


git checkout -- file  -----丢弃工作区的修改
git reset HEAD file   ---丢弃暂存区的修改


git remote add origin git@github.com:michaelliao/learngit.git（改成自己的git地址）----本地仓库推送到gitHub
git push    ------本地库推送到远程
git push origin master  -----本地的最新修改同步到gitHub；

注：自己的github地址：git@github.com:wikyMr/learngit.git

git clone git的ssh地址 ------ 远程仓库拷贝到本地仓库；
git checkout -b dev    ------创建新的分支为dev；相当于下面两个命令
git branch dev
git  checkout dev

git branch -------查看所有分支，当前分支用*标记
git checkout master  -----切换到master分支
git merge --no-ff -m "merge with no-ff" dev 
git merge dev    ------将dev分支合并到master分支；（考虑上面的方式）

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：
优先考虑：
git merge --no-ff -m "merge with no-ff" dev
次要：
git merge <name>

删除分支：git branch -d <name>默认删除会丢失分支信息；

解决冲突：

先git checkout master 到主分支，然后修改内容，add--commit完成；
git log --graph --pretty=oneline --abbrev-commit   ------分支合并情况；


--no-ff方式git merge:
git merge --no-ff -m "merge with no-ff" dev


git branch -D <name>强行删除未合并的分支
git stash  保存现场
git stash apply  恢复现场，现场不删除
git stash drop   删除现场
git stash pop 恢复现场并删除


git remote 查看远程库信息
git remote -v  查看远程库详细信息
git push origin branch-name   ---推送本地分支
git checkout -b branch-name origin/branch-name ----本地创建于远程对应的分支
git pull   ---从远程抓取分支；
git branch --set-upstream branch-name origin/branch-name  ----本地建立与远程对应的分支；
