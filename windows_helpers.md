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
