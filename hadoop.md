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

----


## centos7安装Hadoop+hbase+hive步骤 https://my.oschina.net/lwenhao/blog/3025661

	```
	http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
	http://mirror.bit.edu.cn/apache/hbase/2.1.5/hbase-2.1.5-bin.tar.gz
	https://www-eu.apache.org/dist/hive/hive-3.1.1/apache-hive-3.1.1-bin.tar.gz
	```

一. IP、DNS、主机名

	linux 静态IP、DNS、主机名配置

二. Hadoop

	1. IP分配

		```
		192.168.3.23 Namenode 主节点 hadoop1
		192.168.3.38 Namenode 备份节点 hadoop2
		192.168.3.39 Datanode1 hadoop3
		192.168.3.40 Datanode2 hadoop4
		```

	2. 安装jdk8（四台）（ova自带）
	
		yum list java*
		yum install -y java-1.8.0-openjdk-devel.x86_64
		默认jre jdk 安装路径是/usr/lib/jvm下面

	3. 配置jdk环境变量（四台）
	
		vim /etc/profile

		```
		export JAVA_HOME=/home/java/jdk1.8.0_191
		export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/jre/lib/rt.jar
		export PATH=$PATH:$JAVA_HOME/bin
		```

		使得配置生效
		. /etc/profile
		
		查看变量
		echo $JAVA_HOME
		
		输出 /usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.161-0.b14.el7_4.x86_64

	4. 安装Hadoop
	
		创建文件夹（四台） mkdir /lp mkdir /lp/hadoop
		复制hadoop安装包到/tmp（以下开始，操作只在主节点）
		解压：
		tar -xzvf /tmp/hadoop-3.1.2.tar.gz
		mv hadoop-3.1.2/ /lp/hadoop/
		
		etc/hadoop/hadoop-env.sh 添加如下内容
		
		```
		export JAVA_HOME=/home/java/jdk1.8.0_191/
		export HDFS_NAMENODE_USER="root"
		export HDFS_DATANODE_USER="root"
		export HDFS_SECONDARYNAMENODE_USER="root"
		export YARN_RESOURCEMANAGER_USER="root"
		export YARN_NODEMANAGER_USER="root"
		```
		
		修改etc/hadoop/core-site.xml，把配置改成： 
		
		```
		<configuration>
			<property>
				<name>fs.defaultFS</name>
				<value>hdfs://192.168.3.23:9001</value>
			</property>
			<property>
				<name>io.file.buffer.size</name>
				<value>131072</value>
			</property>
		</configuration>
		```

		修改etc/hadoop/hdfs-site.xml，把配置改成：

		```
		<configuration>
			<!-- Configurations for NameNode: -->
			<property>
				<name>dfs.namenode.name.dir</name>
				<value>/lp/hadoop/hdfs/name/</value>
			</property>

			<property>
				<name>dfs.blocksize</name>
				<value>268435456</value>
			</property>

			<property>
				<name>dfs.namenode.handler.count  </name>
				<value>100</value>
			</property>

			<property>
				<name>dfs.namenode.http-address</name>
				<value>192.168.3.23:8305</value>
			</property>

			<property>
				<name>dfs.namenode.secondary.http-address</name>
				<value>192.168.3.38:8310</value>
			</property>

			<!-- Configurations for DataNode: -->
			<property>
				<name>dfs.datanode.data.dir</name>
				<value>/lp/hadoop/hdfs/data/</value>
			</property>

			<property>
				<name>dfs.replication</name>
				<value>1</value>
			</property>
		</configuration>
		```

		etc/hadoop/yarn-site.xml，把配置改成：

		```
		<configuration>
		<!-- Site specific YARN configuration properties -->
			<property>
				<name>yarn.resourcemanager.hostname</name>
				<value>192.168.3.23</value>
			</property>
			<property>
				<name>yarn.resourcemanager.webapp.address</name>
				<value>192.168.3.23:8320</value>
			</property>
			<property>
				<name>yarn.nodemanager.aux-services</name>
				<value>mapreduce_shuffle</value>
			</property>
			<property>
				<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
				<value>org.apache.hadoop.mapred.ShuffleHandler</value>
			</property>
			<property>
				<name>yarn.log-aggregation-enable</name>
				<value>true</value>
			</property>
			<property>
				<name>yarn.log-aggregation.retain-seconds</name>
				<value>864000</value>
			</property>
			<property>
				<name>yarn.log-aggregation.retain-check-interval-seconds</name>
				<value>86400</value>
			</property>
			<property>
				<name>yarn.nodemanager.remote-app-log-dir</name>
				<value>/lp/hadoop/YarnApp/Logs</value>
			</property>
			<property>
				<name>yarn.log.server.url</name>
				<value>http://192.168.3.23:8325/jobhistory/logs/</value>
			</property>
			<property>
				<name>yarn.nodemanager.local-dirs</name>
				<value>/lp/hadoop/YarnApp/nodemanager</value>
			</property>
			<property>
				<name>yarn.scheduler.maximum-allocation-mb</name>
				<value>5000</value>
			</property>
			<property>
				<name>yarn.scheduler.minimum-allocation-mb</name>
				<value>1024</value>
			</property>
			<property>
				<name>yarn.nodemanager.vmem-pmem-ratio</name>
				<value>4.1</value>
			</property>
			<property>
				<name>yarn.nodemanager.vmem-check-enabled</name>
				<value>false</value>
			</property>
		</configuration>
		```

		etc/hadoop/mapred-site.xml，内容改为如下：

		```
		<configuration>
			 <property>
				<name>mapreduce.framework.name</name>
				<value>yarn</value>
			</property>
			<property>
				<name>yarn.app.mapreduce.am.staging-dir</name>
				<value>/lp/hadoop/YarnApp/tmp/hadoop-yarn/staging</value>
			</property>
			<!--MapReduce JobHistory Server地址-->
			<property>
				<name>mapreduce.jobhistory.address</name>
				<value>192.168.3.23:8330</value>
			</property>
			<!--MapReduce JobHistory Server Web UI地址-->
			<property>
				<name>mapreduce.jobhistory.webapp.address</name>
				<value>192.168.3.23:8331</value>
			</property>
			<!--MR JobHistory Server管理的日志的存放位置-->
			<property>
				<name>mapreduce.jobhistory.done-dir</name>
				<value>${yarn.app.mapreduce.am.staging-dir}/history/done</value>
			</property>
		<!--MapReduce作业产生的日志存放位置-->
			<property>
				<name>mapreduce.jobhistory.intermediate-done-dir</name>
				<value>${yarn.app.mapreduce.am.staging-dir}/history/done_intermediate</value>
			</property>
			<property>
				<name>mapreduce.jobhistory.joblist.cache.size</name>
				<value>1000</value>
			</property>

			<property>
				<name>mapreduce.tasktracker.map.tasks.maximum</name>
				<value>8</value>
			</property>
			<property>
				<name>mapreduce.tasktracker.reduce.tasks.maximum</name>
				<value>8</value>
			</property>
			<property>
				<name>mapreduce.jobtracker.maxtasks.perjob</name>
				<value>5</value>
			</property>
		</configuration>
		```

		修改etc/hadoop/workers
		vim etc/hadoop/workers

		```
		hadoop3
		hadoop4
		```

		压缩配置好的hadoop文件夹
		tar -czvf hadoop.tar.gz /lp/hadoop/hadoop-3.1.2/
		
		拷贝到其余节点：
		scp hadoop.tar.gz root@192.168.3.38:/
		
		解压删除：
		tar -xzvf hadoop.tar.gz
		rm –rf hadoop.tar.gz
		
	5. 配置Hadoop环境变量（四台）
	
		vim /etc/profile.d/hadoop-3.1.2.sh

		```
		export HADOOP_HOME="/lp/hadoop/hadoop-3.1.2"
		export PATH="$HADOOP_HOME/bin:$PATH"
		export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
		export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
		```

		source /etc/profile
		
		配置hosts（四台）
		vim /etc/hosts

		```
		192.168.3.23 hadoop1
		192.168.3.38 hadoop2
		192.168.3.39 hadoop3
		192.168.3.40 hadoop4
		```

		免密码登录自身（四台）

		ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
		cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
		chmod 0600 ~/.ssh/authorized_keys
	
		master免密码登录worker【单台，只需在namenode1上执行】
		ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop2
		ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop3
		ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop4
		
		格式化HDFS [只有首次部署才可使用]【谨慎操作，只在master上操作】
		/lp/hadoop/hadoop-3.1.2/bin/hdfs namenode -format myClusterName
		
		开启hadoop服务 【只在master上操作】
		/lp/hadoop/hadoop-3.1.2/sbin/start-dfs.sh
		/lp/hadoop/hadoop-3.1.2/sbin/start-yarn.sh
		
		web地址

		Hdfs页面：
		主：192.168.3.23:8305
		从：192.168.3.38:8310
		
		Yarn页面：
		192.168.3.23:8320


