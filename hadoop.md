1. 下载 链接: https://pan.baidu.com/s/1a6efdXAn22ap0hWOkIXGuw 提取码: k362 复制这段内容后打开百度网盘手机App，操作更方便哦
2. 使用hadooponwindows-master的bin和etc替换hadoop2.7.7的bin和etc 
3. 配置JAVA_HOME
4. 配置HADOOP_HOME E:\soft\hadoop-2.7.7
5. path添加%HADOOP_HOME%\bin
6. 编辑E:\soft\hadoop-2.7.7\etc\hadoop\hadoop-env.cmd 把set JAVA_HOME改为jdk的位置 
7. 编辑E:\soft\hadoop-2.7.7/etc/hadoop/hdfs-site.xml 修改路径为hadoop下的namenode和datanode /E:/soft/hadoop-2.7.7/data/namenode /E:/soft/hadoop-2.7.7/data/datanode
8. 在E:\soft\hadoop-2.7.7目录下添加tmp文件夹 
9. 在E:\soft\hadoop-2.7.7目录下添加data和namenode，datanode子文件夹 
10. 把hadoop.dll拷贝到 C:\Windows\System32
11. 输入hdfs namenode -format,看到seccessfully就说明format成功 
12. 转到Hadoop-2.7.3\sbin文件下 输入start-all，启动hadoop集群 出现下面四个窗口表示启动hadoop集群成功 
13. 输入jps - 可以查看运行的所有节点 访问http://localhost:50070,访问hadoop的web界面 访问http://localhost:8088,访问yarn的web界面，有就表明已经成功 