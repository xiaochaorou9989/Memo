# mysql cli tips

### login

```shell
mysql -hlocalhost -uroot -p
```

> -h: server address
>
> -u: user name
>
> -p: password，可直接在参数后跟上密码(`-ppassword`)也可不跟然后按照提示输入密码。

### backup database

```shell
mysqldump -hlocalhost -uroot -p -q database_name > /path/to/backup/file.sql
# specified database, table or query
mysqldump -hlocalhost -uroot -p -q --databases database_name --tables table_name --where="id < 10000" --no-tablespaces --no-create-info --single-transaction --skip-tz-utc > /path/to/backup/file.sql
```

> `--no-create-info`: 不带表构造信息，纯数据
>
> `--single-transaction`: 防止执行时锁死整个库
> 
> `--skip-tz-utc`: 阻止 MySQL 的时区转换，避免 `TIMESTAMP` 字段时间回退 8 小时

or 

```shell
mysqldump -hlocalhost -uroot -p -q database_name | gzip > /path/to/backup/file.sql.gz
```

### restore database

```shell
mysql -hlocalhost -uroot -p database_name < /path/to/backup/file.sql
```

or

```shell
cat /path/to/backup/file.sql | mysql -hlocalhost -uroot -p database_name
```

or gzip

```shell
gunzip < /path/to/backup/file.sql.gz | mysql -hlocalhost -uroot -p database_name
```

### create database

```sql
CREATE DATABASE IF NOT EXISTS `database_name` CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci
```

参考：[http://dev.mysql.com/doc/refman/5.7/en/sql-syntax.html](http://dev.mysql.com/doc/refman/5.7/en/sql-syntax.html)

### create user

```sql
CREATE USER 'user_name'@'host' IDENTIFIED BY 'your_password'
```

or

```sql
USE mysql;
INSERT INTO `user` (Host, User, Password) values ('your_host', 'user_name', password('your_password'));
```

### check privileges of user

```sql
SHOW GRANTS FOR 'user_name'@'host';
```

### grant privileges

```sql
GRANT privilege_name ON database_name.table_name TO 'user_name'@'host';
FLUSH PRIVILEGES;
```

例如: 

```sql
-- 授予用户 test 数据库 db_test 所有表权限
GRANT ALL ON `db_test`.* TO 'test'@'localhost';
-- 授予用户 test 数据库 db_test 的 post 表的 SELECT, INSERT, UPDATE 权限
GRANT SELECT, INSERT, UPDATE ON `db_test`.`post` TO 'test'@'localhost';

FLUSH PRIVILEGES;
```

参考：[https://dev.mysql.com/doc/refman/5.7/en/grant.html](https://dev.mysql.com/doc/refman/5.7/en/grant.html)

### remove privileges of user

```sql
REVOKE privilege_name ON `database_name`.`table_name` FROM 'username'@'host';
```

### modify password

```sql
SET PASSWORD = password('new_password_here');
```

or 修改其它用户密码

```sql
SET PASSWORD FOR 'user_name'@'host' = password('new_password_here');
```

### reset password of root user

1. `vi /etc/my.cnf`，在 `[mysqld]` 段加入 `skip-grant-tables`。

2. 重启 mysql `service mysql restart`。

3. 直接使用 `mysql` 进入 mysql，然后执行：

	```sql
	USE mysql;
	UPDATE `user` SET `Password` = password('your_new_password_here') WHERE `User` = 'root';
	FLUSH PRIVILEGES;
	```

4. 退出 mysql，把第一步加入的东西去掉或注释掉，重启 mysql。

5. 完成。

### 导出 XML 格式数据库

```shell
mysqldump --xml -t -uroot -p database_name > /path/to/data.xml

# with table structure info

mysqldump --xml -uroot -p database_name > /path/to/data.xml
```
