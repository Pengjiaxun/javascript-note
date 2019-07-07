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

```flow

st=>start: 工作区(本地)
e=>end: 远程仓库
op1=>operation: git add
after_add=>inputoutput: 暂存区
op2=>operation: git commit
after_commit=>inputoutput: 本地版本库
op3=>operation: git push

st->op1->after_add->op2->after_commit->op3->e
```
<br/>

**撤销修改**

撤销位置|命令|说明
:------:|:--:|:--
工作区  |git checkout -- <file>|1. file修改后还没有被放到暂存区，撤销修改就回到和版本库一模一样的状态；<br/>2. file已经添加到暂存区后，又作了修改，撤销修改就回到添加到暂存区后的状态。
暂存区  |git reset --hard/soft/mixed <版本号>|hard：代码修改内容全部丢弃<br/>soft：回到commit之前，add之后<br/>mixed：等于git reset HEAD <file>，回到add之前<br/><br/>**注意：使用此命令后，因为本地版本比远程仓库版本要老，所以不能使用git push，要使用git push -f强制推送更新。**

`git revert <版本号>`：新增一个提交来回到之前的某个版本

**恢复撤销的代码**

只要通过`git reflog`找到想要回退的commit id，执行`git reset --hard <版本号>`即可。


**删除文件**

在本地删除文件后：
1. 如果确实要从版本库中删除此文件，那就使用`git rm <file>`命令并`git commit`，版本库的文件会同步删除；
2. 如果是误删的话，此时版本库里面仍然有该文件，可以通过`git checkout -- <file>`恢复。

### bug处理

当开发中遇到需要紧急修复的bug，但是当前开发工作只进行到一半而无法提交代码，此时可以使用`git stash`命令把当前的修改内容存起来，待bug修复后再还原。

查看stash记录：`git stash list`

还原并删除记录：`git stash pop`

还原不删除记录：`git stash apply`
