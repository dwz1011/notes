# Hive之WordCount

统计文件内单词个数
	
	[hadoop@hadoop-01 ~]$ cat /home/hadoop/data/wordcount.txt 
	hello world hello hive spark word world hive spark mysql

1. 在Hive中创建存放要处理的数据的表
		
		hive> create table word_data(line string);
		OK
		Time taken: 0.134 seconds
2. 导入数据

		hive> load data local inpath '/home/hadoop/data/wordcount.txt/' overwrite into table word_data;
		Loading data to table default.word_data
		Table default.word_data stats: [numFiles=1, numRows=0, totalSize=57, rawDataSize=0]
		OK
		Time taken: 0.337 seconds
		
3. 查看是否导入成功

		hive> select * from word_data;
		OK
		hello world hello hive spark word world hive spark mysql
		Time taken: 0.089 seconds, Fetched: 1 row(s)

### 用HIVE实现MapReduce的计算

1. 先创建一个words表

		hive> create table words(word string);
		OK
		Time taken: 0.076 seconds
		
2. 将word_data表中的line字段按照空格(目前这里只有一行数据)分割，插入words表中

		hive> insert overwrite table words select explode(split(line,' ')) word from word_data;
		Query ID = hadoop_20180103062727_8140aac4-1342-4550-ab59-a599dc7f4d96
		Total jobs = 3
		Launching Job 1 out of 3
		Number of reduce tasks is set to 0 since there's no reduce operator
		Starting Job = job_1514876544679_0007, Tracking URL = http://hadoop-01:8088/proxy/application_1514876544679_0007/
		Kill Command = /home/hadoop/software/hadoop/bin/hadoop job  -kill job_1514876544679_0007
		Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 0
		2018-01-03 06:51:06,275 Stage-1 map = 0%,  reduce = 0%
		2018-01-03 06:51:13,943 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.72 sec
		MapReduce Total cumulative CPU time: 1 seconds 720 msec
		Ended Job = job_1514876544679_0007
		Stage-4 is selected by condition resolver.
		Stage-3 is filtered out by condition resolver.
		Stage-5 is filtered out by condition resolver.
		Moving data to: hdfs://192.168.137.130:9000/user/hive/warehouse/words/.hive-staging_hive_2018-01-03_06-50-55_977_3127470679985590123-1/-ext-10000
		Loading data to table default.words
		Table default.words stats: [numFiles=1, numRows=10, totalSize=57, rawDataSize=47]
		MapReduce Jobs Launched: 
		Stage-Stage-1: Map: 1   Cumulative CPU: 1.72 sec   HDFS Read: 3337 HDFS Write: 126 SUCCESS
		Total MapReduce CPU Time Spent: 1 seconds 720 msec
		OK
		Time taken: 19.432 seconds
		
3. 查看是否插入成功

		hive> select * from words;
		OK
		hello
		world
		hello
		hive
		spark
		word
		world
		hive
		spark
		mysql
		Time taken: 0.107 seconds, Fetched: 10 row(s)

4. 计算

		hive> select word, count(*) from words group by word;
		Query ID = hadoop_20180103062727_8140aac4-1342-4550-ab59-a599dc7f4d96
		Total jobs = 1
		Launching Job 1 out of 1
		Number of reduce tasks not specified. Estimated from input data size: 1
		In order to change the average load for a reducer (in bytes):
		  set hive.exec.reducers.bytes.per.reducer=<number>
		In order to limit the maximum number of reducers:
		  set hive.exec.reducers.max=<number>
		In order to set a constant number of reducers:
		  set mapreduce.job.reduces=<number>
		Starting Job = job_1514876544679_0008, Tracking URL = http://hadoop-01:8088/proxy/application_1514876544679_0008/
		Kill Command = /home/hadoop/software/hadoop/bin/hadoop job  -kill job_1514876544679_0008
		Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
		2018-01-03 06:53:54,887 Stage-1 map = 0%,  reduce = 0%
		2018-01-03 06:54:01,299 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.25 sec
		2018-01-03 06:54:08,779 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 2.73 sec
		MapReduce Total cumulative CPU time: 2 seconds 730 msec
		Ended Job = job_1514876544679_0008
		MapReduce Jobs Launched: 
		Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 2.73 sec   HDFS Read: 6685 HDFS Write: 46 SUCCESS
		Total MapReduce CPU Time Spent: 2 seconds 730 msec
		OK
		hello   2
		hive    2
		mysql   1
		spark   2
		word    1
		world   2
		Time taken: 24.409 seconds, Fetched: 6 row(s)

	相当于Hadoop中的reduce函数	
	
		
		
		
		
		
		
		
		
		
		
		
		
		
		