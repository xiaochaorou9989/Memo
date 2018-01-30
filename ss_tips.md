# 科学上网 ss

### 安装

```
apt-get install python-pip
pip install shadowsocks
```

### 配置

```
// file: /etc/shadowsocks.json
{
    "server": "138.68.254.220",
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "port_password": {  // 多用户配置
        "8837": "password",
        "8838": "password_1"
    },
    "timeout": 600,
    "method": "aes-256-cfb",
    "fast_open": false
}
```

### 后台启动

```
ssserver -c /etc/shadowsocks.json -d start
```
