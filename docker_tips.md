### 安装docker

1. Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
	- 通过 uname -r 命令查看你当前的内核版本
	- $ uname -r

2. 使用 root 权限登录 Centos。确保 yum 包更新到最新。
	- $ sudo yum update

3. 卸载旧版本(如果安装过旧版本的话)
	- $ sudo yum remove docker  docker-common docker-selinux docker-engine

4. 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
	- $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

5. 设置yum源
	- $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 
6. 可以查看所有仓库中所有docker版本，并选择特定版本安装
	- $ yum list docker-ce --showduplicates | sort -r

7. 安装docker
	- $ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
	- $ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
 
8. 启动并加入开机启动
	- $ sudo systemctl start docker
	- $ sudo systemctl enable docker

9. 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
	- $ docker version

### 问题

1. 因为之前已经安装过旧版本的docker，在安装的时候报错如下：

```
Transaction check error:
  file /usr/bin/docker from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd-shim from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/dockerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
```

2. 卸载旧版本的包
	- $ sudo yum erase docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64

3. 再次安装docker
	- $ sudo yum install docker-ce
	
### Docker Compose 安装

**Docker Compose 是 Docker 的独立产品，因此需要安装 Docker 之后在单独安装 Docker Compose .** 

- 安装
```
#下载
sudo curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
#安装
chmod +x /usr/local/bin/docker-compose
#查看版本
docker-compose version
```

- 项目结构
```
.
├── docker-compose.yml
├── Dockerfile
├── requirements.txt
└── server.py
```

- 常用命令
```
docker-compose up #启动
docker-compose stop #停止
docker-compose ps #查看
docker-compose rm #删除
```


### 新建Dockerfile
在应用目录下新建Dockerfile

```
FROM node:8
WORKDIR /usr/src/app
ADD . /usr/src/app
RUN npm install --registry=https://registry.npm.taobao.org
USER node
EXPOSE 8080
```
8080 端口请根据实际情况调整。建议大于1024，否则使用node用户启动应用时可能权限不足

### 新建docker-compose.yml
在应用目录下新建docker-compose.yml

```
version: "2"
services:
  jsconsole:
    build: .
    volumes:
      - .:/usr/src/app
      - /usr/src/app/node_modules
    ports:
      - "127.0.0.1:8081:8080"
    command: "npm start"
    restart: always
```

127.0.0.1:8081是宿主机IP和端口，我前端采用了nginx做反向代理。如果是直接提供公网服务的话把127.0.0.1去掉

### 构建镜像
在应用根目录下执行

```
docker-compose build
```

构建并运行
在应用根目录下执行-d是后台执行

```
docker-compose up -d
```