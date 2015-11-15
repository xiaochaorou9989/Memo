# vim tips

### On Windows:

##### 取消 vim 自动保存备份文件。

找到 vim 安装目录，一般为 __vim版本号__，如 vim74。然后打开 vimrc_example.vim 文件，定位到代码段：
```
if has("vms")
  set nobackup		" do not keep a backup file, use versions instead
else
  set backup		" keep a backup file
endif
```
注释掉 `else` 段，如下：
```
if has("vms")
  set nobackup		" do not keep a backup file, use versions instead
"else
"  set backup		" keep a backup file
endif
```
保存退出。