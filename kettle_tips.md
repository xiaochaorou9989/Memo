# Pentaho Data Integration(ETL) - Kettle 使用杂项

### 命令行运行示例(ON WINDOWS)

```bat
@echo off
E:
cd E:\tools\pdi-ce-7.1.0.0-12\data-integration
set KETTLE_PATH=E:\tools\pdi-ce-7.1.0.0-12\data-integration
set JOB_PATH=E:\tools\pdi-ce-7.1.0.0-12\data-integration\trans_collec\original

CALL %KETTLE_PATH%\Kitchen.bat -file=%JOB_PATH%\job.kjb -logfile=E:\Kettle\original_sync.log -level=Error
```

### 报错

* Timestamp : Unable to get timestamp from resultset at index xx

该数据库连接属性 - 选项 中添加 `zeroDateTimeBehavior = convertToNull`

* java.lang.StackOverflowError 异常

设置堆栈大小 Xss。修改 spoon.bat 以下行：

```
if "%PENTAHO_DI_JAVA_OPTIONS%"=="" set PENTAHO_DI_JAVA_OPTIONS="-Xms1024m" "-Xmx2048m" "-Xss512m" "-XX:MaxPermSize=256m"
```

* 中文乱码问题

尝试在数据库连接属性 - 选项 中添加 `characterEncoding = utf8`。如果无效，再尝试在 spoon.bat 中添加参数 `-Dfile.encoding=UTF-8`，具体位置如下：

```
set OPT=%OPT% %PENTAHO_DI_JAVA_OPTIONS% "-Dhttps.protocols=TLSv1,TLSv1.1,TLSv1.2" "-Djava.library.path=%LIBSPATH%" "-DKETTLE_HOME=%KETTLE_HOME%" "-DKETTLE_REPOSITORY=%KETTLE_REPOSITORY%" "-DKETTLE_USER=%KETTLE_USER%" "-DKETTLE_PASSWORD=%KETTLE_PASSWORD%" "-DKETTLE_PLUGIN_PACKAGES=%KETTLE_PLUGIN_PACKAGES%" "-DKETTLE_LOG_SIZE_LIMIT=%KETTLE_LOG_SIZE_LIMIT%" "-DKETTLE_JNDI_ROOT=%KETTLE_JNDI_ROOT%" "-Dfile.encoding=UTF-8"
```

* 执行一段时间后连接断开

数据库连接属性 - 选项 中添加 `autoReconnect = true`。同时 MySQL 的 net_read_timeout 和 net_write_timeout 尽量设置宽裕些。