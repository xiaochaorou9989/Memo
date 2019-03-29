### 拉取docker镜像
```
docker pull gitlab/gitlab-ce
```

### 启动docker
```
sudo docker run -d -p 8443:443 -p 8081:80 -p 8022:22 \
--name gitlab \
--restart unless-stopped \
--volume /root/gitlabdir/config:/etc/gitlab \
--volume /root/gitlabdir/logs:/var/log/gitlab \
--volume /root/gitlabdir/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest	
```

### 删除docker
```
docker ps 
docker stop ab449b94fafb
docker rm ab449b94fafb
```

### 配置内网映射
```
vim gitlab.rb
external_url 'http://xxx:4' 
nginx['listen_port'] = 4
```

### 配置外网映射
```
sudo docker run -d -p 8443:443 -p 8081:4 -p 8022:22 \
--name gitlab \
--restart unless-stopped \
--volume /root/gitlabdir/config:/etc/gitlab \
--volume /root/gitlabdir/logs:/var/log/gitlab \
--volume /root/gitlabdir/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest	
```

### 说明
```
docker端口为4
外网端口为4
内网端口为8081
```