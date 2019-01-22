* mysql插入操作跳过(ignore)、覆盖(replace into)、更新(on duplicate key)
> 场景：有唯一索引，保证数据入库不抛异常且回滚

* 利用GROUP_CONCAT和GROUP BY实现分组字段拼接
> 场景：同类型的数据先存起来，需要在某个时间进行分析