三. Hbase

	基于以上的hadoop配置好各个节点。并且使用hbase自带的Zookeeper

	分配
	
		```
		23 Namenode 主节点 hadoop1 
		39 Datanode1 hadoop3
		```

	解压对应的hbase

		tar -xzvf /tmp/hbase-2.1.2-bin.tar.gz
		mv hbase-2.1.2/ /lp/hadoop/
		
		修改/hbase-2.1.2/conf/hbase-site.xml

		```
		<property>
			<name>hbase.cluster.distributed</name>
			<value>true</value>
		</property>
		<property>
			<name>hbase.rootdir</name>
			<value>hdfs://192.168.3.23:9001/hbase</value>
		</property>
		<property>
			<name>hbase.zookeeper.quorum</name>
			<value>hadoop1,hadoop3</value>
		</property>
		<property>
			<name>hbase.master</name>
			<value>psyDebian:60000</value>
		</property>
		<property>
			<name>hbase.master.maxclockskew</name>
			<value>180000</value>
		</property>
		<property>
			<name>hbase.wal.provider</name>
			<value>filesystem</value>
		</property>
		<property>
			<name>hbase.unsafe.stream.capability.enforce</name>
			<value>false</value>
		</property>
		```

	修改/hbase-2.1.2/conf/hbase-env.sh

		```
		export JAVA_HOME=/home/java/jdk1.8.0_191/
		export HBASE_CLASSPATH=/lp/hadoop/hbase-2.1.2/conf
		export HBASE_MANAGES_ZK=true
		```

	修改/hbase-2.1.2/conf/regionservers

		```
		hadoop1
		hadoop3
		```


	把/lp/hadoop/hbase-2.1.2/lib/client-facing-thirdparty目录下的htrace-core-3.1.0-incubating.jar 复制到/lp/hadoop/hbase-2.1.2/lib
	cp /hbase-2.1.2/lib/client-facing-thirdparty/htrace-core-3.1.0-incubating.jar /hbase-2.1.2/lib
	
	压缩配置好的hbase-2.1.2文件夹
	tar -czvf hbase-2.1.2.tar.gz hadoop-3.1.2/
	
	拷贝到hadoop3节点：
	scp hbase-2.1.2.tar.gz root@hadoop3:/lp/hadoop
	
	解压删除
	tar -xzvf hbase-2.1.2.tar.gz
	rm –rf hbase-2.1.2.tar.gz
	
	启动
	./bin/start-hbase.sh
	
	进入shell
	./bin/hbase shell
	
	web页面访问
	192.168.3.23:16010


