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

### springcloud集成配置
```
pom
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.amqp</groupId>
	<artifactId>spring-rabbit</artifactId>
</dependency>

yml
spring:
	rabbitmq:
		host: 192.168.3.23
		port:  5672
		username: guest
		password: guest
	zipkin:
		service:
			name: zipkin
		base-url: http://192.168.3.23:9411
		compression:
			enabled: true
		message-timeout: 6000
```

### 出现的问题
```
zipkin追踪不到消息
去掉mq依赖和远程配置中心正常
本质上应该是哪个节点没有通
单体到mq是否通畅
mq上报到zipkin是否通畅
```