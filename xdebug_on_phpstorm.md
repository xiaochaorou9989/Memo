# Set Xdebug on PHPStorm

### pre

Xdebug 2.4.1

PHPStorm 2017.2

### php.ini

```
[XDebug]
zend_extension="path/to/php_xdebug.dll"
xdebug.remote_enable=1
xdebug.remote_host=host_ip
xdebug.remote_port=9050
xdebug.remote_autostart=1
xdebug.remote_log="path/to/xdebug.log"
xdebug.idekey="PHPSTORM"
```

**注意**：

本地调试时，`remote_host` 要填本机 IP 地址，用 `localhost` 可能会出现如 `Could not connect to client. :-(` 的错误。

`idekey` 设置要统一。

`remote_port` 要保证不冲突可用。

### PHPStorm

```
File/Settings/Languages & Frameworks/PHP/Debug/Xdebug/Debug port: 9050
```

与 php.ini 中 remote_port 一致

```
File/Settings/Languages & Frameworks/PHP/Servers
```

创建新项：Name 随意，Host 与 php.ini 中 remote_host 一致，端口为要调试的项目的访问端口，网站一般为 80，Debugger 选 Xdebug

```
Run/Edit Configurations	
```

创建新项，PHP Web Application，Name 随意，URL 要调试的项目地址

### Xdebug Broswer helper

主要用于自动给调试 url 添加必要的通知参数（XDEBUG_SESSION=PHPSTORM），一般为设置 Cookie，helper 里也要设置 IDEKey，保持与上面设置一致即可。

谷歌浏览器有 `Xdebug helper` 扩展，推荐使用。

如果不借助 helper 而手动设置参数的话，`XDEBUG_SESSION` 的值即为上面设置的 IDEKey。

### Debug

PHPStorm 右上角工具栏，点击监听按钮（像电话的那个）开始监听，然后打好断点，浏览器访问页面，开始 Debug 吧。

END.