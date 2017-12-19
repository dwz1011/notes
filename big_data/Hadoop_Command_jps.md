# Hadoop常用命令之jps


	[hadoop@hadoop-01 ~]$ jps
	3072 NameNode
	3395 SecondaryNameNode
	4422 NodeManager
	4316 ResourceManager
	9420 Jps
	3183 DataNode
	[hadoop@hadoop-01 ~]$ 
	[hadoop@hadoop-01 ~]$ cd /tmp/hsperfdata_hadoop/
	[hadoop@hadoop-01 hsperfdata_hadoop]$ ll
	total 160
	-rw-------. 1 hadoop hadoop 32768 Dec 19 14:56 3072
	-rw-------. 1 hadoop hadoop 32768 Dec 19 14:56 3183
	-rw-------. 1 hadoop hadoop 32768 Dec 19 14:56 3395
	-rw-------. 1 hadoop hadoop 32768 Dec 19 14:56 4316
	-rw-------. 1 hadoop hadoop 32768 Dec 19 14:56 4422
	[hadoop@hadoop-01 hsperfdata_hadoop]$ 
	
	
	
正常流程
	
	[root@hadoop-01 ~]# jps
	3072 -- process information unavailable
	3395 -- process information unavailable
	4422 -- process information unavailable
	4316 -- process information unavailable
	9517 Jps
	3183 -- process information unavailable
	[root@hadoop-01 ~]# 
	找到该进程是哪个用户使用的
	[root@hadoop-01 ~]# ps -ef|grep 3072
	hadoop    3072     1  0 11:13 ?        00:00:43 /usr/java/jdk1.8.0_45/bin/java -Dproc_namenode -Xmx1000m -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/opt/software/hadoop-2.8.1/logs -Dhadoop.log.file=hadoop.log -Dhadoop.home.dir=/opt/software/hadoop-2.8.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,console -Djava.library.path=/opt/software/hadoop-2.8.1/lib/native -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Stack=true -Dhadoop.log.dir=/opt/software/hadoop-2.8.1/logs -Dhadoop.log.file=hadoop-hadoop-namenode-hadoop-01.log -Dhadoop.home.dir=/opt/software/hadoop-2.8.1 -Dhadoop.id.str=hadoop -Dhadoop.root.logger=INFO,RFA -Djava.library.path=/opt/software/hadoop-2.8.1/lib/native -Dhadoop.policy.file=hadoop-policy.xml -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS org.apache.hadoop.hdfs.server.namenode.NameNode
	root      9528  9495  0 15:08 pts/0    00:00:00 grep 3072
	切换到用户
	[root@hadoop-01 ~]# su - hadoop
	[hadoop@hadoop-01 ~]$ jps
	3072 NameNode
	3395 SecondaryNameNode
	4422 NodeManager
	9560 Jps
	4316 ResourceManager
	3183 DataNode
	[hadoop@hadoop-01 ~]$ 
	
异常流程

	[root@hadoop-01 ~]# jps
	3072 -- process information unavailable
	3395 -- process information unavailable
	4422 -- process information unavailable
	4316 -- process information unavailable
	9517 Jps
	3183 -- process information unavailable
	[root@hadoop-01 ~]# 
	若ps -ef|grep pid，找不到用户，则
	[root@hadoop-01 ~]# kill -9 pid
	去/tmp/hsperfdata_hadoop文件夹删除该pid文件
	
	
	
