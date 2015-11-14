# 在 Ubuntu 中编译安装部署 LAMP 环境

### 准备：
Ubuntu 14.04 LTS 64 位

> apache: http://mirror.bit.edu.cn/apache/httpd/httpd-2.4.17.tar.gz
	apr: http://mirror.bit.edu.cn/apache//apr/apr-1.5.2.tar.gz
	apr-util: http://mirror.bit.edu.cn/apache//apr/apr-util-1.5.4.tar.gz
	pcre: ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz
	
> mysql: http://repo.mysql.com/mysql-apt-config_0.5.3-1_all.deb
	
> php: http://cn2.php.net/distributions/php-5.6.15.tar.gz

> xdebug: http://xdebug.org/files/xdebug-2.3.3.tgz

```
apt-get install build-essential
apt-get install libxml2-dev libcurl14-gnutls-dev libjpeg-dev libpng12-dev libicu-dev ldap-utils libssl-dev autoconf
```

### 安装 apr, apr-util，pcre。
Q： cannot remove 'libtoolT': No such file or directory.

A: Edit configure file. Change the line `$RM "$cfgfile"` to `$RM -f "$cfgfile"`。

### 安装 apache。
Q: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory.
A: `ln -s /usr/local/lib/libpcre.so.1 /lib` or `ln -s /usr/local/lib/libpcre.so.1 /lib64`

### 安装 mysql。
```
dpkg -i mysql-apt-config_0.5.3-1_all.deb
apt-get update
apt-get install mysql-server
```

### 安装 php。
```
./configure --enable-mysqlnd --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-opcache --enable-pcntl -enable-mbstring --enable-soap --enable-zip --enable-calendar --enable-bcmath --enable-exif --enable-ftp --enable-ftp --enable-intl --with-openssl --with-zlib --with-curl --with-gd --with-gettext --with-mhash --with-ldap=/usr/lib/x86_64-linux-gnu --disable-fileinfo --with-apxs2=/usr/local/apache2/bin/apxs
make && make install
```

##### 安装 php 扩展
进入 php 源码中 ext 目录，找到要安装的扩展并进入目录。然后执行以下操作：
```
phpize --with-php-config=/usr/local/bin/php-config
./configure
make && make install
```
最后在 php.ini 中添加 `extension=module.so`。

Q: configure php源码的时候报错： Cannot find ldap libraries in /usr/lib。

A: 去掉 `configure --with-ldap` 参数进行安装，安装完成后按照 **安装 php 扩展** 的方法单独安装 ldap 扩展，configure 的时候如果依然报错，加上 `--with-libdir=/usr/lib/x86_64-linux-gnu` 参数。

##### 安装 xdebug。
```
tar -zxvf xdebug-2.3.3.tgz
cd xdebug-2.3.3
phpize --with-php-configure=/usr/local/bin/php-config
./configure --enable-xdebug
make && make install
```
最后添加 `zend_extension=xdebug.so` 到 php.ini 中。