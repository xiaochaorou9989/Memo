### 拉取docker镜像
```
docker pull jenkins
```

### 启动docker
```
docker run -itd -p 8083:8080 -p 50000:50000 --name jenkins --privileged=true  -v /root/jenkinsdir:/var/jenkins_home jenkins:latest
```

### 查看密码
```
docker ps 
docker exec -it 66e05f3ffb52 /bin/bash
cat initialAdminPassword
```
