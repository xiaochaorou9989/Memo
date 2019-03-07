<pre>

String增删查
SET runoobkey redis
DEL runoobkey
GET runoobkey

---

哈希Hash
HMSET runoobkey name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
HGETALL runoobkey

---

列表List
LPUSH runoobkey1 redis
LPUSH runoobkey2 mongodb
LPUSH runoobkey3 mysql
LRANGE runoobkey1 0 10 //取出

---

list集合的值不具有唯一性
set集合的值具有唯一性

---

集合Set
SADD runoobkey redis
SADD runoobkey mongodb
SADD runoobkey mysql
SADD runoobkey mysql
SMEMBERS runoobkey //取出

---

有序集合(sorted set)
ZADD runoobkey 1 redis
ZADD runoobkey 2 mongodb
ZADD runoobkey 3 mysql
ZADD runoobkey 3 mysql
ZADD runoobkey 4 mysql
ZRANGE runoobkey 0 10 WITHSCORES //取出

---

HyperLogLog算法是一种非常巧妙的近似统计海量去重元素数量的算法。
它内部维护了 16384 个桶（bucket）来记录各自桶的元素数量。
当一个元素到来时，它会散列到其中一个桶，以一定的概率影响这个桶的计数值。
因为是概率算法，所以单个桶的计数值并不准确，但是将所有的桶计数值进行调合均值累加起来，结果就会非常接近真实的计数值。

---

HyperLogLog
PFADD runoobkey "redis"
PFADD runoobkey "redis"
PFADD runoobkey "mongodb"
PFADD runoobkey "mongodb"
PFADD runoobkey "mysql"
PFCOUNT runoobkey //取出

---

发布订阅
SUBSCRIBE redisChat //订阅者
PUBLISH redisChat "Redis is a great caching technique"
PUBLISH redisChat "Learn redis by runoob.com"
#订阅者的客户端会显示如下消息
1) "message"
2) "redisChat"
3) "Redis is a great caching technique"
1) "message"
2) "redisChat"
3) "Learn redis by runoob.com"

---

Redis 事务可以一次执行多个命令， 并且带有以下两个重要的保证：

批量操作在发送 EXEC 命令前被放入队列缓存。
收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。
一个事务从开始到执行会经历以下三个阶段：

开始事务。
命令入队。
执行事务。

它先以 MULTI 开始一个事务， 然后将多个命令入队到事务中， 最后由 EXEC 命令触发事务， 一并执行事务中的所有命令：

事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

---

[摘自（练习了一遍）](./http://www.runoob.com/redis/redis-transactions.html)


