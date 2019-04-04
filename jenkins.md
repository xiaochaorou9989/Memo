## jenkins安装

### 拉取docker镜像
```
docker pull jenkinsci/jenkins
```

### 启动docker
```
docker run \
-d \
-p 8083:8080 \
-p 50000:50000 \
--name jenkins \
--link gitlab:git.hnshlwyy.com \
-u root \
-v /root/jenkinsdir:/var/jenkins_home \
jenkinsci/jenkins:latest
```

### 查看密码（此密码只用来安装jenkins）
```
cat /root/jenkinsdir/initialAdminPassword
```

### 安装插件
```
Gitlab Hook	 完成
GitLab	 完成
Build Authorization Token Root 完成
```

---

## jenkins集成gitlab

### 新建任务（从git上拉取代码）
```
源码管理->Git->Repository URL：git项目地址
源码管理->Git->Credentials：可直接输入账号密码
```

### 构建触发器（什么时候触发服务器maven构建）
```
触发远程构建 (例如，使用脚本)：访问生成的连接可直接进行构建
轮询 SCM：定时构建（*/10 * * * *   每隔10分钟一次）
Build when a change is pushed to GitLab.(配合gitlab的webhook使用)->高级->Secret token->generate：URL和Secret token复制到gitlab里的webhook里

gitlab设置
因为gitlab和jenkins用的同一个域名，首先要设置gitlab可访问本地域名
用超级管理员登录gitlab->点击导航上的设置图标->setting->network->Outbound requests [expand]->Allow requests to the local network from hooks and services
登录个人账号->选择要构建的项目->setting->Integrations（集成）->添加url和Secret Token [add webhook]->test [push events]

注意
因为gitlab和jenkins用的同一个域名
gitlab的webhook在添加url时用的内网ip加端口（经过测试可成功通信）
gitlab的钩子不仅仅只能和jenkins搭配，可以自定义部署接口
```

### 构建后操作（构建后通常会部署jar到服务器上并启动）
**部署方案：把jenkins里构建好的jar包远程部署到宿主机里（docker宿主机的内网ip为172.17.0.1）**
```
jenkins后台->系统管理->系统设置->Publish over SSH->新增：填写宿主机的账号密码和目录

构建后操作表单填写：
Source files：**/*.jar（所有的jar包）
Remote directory：web（拷贝到目标机器的目录，因为之前在Publish over SSH里填写了/root目录，所以我要拷贝到/root/web只需填写web即可）
Exec command：bash /root/web/start.sh（执行命令，我这里的命令是执行了一个脚本）
```

### start.sh脚本内容（springboot项目，因为命令里要把显示打印到catalina.out里，所以要提前新建catalina.out文件）
```
#start.sh
#!/bin/bash
export PATH=/usr/java/jdk1.8/bin:$PATH
echo "Restarting SpringBoot Application"
pid=`ps -ef | grep demo-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}'`
if [ -n "$pid" ]
then
   kill -9 $pid
   echo "关闭进程："$pid
fi

echo "授予当前用户权限"
chmod 777 /root/web/target/demo-0.0.1-SNAPSHOT.jar
echo "执行....."
java -jar /root/web/target/demo-0.0.1-SNAPSHOT.jar > /root/web/catalina.out  2>&1 &
```

---

## 其他的操作
```
在系统设置里配置gitlab的token；Publish over SSH配置免密登录（可能会用到）
在全局安全配置里去掉跨站请求伪造保护（可能会用到）
全局工具配置编辑（可能会用到）
```


---
## 常用命令

### docker常用命令
```
docker ps #列出docker
docker stop 4dbb32a06464 #暂停docker
docker rm 4dbb32a06464 #移除docker
docker restart 4dbb32a06464 #重启docker
docker exec -it 4dbb32a06464 /bin/bash #进入docker
```

### linux常用命令
```
netstat -tunlp|grep #查看端口号
```