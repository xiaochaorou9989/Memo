# ElasticSearch Tips

### 查询设置分词间相隔最高间隔

通过设置 `slop` 为 0 实现类似 MySQL 中的 `LIKE` 查询 (LIKE '%keyword%')。

```JSON
{
  "query": {
    "match_phrase": {
      "market": {
        "query": "科技",
        "slop": 0
      }
    }
  }
}
```

### 使用 Logstash 同步 MySQL 数据

需要借助 **logstash-input-jdbc** 插件。本示例使用的 Logstash 7.2 版本本身自带，没有的需要自行安装一下。

```
# file: mysql_sync.conf
input {
  jdbc {
    # 配置 JDBC 的路径，MySQL 版本不同会需要不同版本的 JDBC
    jdbc_driver_library => "/path/to/mysql-connector-java-5.1.46.jar"
    jdbc_driver_class => "com.mysql.jdbc.Driver"
    # MySQL 连接
    jdbc_connection_string => "jdbc:mysql://host:3306/database_name?serverTimezone=GMT%2B8&zeroDateTimeBehavior=convertToNull"
    jdbc_user => "database_user"
    jdbc_password => "secret"
    jdbc_paging_enabled => true
    jdbc_page_size => 50000
    jdbc_fetch_size => 10000
    connection_retry_attempts => 3
    connection_retry_attempts_wait_time => 2
    jdbc_pool_timeout => 5
    lowercase_column_names => true
    # 是否记录最后运行相关信息，用于增量同步
    record_last_run => true
    # 最后运行信息记录保存路径，不设置则默认保存在用户根目录下
	last_run_metadata_path => "/path/to/.my_last_run"
    # 定时规则，同 cron 规则
    schedule => "* * * * *"
    use_column_value => true
    # 跟踪字段，设置了 record_last_run 的话就会记录下该字段值
    tracking_column => "id"
    # 读取要同步的数据记录，变量 sql_last_value 会取 record_last_run 记录的值
    statement => "SELECT * FROM table_name WHERE id > :sql_last_value"
  }
}

output {
  elasticsearch {
    # ES 服务地址
    hosts => ["localhost:9200"]
    # ES 索引名称
    index => "index_name"
    # 文档 id 使用 MySQL 表中的 id
    document_id => "%{id}"
  }
}

# 运行
/path/to/bin/logstash -f /path/to/mysql_sync.conf
```

**PS**: 实际使用中，上边 mysql_sync.conf 中 # 注释部分要全部删除，因为这并不是 Logstash 配置文件的标准注释