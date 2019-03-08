# Git tips

### 合并历史提交 combine commits

`git log` 找到要合并的提交之前的提交的哈希值，如 1a2b3c，然后执行 rebase 操作：

`git rebase -i 1a2b3c`

会得到如下格式数据：

```shell
pick abc356
pick 356abc
pick xxxx00
...
```

将要合并的提交前的 pick 修改为 squash，保存退出。之后会提示编辑这次合并的提交描述，编辑完保存退出即可。

合并完毕。

如果要 push 到远程，需要给 push 添加 -f 参数。

### 撤销所有未提交暂存区的修改

`git checkout .`

### 合并其它分支的指定文件(覆盖)到当前分支

```shell
// on branch master
git checkout dev readme.md base.php
git add -A
git commit -m "modify files"
```

以上命令将 `dev` 分支的 `/readme.md` 和 `/base.php` 文件合并到 `master` 分支。**注意**，此合并会用 `dev` 分支上的指定文件强制覆盖当前分支的文件，即使当前分支的文件与 `dev` 分支的文件存在冲突。

### 仅克隆/拉取仓库的指定目录

使用 sparse-checkout 来实现。

```shell
mkdir myproject
cd myproject
git init
git config core.sparsecheckout true
echo '/path/to/dir/you/want/in/repo' >> .git/info/sparse-checkout
git remote add origin https://github.com/theRepoYouWant.git
git pull origin master
```

### 克隆指定分支到指定目录

```shell
# format
git clone -b branchname https://host/path/to/demo.git /path/to/local

# sample 克隆 Laravel 的 develop 分支到本地 ./blog 目录
git clone -b develop https://github.com/laravel/laravel.git ./blog/
```

### 当前分支不提交且干净地切换到其它分支开始工作

```shell
# on branch new_ver
git stash
git checkout old_ver
git add .
git commit -m "old_ver complete"
git checkout new_ver
git stash pop
# git stash list 列出当前暂存
```

在重构旧版本，且旧版本暂时仍在服务期时，频繁切换分支工作，`git stash` 很有用。

### 更新及查看日志

```shell
# 更新
git fetch origin

# 本地与远程的差集
git log master..origin/master

# 统计文件的改动
git diff -3 --stat master origin/master
git log -3 --stat dev..origin/dev

# 可以看到fileName相关的commit记录
git log filename

# 可以显示每次提交的diff
git log -p filenam

# 只看某次提交中的某个文件变化，可以直接加上fileName
git show c5e69804bbd9725b5dece57f8cbece4a96b9f80b filename
```

```shell
# 撤销工作区中当前目录中的所有更改
git checkout .

# 撤销工作区中指定文件的修改
git checkout -- filename

# 切换到指定的分支（如果分支在本地不存在，会自动从远程仓库拉取下来）
git checkout develop
 
# 切换分支的同时，创建分支
git checkout -b my-dev

# 针对文件回退本地修改
git checkout HEAD file/to/restore 

# 查看工作区和版本库的区别
git diff

# 比较两个版本之间的差异
git diff 4129523 0a7d9af

# 比较之前的版本和当前版本的差异
git diff 4129523 head
 
# 比较某个文件在两个版本之间的差异
git diff 09d9b45 head ./config/app.php
 
# 比较之前的版本和当前版本的差异的简写形式
git diff 4129523

# 比较 develop 分支和 master 分支的区别
git diff develop master

# 对比本地的 develop 分支和远程的 master 分支的区别
git diff develop origin/master

# 查看版本库的历史记录
git log
 
# 查看版本库的历史记录，美化输出
git log --pretty=oneline
 
# 查看版本库的历史记录，只显示前 5 条
git log -5
git log -5 --pretty=oneline

# 回滚到指定的版本
git reset --hard e377f60e28c8b84158 

# 撤销工作区和暂存区中的修改
git reset --hard

# 撤销暂存区中的修改
git reset

# 回退到上一个版本
git reset --hard head^

# 回退到上一个版本
git reset --hard HEAD^ 

# 查看当前的版本号
git rev-parse HEAD

# 强制提交
git push -f origin master 

# 打标签并添加说明（-m后面的可以不写）
git tag -a v3.2.1 -m '线上版本' 

# 将标签提交到远程仓库
git push origin v3.2.1 
	
# 查看标签
git tag 

# 查看指定版本号
git show v3.2.1 

####

# error: 
Your local changes to the following files would be overwritten by merge:
Please, commit your changes or stash them before you can merge.

# 先将本地修改存储起来
git stash 

# pull内容
git pull 

# 还原暂存的内容
git stash pop 

# 确认代码自动合并的情况
git diff -w +文件名 
```
