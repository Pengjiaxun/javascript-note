# Git

### 分支管理

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### 版本管理

**文件提交流程**

工作区file —> `git add` —> 暂存区file —> `git commit` —> 版本库file —> `git push` —> 同步到远程仓库

**撤销修改**

1.撤销工作区的修改：`git checkout -- <file>`

   >包含两种情况：
   >1. 一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
   >2. 一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

2.撤销掉暂存区的修改：`git reset HEAD <file>`

撤销后暂存区是干净的，工作区是有修改的。

3.撤销提交的修改

`git reset --hard/soft/mixed <目标版本号>`

使用此命令后，因为版本版本比远程仓库版本要老，所以不能使用`git push`，使用`git push -f`强制推送更新。

hard/soft/mixed区别
- hard：代码修改内容全部丢弃
- soft：回到commit之前，add之后
- mixed：等于`git reset HEAD <file>`，回到add之前

`git revert`：新增一个提交来回到之前的某个版本

4.删除文件

在本地删除文件后，使用`git rm <file>`命令并`git commit`，版本库的文件会同步删除。

### bug处理

当开发中遇到需要紧急修复的bug，但是当前开发工作只进行到一半而无法提交代码，此时可以使用`git stash`命令把当前的修改内容存起来，待bug修复后再还原。

查看stash记录：`git stash list`

还原并删除记录：`git stash pop`

还原不删除记录：`git stash apply`
