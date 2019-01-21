# Windows 开发相关小工具

### NSSM - the Non-Sucking Service Manager 将应用安装为 Windows 服务

下载 NSSM，然后将执行文件目录放入系统环境变量。

##### 使用：

**安装**：`nssm install <servicename> "/path/to/application"`

**移除**： `nssm remove <servicename>`

`path/to/application` 可以是可执行文件或者可执行的命令。

**示例**：将 Mycat 安装为服务

```
nssm install Mycat "mycat console"
```

更多参考：[NSSM](http://nssm.cc/usage)

### WinScp Windows 下使用 scp 传输文件

下载 WinScp，然后将执行文件目录放入系统环境变量。

##### 使用：

```
# 下载

winscp /console /command "option batch continue" "option confirm off" "open ssh:root:password@host:port" "option transfer binary" "get //path/to/dir/ E:\local\dir" "exit" /log=E:\log\dir\scp.log

# 上传

winscp /console /command "option batch continue" "option confirm off" "open ssh:root:password@host:port" "option transfer binary" "put E:\local\dir\ //path/to/dir/" "exit" /log=E:\log\dir\scp.log
```

更多参考：[WinScp Document](https://winscp.net/eng/docs/commandline)
