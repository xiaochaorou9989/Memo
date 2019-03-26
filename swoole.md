```
yum install -y perl*
yum install -y httpd-devel
```

```
libmcrypt-2.5.8-9.el6.x86_64.rpm 
https://centos.pkgs.org/6/epel-x86_64/libmcrypt-devel-2.5.8-9.el6.x86_64.rpm.html
libmcrypt-devel-2.5.8-9.el6.x86_64.rpm
http://rpmfind.net/linux/rpm2html/search.php?query=libmcrypt-devel(x86-64)
yum localinstall libmcrypt-2.5.8-9.el6.x86_64.rpm 
yum localinstall libmcrypt-devel-2.5.8-9.el6.x86_64.rpm
```

```
yum -y install gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs e2fsprogs-devel krb5 krb5-devel libidn libidn-devel openssl openssl-devel openldap openldap-devel nss_ldap openldap-clients openldap-servers gd gd2 gd-devel gd2-devel perl-CPAN pcre-devel
```

```
cd php-5.5.10/
./configure --prefix=/usr/local/php --with-config-file-path=/etc/php --enable-fpm --enable-pcntl --enable-mysqlnd --enable-opcache --enable-sockets --enable-sysvmsg --enable-sysvsem  --enable-sysvshm --enable-shmop --enable-zip --enable-ftp --enable-soap --enable-xml --enable-mbstring --disable-rpath --disable-debug --disable-fileinfo --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-pcre-regex --with-iconv --with-zlib --with-mcrypt --with-gd --with-openssl --with-mhash --with-xmlrpc --with-curl --with-imap-ssl
sudo make
sudo make install
sudo cp php.ini-development /etc/php
```

```
~/.bashrc
export PATH=/usr/local/php/bin:$PATH
export PATH=/usr/local/php/sbin:$PATH
source ~/.bashrc
php --version
```

```
Swoole
https://github.com/swoole/swoole-src/releases
cd swoole-src-swoole-1.7.6-stable/
phpize
./configure --enable-async-mysql
sudo make
sudo make install
```

```
find -name php.ini
/usr/local/php/bin/php --ini
extension=swoole.so
php -m
php -m | grep "swoole"
mkdir phps
mv php phps
cd php
mv php php.ini
cd ../
mv phps php
php -m | grep "swoole"
```

```
touch server.php
touch client.php
```

```
// Server
class Server
{
    private $serv;

    public function __construct() {
        $this->serv = new swoole_server("0.0.0.0", 9501);
        $this->serv->set(array(
            'worker_num' => 8,
            'daemonize' => false,
            'max_request' => 10000,
            'dispatch_mode' => 2,
            'debug_mode'=> 1
        ));

        $this->serv->on('Start', array($this, 'onStart'));
        $this->serv->on('Connect', array($this, 'onConnect'));
        $this->serv->on('Receive', array($this, 'onReceive'));
        $this->serv->on('Close', array($this, 'onClose'));

        $this->serv->start();
    }

    public function onStart( $serv ) {
        echo "Start\n";
    }

    public function onConnect( $serv, $fd, $from_id ) {
        $serv->send( $fd, "Hello {$fd}!" );
    }

    public function onReceive( swoole_server $serv, $fd, $from_id, $data ) {
        echo "Get Message From Client {$fd}:{$data}\n";
    }

    public function onClose( $serv, $fd, $from_id ) {
        echo "Client {$fd} close connection\n";
    }
}
// 启动服务器
$server = new Server();
```

```
<?php
class Client
{
	private $client;

	public function __construct() {
		$this->client = new swoole_client(SWOOLE_SOCK_TCP);
	}
	
	public function connect() {
		if( !$this->client->connect("127.0.0.1", 9501 , 1) ) {
			echo "Error: {$fp->errMsg}[{$fp->errCode}]\n";
		}
		$message = $this->client->recv();
		echo "Get Message From Server:{$message}\n";

		fwrite(STDOUT, "请输入消息：");  
		$msg = trim(fgets(STDIN));
		$this->client->send( $msg );
	}
}

$client = new Client();
$client->connect();
```