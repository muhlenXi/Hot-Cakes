---
title: Git 使用小记
date: 2016-12-15 10:20:19
categories: 学习笔记
tags: [Git]
---

>  Git 是目前世界上最先进的分布式版本控制系统。

### 生成 .gitignore 文件

| 命令 | 简单说明 |
| :--------- | :---------- |
| echo "function gi() { curl -L -s https://www.gitignore.io/api/\$@ ;}" >> ~/.zshrc && source ~/.zshrc | 配置 gi 函数 |
| gi swift >>.gitignore |生成 Swift .gitignore 文件|


### 常用 Git 指令

Unix 的哲学是 『没有消息就是好消息』，没有消息则说明指令操作成功。

#### 1 - 文件操作

| 命令 | 简单说明 |
| :--------- | :---------- |
| cd 目录 | 进入到某一路径 |
| pwd | 查看当前路径 |
| mkdir 文件夹名 | 创建文件夹 |
| touch 文件名 | 创建文件 |
| cat 文件名 | 查看文件内容 |

####  2 - 仓库『repository』操作

| 命令 | 简单说明 |
| :--------- | :---------- |
| git init  | 初始化 git 仓库 |
| git add 文件名| 添加文件到仓库中 |
| git add . | 添加当前目录所有文件 |
| git status | 查看工作区的状态 |
| git commit -m "描述" | 提交记录 |
| git log | 查看历史提交记录 |
| git reset --hard HEAD^  | 回退到上一版本 |
| git reflog   | 查看历史 commit  id |
| git reset --hard commit_id  | 回退指定版本 |
| git remote add origin  仓库地址 | 关联远程仓库 |

####  3 - 分支『branch』操作

| 命令 | 简单说明 |
| :--------- | :---------- |
| git branch  |  查看分支 | 
| git branch -a   |  查看远端分支 | 
| git branch 分支名   |  创建分支 | 
| git checkout 分支名   |  切换分支 |  
| git checkout -b 分支名  |  创建并切换分支 | 
| git merge 分支名   |  合并某分支到当前分支 | 
| git branch -d 分支名  |  删除分支 | 
| git push origin master | 推送 commit 记录到远端 master 分支 |
| git push origin master --tags  | 推送 commit 记录和 tag 到远端 master 分支 |
| git push origin -d 远端分支名   |  删除远端分支 | 
| git clone -b 分支名  |  克隆指定分支 | 
| git rebase 分支名| 将当前分支的提交记录追加到指定分支后 |

#### 4 - 标签『tag』 操作

| 命令 | 简单说明 |
| :--------- | :---------- |
| git tag v1.0   |  给最新 commit 打一个标签，v1.0为标签名  |
| git tag 标签名 commit_id   |  给指定的commit打标签  |
| git tag  |  查看所有的标签   |
| git log --pretty=oneline --abbrev-commit   |  查看历史提交的commit id  |
| git show 标签名   |  查看标签信息  |
| git tag -a 标签名 -m “说明文字” commit_id  |  打带说明文字的标签  |
| git tag -s 标签名 -m “说明文字” commit_id   |  打用PGP签名标签  |
| git tag -d 标签名   |  删除本地标签  |
| git push origin --delete tag 标签名 | 删除远端 指定标签 | 
| git push origin 标签名  |  推送某个标签到远程仓库  |
| git push origin --tags  |  推送全部标签到远程仓库  |
| git push origin :refs/tags/标签名  |  删除远程标签，需先删除本地标签  |


#### 5 - 其他』 操作

| 命令 | 简单说明 |
| :--------- | :---------- |
| git remote prune origin   | |
| git branch --unset-upstream   | |
| git remote show origin   | |
| git push --set-upstream origin fxchat_xiyinjun   | |

	 

### Git 的配置

| 命令 | 简单说明 |
| :--------- | :---------- |
| git config --global user.name "John Doe"  | 设置用户名 |
| git config --global user.email | 设置邮箱 |
| git config --global core.editor emacs | 设置默认编辑器 |
| git config --list | 检查配置信息 |

*你可以通过输入 `git config <key>` 来检查 Git 的某一项配置。*

```shell
git config user.name
```

【2】配置缩写别名：

```shell
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.cm commit
git config --global alias.br branch
git config --global alias.last 'log -1'  //显示最近一次的提交
```
    
*`--global` 参数是全局配置参数，也就是这些命令在这台电脑的所有 Git 仓库下都生效。*

【2】配置文件的路径

- 每个仓库的Git配置文件都放在 `.git/config` 文件中，用命令 `cat .git/config ` 可以查看内容。

 *别名就在 [alias] 后面，要删除别名，直接把对应的行删掉即可。*　　

- 当前用户的 Git 配置文件放在 `用户主目录` 下的一个隐藏文件 `.gitconfig` 中，用命令  `cat .gitconfig ` 可以查看内容。
    
### 其他事项

1、如何 配置 SSH？

* 1- 本地 Git 仓库和 GitHub 仓库之间的传输是通过 ssh 加密传输的，通过以下命令生成 ssh key。 

```shell
ssh-keygen -t rsa -C "你注册的邮箱地址"   // 生成 ssh key
```
    
* 2 - 创建完成后，会在主目录中生成  `.ssh` 目录，里面有 `id_rsa（私钥）`和`id_rsa.pub（公钥）`。`cd` 到x `.ssh` 目录下，用命令 `cat id_rsa.pub` 可查看公钥内容。

* 3 - 登陆 GitHub，打开 Account setting , 在 SSH keys 界面 粘贴公钥里的内容就可以了。



