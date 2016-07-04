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

```
create database if not exists database_name CHARACTER SET utf8 COLLATE utf8_general_ci
```

参考：[http://dev.mysql.com/doc/refman/5.7/en/sql-syntax.html](http://dev.mysql.com/doc/refman/5.7/en/sql-syntax.html) 