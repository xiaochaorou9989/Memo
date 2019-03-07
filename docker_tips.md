### ��װdocker

1. Docker Ҫ�� CentOS ϵͳ���ں˰汾���� 3.10 ���鿴��ҳ���ǰ����������֤���CentOS �汾�Ƿ�֧�� Docker ��
	- ͨ�� uname -r ����鿴�㵱ǰ���ں˰汾
	- $ uname -r

2. ʹ�� root Ȩ�޵�¼ Centos��ȷ�� yum �����µ����¡�
	- $ sudo yum update

3. ж�ؾɰ汾(�����װ���ɰ汾�Ļ�)
	- $ sudo yum remove docker  docker-common docker-selinux docker-engine

4. ��װ��Ҫ��������� yum-util �ṩyum-config-manager���ܣ�����������devicemapper����������
	- $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

5. ����yumԴ
	- $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
 
6. ���Բ鿴���вֿ�������docker�汾����ѡ���ض��汾��װ
	- $ yum list docker-ce --showduplicates | sort -r

7. ��װdocker
	- $ sudo yum install docker-ce  #����repo��Ĭ��ֻ����stable�ֿ⣬�����ﰲװ���������ȶ���17.12.0
	- $ sudo yum install <FQPN>  # ���磺sudo yum install docker-ce-17.12.0.ce
 
8. ���������뿪������
	- $ sudo systemctl start docker
	- $ sudo systemctl enable docker

9. ��֤��װ�Ƿ�ɹ�(��client��service�����ֱ�ʾdocker��װ�������ɹ���)
	- $ docker version

### ����

1. ��Ϊ֮ǰ�Ѿ���װ���ɰ汾��docker���ڰ�װ��ʱ�򱨴����£�

```
Transaction check error:
  file /usr/bin/docker from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/docker-containerd-shim from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
  file /usr/bin/dockerd from install of docker-ce-17.12.0.ce-1.el7.centos.x86_64 conflicts with file from package docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64
```

2. ж�ؾɰ汾�İ�
	- $ sudo yum erase docker-common-2:1.12.6-68.gitec8512b.el7.centos.x86_64

3. �ٴΰ�װdocker
	- $ sudo yum install docker-ce
	
### Docker Compose ��װ

**Docker Compose �� Docker �Ķ�����Ʒ�������Ҫ��װ Docker ֮���ڵ�����װ Docker Compose .** 

- ��װ
```
#����
sudo curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
#��װ
chmod +x /usr/local/bin/docker-compose
#�鿴�汾
docker-compose version
```

- ��Ŀ�ṹ
```
.
������ docker-compose.yml
������ Dockerfile
������ requirements.txt
������ server.py
```

- ��������
```
docker-compose up #����
docker-compose stop #ֹͣ
docker-compose ps #�鿴
docker-compose rm #ɾ��
```


### �½�Dockerfile
��Ӧ��Ŀ¼���½�Dockerfile

```
FROM node:8
WORKDIR /usr/src/app
ADD . /usr/src/app
RUN npm install --registry=https://registry.npm.taobao.org
USER node
EXPOSE 8080
```
8080 �˿������ʵ������������������1024������ʹ��node�û�����Ӧ��ʱ����Ȩ�޲���

### �½�docker-compose.yml
��Ӧ��Ŀ¼���½�docker-compose.yml

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

127.0.0.1:8081��������IP�Ͷ˿ڣ���ǰ�˲�����nginx��������������ֱ���ṩ��������Ļ���127.0.0.1ȥ��

### ��������
��Ӧ�ø�Ŀ¼��ִ��

```
docker-compose build
```

����������
��Ӧ�ø�Ŀ¼��ִ��-d�Ǻ�ִ̨��

```
docker-compose up -d
```