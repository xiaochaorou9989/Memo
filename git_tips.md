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