### rabbitmq
```
service docker start
docker pull rabbitmq:management
docker run -d --hostname my-rabbit --name rabbit -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest -p 15672:15672 -p 5672:5672 -p 25672:25672 -p 61613:61613 -p 1883:1883 rabbitmq:management
```

### zipkin
```
https://zipkin.io/
docker run -d -p 9411:9411 openzipkin/zipkin
```

### docker-compose安装方法（简单安装，服务隔离，且解决因为没配置好zipkin没有收到消息的问题）
```
克隆yml文件 git clone https://github.com/fleam/zipkin-rabbitmq-docker-compose.git
替换docer-compose https://dn-dao-github-mirror.daocloud.io/docker/compose/releases/download/1.24.0/docker-compose-Linux-x86_64
查看镜像 docker images
删除镜像 docker rmi fd990a821543
重启docker systemctl restart  docker
docker-compose build
docker-compose build --no-cache
docker-compose up
```