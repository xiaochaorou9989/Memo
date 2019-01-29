# Git tips

### 合并历史提交 combine commits

`git log` 找到要合并的提交之前的提交的哈希值，如 1a2b3c，然后执行 rebase 操作：

`git rebase -i 1a2b3c`

会得到如下格式数据：

```
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

```
// on branch master
git checkout dev readme.md base.php
git add -A
git commit -m "modify files"
```

以上命令将 `dev` 分支的 `/readme.md` 和 `/base.php` 文件合并到 `master` 分支。**注意**，此合并会用 `dev` 分支上的指定文件强制覆盖当前分支的文件，即使当前分支的文件与 `dev` 分支的文件存在冲突。

### 仅克隆/拉取仓库的指定目录

使用 sparse-checkout 来实现。

```
mkdir myproject
cd myproject
git init
git config core.sparsecheckout true
echo '/path/to/dir/you/want/in/repo' >> .git/info/sparse-checkout
git remote add origin https://github.com/theRepoYouWant.git
git pull origin master
```

### 克隆指定分支到指定目录

```
# format
git clone -b branchname https://host/path/to/demo.git /path/to/local

# sample 克隆 Laravel 的 develop 分支到本地 ./blog 目录
git clone -b develop https://github.com/laravel/laravel.git ./blog/
```


### 更新及查看日志

```
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
