# Hive日志存放路径

- **Hive中的日志分为两种**

	1. 系统日志，记录了hive的运行情况，错误状况
	2. Job日志，记录了Hive中job的执行的历史过程

在`hive/conf/hive-log4j.properties`文件中记录了Hive日志的存储情况

	[hadoop@hadoop-01 conf]$ pwd
	/home/hadoop/app/hive-1.1.0-cdh5.7.0/conf
	[hadoop@hadoop-01 conf]$ ll
	total 28
	-rw-r--r--. 1 hadoop hadoop 1196 Mar 24  2016 beeline-log4j.properties.template
	-rw-r--r--. 1 hadoop hadoop 2422 Dec 28 11:26 hive-env.sh
	-rw-r--r--. 1 hadoop hadoop 2378 Mar 24  2016 hive-env.sh.template
	-rw-r--r--. 1 hadoop hadoop 2662 Mar 24  2016 hive-exec-log4j.properties.template
	-rw-r--r--. 1 hadoop hadoop 3505 Mar 24  2016 hive-log4j.properties.template
	-rw-rw-r--. 1 hadoop hadoop  709 Jan  3 05:20 hive-site.xml
	[hadoop@hadoop-01 conf]$ cp hive-log4j.properties.template hive-log4j.properties
	[hadoop@hadoop-01 conf]$ cat hive-log4j.properties
	# 其中有一段
	hive.log.threshold=ALL
	hive.root.logger=WARN,DRFA
	hive.log.dir=${java.io.tmpdir}/${user.name}
	hive.log.file=hive.log
默认是在`/tmp/${user.name}`

修改路径

	[hadoop@hadoop-01 conf]$ vi hive-log4j.properties
	# 将hive.log.dir=${java.io.tmpdir}/${user.name} 修改为自己需要存放的路径即可
	hive.log.dir=/home/hadoop/app/hive-1.1.0-cdh5.7.0/log
	
启动hive，查看log目录

	[hadoop@hadoop-01 log]$ pwd
	/home/hadoop/app/hive-1.1.0-cdh5.7.0/log
	[hadoop@hadoop-01 log]$ ll
	total 4
	-rw-rw-r--. 1 hadoop hadoop 1067 Jan  3 05:20 hive.log

修改log文件名只需将`hive-log4j.properties`文件中的`hive.log.file=hive.log`改为自己需要的名称即可
	

	

	
	
