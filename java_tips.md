# JAVA TIPS

### CentOS 执行 ping 命令报错 `name or service not know`

```
vi /etc/sysconfig/network-scprits/ifcfg-ens33
# 在文件中找到 ONBOOT=NO 改成 ONBOOT=yes
service network restart
```

### 最小化安装

```
top
yum -y install lrzsz net-tools wget vim-enhanced lsof tar gcc
unzip zip
```

### 关闭防火墙

```
firewall-cmd --state
systemctl stop firewalld
```

### selinux 关闭

```
getenforce
vi /etc/selinux/config
```

### 安装 JDK

```
cd home    
mkdir java
cd java
tar -xvzf jdk-8u92-linux-x64.tar.gz
vim /etc/profile
    
	JAVA_HOME=/java/jdk /jdk1.8.0_92         
    JRE_HOME=$JAVA_HOME/jre/             
	CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar  
    PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH 
	
source /etc/profile   
java -version
```

### 安装 TOMCAT

```
rz
tar -xvzf apache-tomcat-8.5.35.tar.gz
cd bin
./startup.sh
tail -f catalina.out
ps -ef |grep tomcat
```

### 安装 MAVEN

```
# 下载源码
rz
tar -xvzf apache-maven-3.5.4-bin.tar.gz 
# 修改系统变量
vi /etc/profile

    export MAVEN_HOME=/home/maven/apache-maven-3.5.4 
    export PATH=$PATH:$MAVEN_HOME/bin        

# 使系统变量生效
source /etc/profile
mvn -version
```

### 安装 NEXUS

```
tar -zvxf nexus-2.12.0-01-bundle.tar.gz
./bin/nexus start
```

### 安装 MYSQL

```
# 下载安装
yum list installed MariaDB *
rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum install mysql mysql-server mysql-libs
service mysql start

# 开启远程连接(此处仅为演示，不建议开放 root 远程访问)
mysql -hlocalhost -uroot -p 
show databases;
use mysql;
select host, user from user;
update user set host='%' where user='root';
flush privileges;
```

### 安装 REDIS

```
# 下载安装
wget http://download.redis.io/releases/redis-5.0.2.tar.gz
tar xzf redis-5.0.2.tar.gz
cd redis-5.0.2
make MALLOC=libc

# 后台启动
mkdir myconf         
cp redis.conf /home/redis/myconf/            
# 修改redis.conf文件中的daemonize no为 yes
src/redis-server /home/redis/myconf/redis.conf           
```

### KAFAKA 安装

```
# Step 1: Download the code http://kafka.apache.org/
wget http://mirrors.hust.edu.cn/apache/kafka/2.1.0/kafka_2.11-2.1.0.tgz 
tar -xzf kafka_2.11-2.1.0.tgz                         
cd kafka_2.11-2.1.0  

# Step 2: Start the server
bin/zookeeper-server-start.sh config/zookeeper.properties
bin/kafka-server-start.sh config/server.properties

# Step 3: Create a topic
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
bin/kafka-topics.sh --list --zookeeper localhost:2181

# 远程连接配置 server.properties
# 配置 listeners=PLAINTEXT://:9092    host.name=192.168.1.220

# 集群部署
配置 advertised.listeners=PLAINTEXT://192.168.1.220:9092
```

### 主备切换

1. 进入目录：cd /usr/local/nginx/conf/vhosts

2. vim 编辑器打开：vi api.hnqst.cn.conf

    - i 进行编辑，然后才做更换#位置变换主备服务器
	
    - esc 退出编辑，再（:wq） 写入并退出
	
3. 测试配置文件：/usr/local/nginx/sbin/nginx -t

4. 平滑重启：/usr/local/nginx/sbin/nginx -s reload

### idea 快捷键

- Ctrl+Shift+N 搜索文件

- Ctrl+Shift+R 搜索关键字

- Shift+Shift 快捷搜索

- Ctrl+左键 被那些地方引用

### jRebel 注册

- http://139.199.89.239:1008/88414687-3b91-4286-89ba-2dc813b107ce

- 1026367714@qq.com

### 远程部署

```
    <build>
        <finalName>${project.artifactId}</finalName>

        <!-- 远程部署 -->
        <extensions>
            <extension>
                <groupId>org.apache.maven.wagon</groupId>
                <artifactId>wagon-ssh</artifactId>
                <version>2.8</version>
            </extension>
        </extensions>

        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
            <!-- 跳过单元测试 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <skipTests>true</skipTests>
                </configuration>
            </plugin>
            <!-- 远程部署 -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>wagon-maven-plugin</artifactId>
                <version>1.0</version>
                <configuration>
                    <fromFile>target/vblog-api.war</fromFile>
                    <url>scp://root:double-521@192.168.1.17/home/tomcat/apache-tomcat-8.5.35/webapps</url>
                    <commands>
                        <!-- 重启Tomcat -->
                        <command>sh /home/tomcat/apache-tomcat-8.5.35/bin/shutdown.sh</command>
                        <command>rm -rf /home/tomcat/apache-tomcat-8.5.35/webapps/vblog-api</command>
                        <command>sh /home/tomcat/apache-tomcat-8.5.35/bin/startup.sh</command>
                    </commands>
                    <!-- 显示运行命令的输出结果 -->
                    
                    <!--clean package wagon:upload-single wagon:sshexec-->
                   
                    <displayCommandOutputs>true</displayCommandOutputs>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

### nginx 安装

```
wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
yum install nginx
sudo systemctl start nginx
sudo yum install epel-release
sudo yum install nginx
sudo systemctl start nginx
```

### nginx 部署 vue

编辑 `/etc/nginx/nginx.conf`

```
    user  nginx;
    worker_processes  1;
    
    error_log  /var/log/nginx/error.log warn;
    pid        /var/run/nginx.pid;
    
    
    events {
        worker_connections  1024;
    }
    
    
    http {
        include       /etc/nginx/mime.types;
        default_type  application/octet-stream;
    
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
    
        access_log  /var/log/nginx/access.log  main;
    
        sendfile        on;
        #tcp_nopush     on;
    
        keepalive_timeout  65;
    
        #gzip  on;
    
        #开始 vue 配置
        server{
            listen 80;
            server_name localhost;
            root /home/nginx/sunrain;
     
            location /{
                index index.html;
            }
    
            location /api/{
                proxy_pass http://192.168.1.220:8080/service-auth/api/;
                proxy_set_header Host $http_host;
            }
        }
        #结束 vue 配置
    
        include /etc/nginx/conf.d/*.conf;
    }
```

重载配置

```
nginx -t
nginx -s reload
```

### git

```
git rm --cached somefile #取消跟踪   
git commit -m "remove somefile added by mistake"
git status 查看状态     
git fetch & git pull 更新        
git add .             
git commit -m "first commit"     
git push            
git log --oneline --graph --decorate --all  #git 树
```

参考：[git 教程](https://www.yiibai.com/git/git_remote.html)

### ova 怎么安装到 vmware 虚拟机里面去

1. 打开VMWare WorkStation, 点击“打开虚拟机”，选择要导入的ova文件；

2. 点击确认后，会提示让你选择要导入的虚拟机的“存储路径”；设置完存储路径后，点击导入；

3. 系统执行导入文件操作（如果ova文件很大的话，花费的时间也会较长）；

4. 导入完成后，点击打开虚拟机即可；
