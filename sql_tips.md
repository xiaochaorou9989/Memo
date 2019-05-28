# SQL Tips - Based on MySQL

### 修改表编码字符集，同时更改表内字段的

```sql
ALTER TABLE `tablename`
  CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

### 清除表字段中的空白字符（空格、换行、Tab）

```sql
UPDATE `tablename`  
SET `name` = REPLACE(REPLACE(REPLACE(REPLACE(`name`, CHAR(32),''), CHAR(9), ''), CHAR(10), ''), CHAR(13), '');
```

PS: 两端清除空格可以使用 TRIM 函数。

### 提升 MySQL 连接速度

可以在 `[mysqld]` 段加入 `skip-name-resolve`。

注：此参数会阻止通过主机名的连接，连接配置主机时必须使用 IP 值。例如：`root@localhost` 的连接会被阻止，需要使用 `root@127.0.0.1` 方式。

### MySQL 插入操作跳过(ignore)、覆盖(replace into)、更新(on duplicate key)

场景：有唯一索引，保证数据入库不抛异常且回滚

### 利用 GROUP_CONCAT 和 GROUP BY 实现分组字段拼接

```sql
SELECT GROUP_CONCAT(`name` SEPARATOR ',')
FROM `user`
GROUP BY `age`
LIMIT 100;
```

场景：同类型的数据先存起来，需要在某个时间进行分析

### SHOW ENGINE InnoDB STATUS

场景：查看死锁

### 索引类型

* PRIMARY：

主键索引。索引列唯一且不能为空,一张表只能有一个主键索引（主键索引通常在建表的时候就指定）

* NORMAL：

普通索引。索引列没有任何限制

* UNIQUE：

唯一索引。索引列的值必须是唯一的，但允许有空

* FULLTEXT：

全文搜索的索引。`FULLTEXT` 用于搜索很长一篇文章的时候，效果最好

* HASH：

用于对等比较，如 `=` 和 `<=>`

* BTREE：

以树形结构存储，通常用在像 `=`, `>`, `>=`, `<`, `<=`, `BETWEEN`, `Like` 等操作符查询效率较高

缺点：当对表中的数据进行增加、删除、修改时，索引也需要动态的维护，降低了数据的维护速度

注：MyISAM 和 InnoDB 存储引擎：只支持 BTREE 索引，也就是说默认使用 BTREE，不能够更换。**MEMORY/HEAP** 存储引擎：支持 HASH 和 BTREE 索引
