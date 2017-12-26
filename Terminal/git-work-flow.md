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
	
### 备注


	