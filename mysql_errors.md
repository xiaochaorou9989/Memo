# MySQL Error Logs

**数据库的所有操作之前都请首先进行备份处理，避免造成二次损害**

### MySQL 启动失败（1067），错误日志：Error: could not open single-table tablespace file ./mysql/innodb_index_stats.ibd

Fix: 

1. 去掉 MySQL 配置文件中 `# innodb_force_recovery = 2` 项的注释，如果没有则添加此项。

2. 启动 MySQL，此时 MySQL 会尝试自己修复错误。

3. 修复后，将第一步的配置项注释掉。

4. 重启 MySQL。

参考：[stackoverflow.com](https://stackoverflow.com/questions/18575755/xampp-mysql-could-not-open-single-table-tablespace-file-mysql-innodb-index-st)
