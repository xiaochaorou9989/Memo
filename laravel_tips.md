1. composer install 

2. 遇到的问题

	- composer.json 里有多余的逗号

	- 没有打开php_openssl、php_fileinfo

	- 报错没具体报错信息，修改配置文件

		- config\app.php debug = true

	- Laravel 出现"RuntimeException inEncrypter.php line 43: The only supported ciphers are AES-128-CBC and AES-256-CBC with the correct key lengths."

		- php artisan key:generate

	- 出现

		[ErrorException]
		file_get_contents(D:\ProgramFiles\phpStudy\PHPTutorial\WWW\laravelcms-master/.env): failed to open stream: No such
		file or directory

		- .env.example copy 为 .env

	- 数据库连接错误

		- 改下配置文件

ok