四. Hive

	基于以上配置把hive配置到hadoop1，mysql5.7安装到hadoop3

	分配
	
		```
		23 Namenode 主节点 hadoop1 
		39 Datanode1 hadoop3
		```

	解压对应的hive
	tar -xzvf /tmp/apache-hive-3.1.1-bin.tar.gz
	mv apache-hive-3.1.1-bin/ /lp/hadoop/
	
	配置hive 进入apache-hive-3.1.1-bin/conf/目录 复制hive-env.sh.template 为 hive-env.sh
	cp hive-env.sh.template hive-env.sh
	
	编辑hive-env.sh

		```
		export HADOOP_HOME=/lp/hadoop/hadoop-3.1.2
		export HIVE_CONF_DIR=/lp/hadoop/apache-hive-3.1.1-bin/conf
		export HIVE_AUX_JARS_PATH=/lp/hadoop/apache-hive-3.1.1-bin/lib
		```

	复制hive-default.xml.template 为 hive-site.xml
	cp hive-default.xml.template hive-site.xml
	
	修改hive-site.xml

		```
		<property>
			<name>javax.jdo.option.ConnectionURL</name>
			<value>jdbc:mysql://hadoop3:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=false</value>
		</property>

		<property>
			<name>javax.jdo.option.ConnectionDriverName</name>
			<value>com.mysql.jdbc.Driver</value>
		</property>

		<property>
			<name>javax.jdo.option.ConnectionUserName</name>
			<value>root</value>
			<description>Username to use against metastore database</description>
		</property>

		<property>
			<name>javax.jdo.option.ConnectionPassword</name>
			<value>123456</value>
			<description>password to use against metastore database</description>
		</property>
		```

	其他服务可以通过thrift接入hive，可以加上是否需要验证的配置，此处设为NONE，暂时不需要验证

		```
		<property>
			<name>hive.server2.authentication</name>
			<value>NONE</value>
		</property>
		```

	复制hive-exec-log4j2.properties.template 为 hive-exec-log4j2.properties
	cp hive-exec-log4j2.properties.template hive-exec-log4j2.properties
	
	复制hive-log4j2.properties.template为hive-log4j2.properties
	cp hive-log4j2.properties.template hive-log4j2.properties
	
	下载mysql驱动放入/home/hadoop/apache-hive-3.1.1-bin/lib包中

	在hadoop3安装mysql5.7版本，并且把root设置为任意主机访问或者hadoop1主机访问

	use mysql;
	select host,user from user;
	grant all privileges  on *.* to root@'%' identified by "123456";
	flush privileges;
	select host,user from user;
	
	初始化（第一次启动）
	./schematool -initSchema -dbType mysql
	
	启动
	./hive
	./hive --service hiveserver2
	
	启动hiveserver2使其他服务可以通过thrift接入hive

	Mysql数据库中会自动创建hive数据库

	测试

	beeline工具测试使用jdbc方式连接

	./beeline -u jdbc:hive2://localhost:10000
	端口号默认是10000

	hiveserver2会启动一个WEB，端口号默认为10002，可以通过 http://192.168.3.23:10002/