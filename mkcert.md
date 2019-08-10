# 本地测试开发使用 SSL 证书（mkcert）

`mkcert` 仓库地址 [https://github.com/FiloSottile/mkcert](https://github.com/FiloSottile/mkcert)，本文档参考此地址。

### 安装 mkcert

**Windows**

使用 [Chocolatey](https://chocolatey.org/) 来安装 mkcert

```
choco install mkcert
```

PS: Chocolatey 真是个好东西！

**Linux**

```
go get -u github.com/FiloSottile/mkcert
$(go env GOPATH)/bin/mkcert
```

### 生成证书

```
# 安装本地信任证书
mkcert -install
# 为域名生成证书，可多个，空格隔开
mkcert localhost 127.0.0.1 example.com
```

命令执行成功会提示有证书保存位置，用于下一步配置 SSL。

### 配置 Nginx

```
# server 段
ssl_certificate     /etc/nginx/ssl/localhost.pem;
ssl_certificate_key /etc/nginx/ssl/localhost-key.pem;
```
