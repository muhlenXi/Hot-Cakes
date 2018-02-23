#### git 工作流

1、克隆远程分支

	git clone -b FXChat_Dev  分支地址 
	
2、创建并切换到新分支

	git checkout -b FxChat_xyj
	
3、添加文件、提交

	git add.
	
	git commit -m "描述语句"
	
4、更新远程分支

	git checkout FXChat_Dev
	
	git pull
	
5、rebase 分支

	git checkout  FxChat_xyj
	
	git rebase FXChat_Dev
	
产生冲突后：在冲突分支上，解决冲突后

	git add.
	
	git rebase --continue
	
6、推送分支到远端

	git push
	
### common git commands

*本地分支关联远程仓库*

	git remote add origin 远程仓库地址

*clone 远程仓库到本地*

	git clone 远程仓库地址
	
*添加更改到暂存区*

	git add .
	git add 文件名
	
*提交本地更改记录*

	git commit -m "描述备注"
	
*对最新一条 commit 进行修正*

	git commit --amend
	
*同步 commits 到远程 master 分支*

	git push origin master
	
*同步本地新建分支到远端仓库（远程仓库无该分支）*

	git push --set-upstream origin 分支名
	
*本地分支最新记录强制覆盖远程分支*

	git push origin master --force
	
*删除远程仓库分支*

	git push origin -d 分支名
	
*撤销添加到暂存区的内容*	

	git reset head

*查看仓库的提交历史*

	git log
	
*查看工作目录当前状态*

	git status
	
*合并分支*

	git merge 分支名
	
*放弃解决冲突，取消 merge*

	git merge --abort
	
*强制删除未合并的分支*

	git branch -D 分支名
	
*撤销最新的提交 *

	git reset --hard 目标commit
	
*临时存放工作目录的改动*

	git stash
	
	git stash pop	  // 取回改动


