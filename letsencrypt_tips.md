# 使用 Let's Encrypt with certbot

### 安装 certbot

```
:~# apt-get update
:~# apt-get install software-properties-common
:~# add-apt-repository ppa:certbot/certbot
:~# apt-get update
:~# apt-get install python-certbot-nginx 
```

### nginx 配置

```
# server 段
location ^~ /.well-known/acme-challenge/ {
   default_type "text/plain";
   root     /home/wwwroot/jay4497.cn/;  // 持有网站的根目录
}
 
location = /.well-known/acme-challenge/ {
   return 404;
}
```

然后重新加载配置

```
:~# /etc/init.d/nginx reload
```

### 生成证书 with plugin webroot

```
:~# certbot certonly --webroot -w /home/wwwroot/jay4497.cn/ -d jay4497.cn -d www.jay4497.cn
```

注：`w` 持有网站的根目录。 `d` 要生成证书的域名，多个域名可继续添加此参数及值。

### 重新生成证书，续期

```
:~# certbot renew --dry-run // 测试是否可以成功生成，不实际生成
:~# certbot renew
```

### nginx 配置 SSL

```
server {
  listen 443 ssl;
  listen [::]:443 ssl ipv6only=on;

  ssl_certificate /etc/letsencrypt/live/your.domain.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/your.domain.com/privkey.pem;
  ssl_trusted_certificate /etc/letsencrypt/live/your.domain.com/chain.pem;
  
  ..... 其它通用配置
}
```

把 80 端口重定向到 443 端口

```
server{
  listen 80;
  server_name jay4497.cn www.jay4497.cn;
  return 301 https://$server_name$request_uri;
}
```
