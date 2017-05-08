## git 操作

### tag
1. 生成tag

	`git tag -a tagName -m "message"`
	
2. 删除tag

	`git tag -d tagName`
	
3. 删除远程tag

	`git push origin --delete tag v5.0.4 `	
3. 上传tag

	`git push origin tagName`
	
4. 上传所有tag

	`git push origin -tags`
	
### 分支
1. 拉取新的分支
	* 先切到master分支`git checkout master`;
	* 在master分支上切到一个和远程分支名字相同的分支`git checkout -b develop`;
	* 在develop分支上pull远程的develop分支`git pull origin develop`;

2. 合并分支
	* 从临时分支0508_14checkout到主分支develop;
	* 在develop分支中`git merge --squash 0508_14`;

