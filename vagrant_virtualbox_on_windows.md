# Windows 搭建 vagrant + virtualbox

* 下载 vagrant 和 virtualbox。

> vagrant: [vagrantup.com](https://www.vagrantup.com)

> virtualbox: [virtualbox.org](https://www.virtualbox.org)

* 分别进行安装。

* 运行 `cmd` 打开命令行，开始使用 [vagrant](http://docs.vagrantup.com)。

### 常见问题

Q: `vagrant up` 启动失败。

A: 恢复 C:\Windows\System32 中 uxtheme.dll 文件。

Q: 更改 VAGRANT_HOME 路径（vagrant 下载 box 的存储路径）。

A: 在命令行中运行 `setx VAGRANT_HOME "path/to/yours"`。

Q: 更改 virtualbox 虚拟机安装位置。

A: 打开 virtualbox 软件图形界面，点击 管理 - 全局设定 - 常规 - 默认虚拟电脑位置 进行更改。

备注：

* box 可用资源链接：

> [vagrantbox.es](http://www.vagrantbox.es/)

> [atlas.hashicorp.com](https://atlas.hashicorp.com/boxes/search)