# Ubuntu tips

### 网卡

**启动网卡**

```
root@host:~# ifconfig eth0 up
```

**刷新地址**

```
root@host:~# dhclient eth0
```

**网卡信息设置**

```
# file: /etc/network/interfaces
auto lo 
iface lo inet loopback // 这边的设置是本地回路

auto eth0 
iface eth0 inet static // 设置为静态地址
# iface eth0 inet dhcp  // 自动获取 IP
address 192.168.1.230 // IP 地址
netmask 255.255.255.0 // 子网掩码
gateway 192.168.1.1 // 网关
```
