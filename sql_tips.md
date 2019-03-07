* mysql插入操作跳过(ignore)、覆盖(replace into)、更新(on duplicate key)

> 场景：有唯一索引，保证数据入库不抛异常且回滚

* 利用GROUP_CONCAT和GROUP BY实现分组字段拼接

> 场景：同类型的数据先存起来，需要在某个时间进行分析

* show engine innodb status

> 场景：查看死锁

* 索引

> PRIMARY：主键索引。索引列唯一且不能为空,一张表只能有一个主键索引（主键索引通常在建表的时候就指定）

> NORMAL：普通索引。索引列没有任何限制

> UNIQUE：唯一索引。索引列的值必须是唯一的，但允许有空

> FULLTEXT：全文搜索的索引。FULLTEXT 用于搜索很长一篇文章的时候，效果最好

> HASH：用于对等比较，如"="和" <=>"

> BTREE：以树形结构存储，通常用在像 "=，>，>=，<，<=、BETWEEN、Like"等操作符查询效率较高

> 缺点：当对表中的数据进行增加、删除、修改时，索引也需要动态的维护，降低了数据的维护速度

> MyISAM和InnoDB存储引擎：只支持BTREE索引，也就是说默认使用BTREE，不能够更换

> MEMORY/HEAP存储引擎：支持HASH和BTREE索引