<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html><head><title>hive.html</title>
<meta http-equiv="Content-Style-Type" content="text/css">
<style type="text/css"><!--
body {
  margin: 15px 18px 15px 18px;
  background-color: #ffffff;
}
/* ========== Text Styles ========== */
hr { color: #000000}
body, table, span.rvts0 /* Normal text */
{
 font-size: 10pt;
 font-family: '宋体';
 font-style: normal;
 font-weight: normal;
 color: #000000;
 text-decoration: none;
}
a.rvts1, span.rvts1 /* Hyperlink */
{
 color: #0000ff;
 text-decoration: underline;
}
span.rvts2
{
 font-size: 12pt;
 font-family: '等线';
 font-weight: bold;
}
a.rvts3, span.rvts3
{
 font-size: 12pt;
 font-family: '等线';
 font-weight: bold;
 color: #0000ff;
 text-decoration: underline;
}
span.rvts4
{
 font-size: 12pt;
 font-family: '等线';
 font-weight: bold;
 color: #008000;
}
a.rvts5, span.rvts5
{
 font-size: 12pt;
 font-family: '等线';
 font-weight: bold;
 color: #0000ff;
 text-decoration: underline;
}
a.rvts5:hover { color: #ff0000; }
span.rvts6
{
 font-size: 12pt;
 font-family: '等线';
 font-weight: bold;
 color: #ff0000;
}
/* ========== Para Styles ========== */
p,ul,ol /* Paragraph Style */
{
 text-align: left;
 text-indent: 0px;
 padding: 0px 0px 0px 0px;
 margin: 0px 0px 0px 0px;
}
.rvps1 /* Centered */
{
 text-align: center;
}
.rvps2
{
 line-height: 1.50;
}
--></style>
</head>
<body>

<span class=rvts2>1&#12289;&#20851;&#38381;&#38450;&#28779;&#22681;</span>

<span class=rvts4>systemctl stop firewalld</span>

<span class=rvts2>
</span>

<span class=rvts2>2&#12289;&#25171;&#24320;mysql</span>

<span class=rvts4>docker ps -a</span>

<span class=rvts4>docker start 6050363c1064</span>

<span class=rvts4>docker ps</span>

<span class=rvts2>
</span>

<span class=rvts2>3&#12289;&#23433;&#35013;hive</span>

<span class=rvts3>http://mirrors.hust.edu.cn/apache/hive/</span>

<span class=rvts2>[root@master ~]# mkdir /opt/SoftWare/Hive</span>

<span class=rvts2>[root@master ~]# cd /opt/SoftWare/Hive</span>

<span class=rvts2>[root@master Hive]# tar -zxvf apache-hive-2.3.4-bin.tar.gz</span>

<span class=rvts2>[root@master Hive]# mv apache-hive-2.3.4-bin hive-2.3.4</span>

<span class=rvts2>[root@master Hive]# vi /etc/profile</span>

<span class=rvts2>export HIVE_HOME=/opt/SoftWare/Hive/hive-2.3.4</span>

<span class=rvts2>export PATH=$PATH:$HIVE_HOME/bin</span>

<span class=rvts2>[root@master Hive]# source /etc/profile</span>

<span class=rvts2>[root@master Hive]# cd hive-2.3.4/</span>

<span class=rvts2>[root@master hive-2.3.4]# hadoop fs -mkdir -p /hive/tmp</span>

<span class=rvts2>[root@master hive-2.3.4]# hadoop fs -mkdir -p /hive/logs</span>

<span class=rvts2>[root@master hive-2.3.4]# hadoop fs -mkdir -p /hive/warehouse</span>

<span class=rvts2>[root@master hive-2.3.4]# cp conf/hive-default.xml.template conf/hive-site.xml</span>

<span class=rvts2>[root@master hive-2.3.4]# vi conf/hive-site.xml </span>

<span class=rvts2>&lt;property&gt;&lt;!--&#25968;&#25454;&#24211;&#36830;&#25509;&#22320;&#22336;&#65292;&#20351;&#29992;MySQL&#23384;&#20648;&#20803;&#25968;&#25454;&#20449;&#24687;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;javax.jdo.option.ConnectionURL&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;jdbc:mysql://master:3306/metastore?createDatabaseIfNotExist=true&amp;amp;useSSL=false&lt;/value&gt;</span>

<span class=rvts2>&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--&#25968;&#25454;&#24211;&#39537;&#21160;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;javax.jdo.option.ConnectionDriverName&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;com.mysql.jdbc.Driver&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--&#25968;&#25454;&#24211;&#29992;&#25143;&#21517;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;javax.jdo.option.ConnectionUserName&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;root&lt;/value&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;description&gt;Username to use against metastore database&lt;/description&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&lt;property&gt;&lt;!--&#23494;&#30721;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;javax.jdo.option.ConnectionPassword&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;123456&lt;/value&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;description&gt;password to use against metastore database&lt;/description&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--HDFS&#36335;&#24452;&#65292;&#29992;&#20110;&#23384;&#20648;&#19981;&#21516; map/reduce &#38454;&#27573;&#30340;&#25191;&#34892;&#35745;&#21010;&#21644;&#36825;&#20123;&#38454;&#27573;&#30340;&#20013;&#38388;&#36755;&#20986;&#32467;&#26524;&#12290;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.exec.local.scratchdir&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;/hive/tmp&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--Hive &#26597;&#35810;&#26085;&#24535;&#25152;&#22312;&#30340;&#30446;&#24405;&#65292;&#22914;&#26524;&#35813;&#20540;&#20026;&#31354;&#65292;&#23558;&#19981;&#21019;&#24314;&#26597;&#35810;&#26085;&#24535;&#12290;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.querylog.location&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;/hive/logs&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--&#26412;&#22320;&#34920;&#30340;&#40664;&#35748;&#20301;&#32622;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.metastore.warehouse.dir&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;/hive/warehouse&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.metastore.uris&lt;/name&gt;&lt;!--Hive&#36830;&#25509;&#21040;&#35813;URI&#35831;&#27714;&#36828;&#31243;&#20803;&#23384;&#20648;&#30340;&#20803;&#25968;&#25454;--&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;thrift://master:9083&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;&lt;!--&#20851;&#38381;&#26412;&#22320;&#27169;&#24335;&#65292;&#27492;&#39033;&#19981;&#23384;&#22312;&#65292;&#38656;&#35201;&#33258;&#24049;&#28155;&#21152;--&gt;</span>

<span class=rvts2>&lt;name&gt;hive.metastore.local&lt;/name&gt;</span>

<span class=rvts2>&lt;value&gt;false&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&nbsp;&lt;property&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.server2.logging.operation.log.location&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;/hive/logs&lt;/value&gt;</span>

<span class=rvts2>&nbsp;&lt;/property&gt;</span>

<span class=rvts2>&lt;property&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;name&gt;hive.downloaded.resources.dir&lt;/name&gt;</span>

<span class=rvts2>&nbsp; &nbsp;&lt;value&gt;/hive/tmp/${hive.session.id}_resources&lt;/value&gt;</span>

<span class=rvts2>&lt;/property&gt;</span>

<span class=rvts2>[root@master hive-2.3.4]# vi bin/hive-config.sh</span>

<span class=rvts2>export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.222.b10-0.el7_6.x86_64/</span>

<span class=rvts2>export HADOOP_HOME=/opt/SoftWare/Hadoop/hadoop-2.7.7</span>

<span class=rvts2>export HIVE_HOME=/opt/SoftWare/Hive/hive-2.3.4</span>

<span class=rvts2># &#22797;&#21046;MySQL&#30340;&#39537;&#21160;jar&#21040;hive/lib&#30446;&#24405;</span>

<span class=rvts2>[root@master hive-2.3.4]# cd lib</span>

<span class=rvts2>[root@master lib]# cd $HIVE_HOME/bin</span>

<span class=rvts2>[root@master bin]# schematool -initSchema -dbType mysql</span>

<span class=rvts2>schemaTool completed</span>

<span class=rvts4>[root@master bin]# hive --service metastore</span>

<span class=rvts4>[root@master bin]# hive</span>

<span class=rvts2>hive&gt; show tables;</span>

<span class=rvts2>OK</span>

<span class=rvts2>
</span>

<span class=rvts2>4&#12289;&#32593;&#32476;&#26085;&#24535;&#30340;&#31616;&#21333;&#26597;&#35810;&#12304;&#27880;&#24847;&#26085;&#24535;&#20999;&#21106;&#12305;</span>

<span class=rvts2>/opt/SoftWare/Test/weblog.data</span>

<span class=rvts2>1c13c719fbfd87f49522f189413c6ba /gybfokxyojgtwrq.html 2012-05-10 21:17:32 169.65.197.63
</span>

<span class=rvts2>e42fe5714cb4402f81e5dce8da1755 /hpipbodlxgt.html 2012-05-10 21:12:04 166.22.84.135</span>

<span class=rvts2>/opt/SoftWare/Test/ip_to_country.txt </span>

<span class=rvts2>169.65.197.63 UnitedStates
1</span>

<span class=rvts2>166.22.84.135 UnitedKingdom</span>

<span class=rvts2># &#24314;&#34920;</span>

<span class=rvts2>create external table weblog_entries</span>

<span class=rvts2>( md5 string,</span>

<span class=rvts2>url string,</span>

<span class=rvts2>request_date string,</span>

<span class=rvts2>request_time string,</span>

<span class=rvts2>ip string</span>

<span class=rvts2>)</span>

<span class=rvts2>row format delimited fields terminated by '\t'</span>

<span class=rvts2>lines terminated by '\n'</span>

<span class=rvts2>location '/data/hive/weblog';</span>

<span class=rvts2># &#24314;&#34920;</span>

<span class=rvts2>create table ip_to_country(</span>

<span class=rvts2>ip string,</span>

<span class=rvts2>country string</span>

<span class=rvts2>)</span>

<span class=rvts2>row format delimited fields terminated by '\t'</span>

<span class=rvts2>lines terminated by '\n'</span>

<span class=rvts2>location '/data/hive/ip_to_country';</span>

<span class=rvts2># &#21152;&#36733;&#25968;&#25454;</span>

<span class=rvts2>hive&gt; load data local inpath '/opt/SoftWare/Test/weblog.data' into table weblog_entries;</span>

<span class=rvts2>hive&gt; load data local inpath '/opt/SoftWare/Test/ip_to_country.txt' into table ip_to_country;</span>

<span class=rvts2># &#36830;&#25509;&#26597;&#35810;</span>

<span class=rvts2>hive&gt; select wle.*, itc.country FROM weblog_entries wle join ip_to_country itc on wle.ip = itc.ip;</span>

<span class=rvts2>hive&gt; create table weblog_entries_with_url_length as select url, request_date, request_time, length(url) as url_length from weblog_entries;</span>

<span class=rvts2>
</span>

</body></html>