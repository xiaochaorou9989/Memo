# mysql cli tips

### login

```
mysql -hlocalhost -uroot -p
```

> -h: server address

> -u: user name

> -p: password，可直接在参数后跟上密码(`-ppassword`)也可不跟然后按照提示输入密码。

### backup database

```
mysqldump -hlocalhost -uroot -p database_name > path.to.backup.file.sql
```

or 

```
mysqldump -hlocalhost -uroot -p database_name | gzip > path.to.backup.file.sql.gz
```

### restore database

```
mysql -hlocalhost -uroot -p database_name < path.to.backup.file.sql
```

or

```
cat path.to.backup.file.sql | mysql -hlocalhost -uroot -p database_name
```

or gzip

```
gunzip < path.to.backup.file.sql.gz | mysql -hlocalhost -uroot -p database_name
```

### create database

```sql
create database if not exists database_name CHARACTER SET utf8 COLLATE utf8_general_ci
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

for example: 

授予用户 test 数据库 db_test 所有表权限

```sql
GRANT ALL ON db_test.* TO 'test'@'localhost';
FLUSH PRIVILEGES;
```

### remove privileges of user

```sql
REVOKE privilege_name ON database_name.table_name FROM 'username'@'host';
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