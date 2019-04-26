docker pull mysql:5.6

docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -v $PWD/conf:/etc/mysql/conf.d -v $PWD/data:/var/lib/mysql -v $PWD/logs:/logs --name mysql mysql:5.6