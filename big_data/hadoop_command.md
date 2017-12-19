# Hadoop 常用命令

### hadoop fs == hdfs dfs

- 将文件上传至hadoop的根目录/下载至本地

		hadoop fs -put filename / 	
		hadoop fs -get /filename	
		# '/'不是Linux的根目录，表示hadoop的根目录
		例：
		[hadoop@hadoop-01 ~]$ hdfs dfs -put test.log /
		[hadoop@hadoop-01 data]$ hdfs dfs -get /test.log
		
		[-moveFromLocal <localsrc> ... <dst>]
		[-moveToLocal <src> <localdst>]
	
- 查看hadoop里的文件

		hadoop fs -ls /
		例：
		[hadoop@hadoop-01 ~]$ hadoop fs -ls /
		Found 3 items
		-rw-r--r--   1 hadoop supergroup          6 2017-12-19 11:18 /test.log
		drwx------   - hadoop supergroup          0 2017-12-14 15:17 /tmp
		drwxr-xr-x   - hadoop supergroup          0 2017-12-14 15:17 /user

- 查看hadoop里的文件的内容

		hadoop fs -cat filename
		例：
		[hadoop@hadoop-01 ~]$ hadoop fs -cat  /test.log
		hello
		
- 创建文件夹

		hadoop fs -mkdir -p /filename/filename
		例：
		[hadoop@hadoop-01 ~]$ hadoop fs -mkdir -p /dwz01/dwz001
		
- 删除
		
		#  [-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
		1.配置回收站
		vi core-site.xml
		<property>  
		  <name>fs.trash.interval</name>  
		  <value>10080</value>  		# 回收站保留时间（分钟）
		</property> 
		
		
		2.测试
		hadoop fs -rm -r -f /xxxx  ---》进入回收站，是可以恢复的
		hadoop fs -rm -r -f -skipTrash /xxxx ---》不进入回收站，是不可以恢复的
		例：
		A.进入回收站
		[hadoop@hadoop-01 hadoop]$ hadoop fs -rm -r -f /test.log
		17/12/19 14:06:06 INFO fs.TrashPolicyDefault: Moved: 'hdfs://192.168.137.130:9000/test.log' to trash at: hdfs://192.168.137.130:9000/user/hadoop/.Trash/Current/test.log
		[hadoop@hadoop-01 hadoop]$ hadoop fs -ls
		Found 1 items
		drwx------   - hadoop supergroup          0 2017-12-19 14:06 .Trash
		[hadoop@hadoop-01 data]$ hadoop fs -ls .Trash/Current
		Found 1 items
		-rw-r--r--   1 hadoop supergroup          6 2017-12-19 14:12 .Trash/Current/test.log
		恢复刚刚删除的目录
		[hadoop@hadoop-01 hadoop]$ hadoop fs -mv /user/hadoop/.Trash/Current/test.log /
		[hadoop@hadoop-01 hadoop]$ hadoop fs -ls /
		Found 4 items
		drwxr-xr-x   - hadoop supergroup          0 2017-12-19 11:44 /dwz01
		-rw-r--r--   1 hadoop supergroup          6 2017-12-19 11:18 /test.log
		drwx------   - hadoop supergroup          0 2017-12-14 15:17 /tmp
		drwxr-xr-x   - hadoop supergroup          0 2017-12-14 15:17 /user
		B.不进入回收站
		[hadoop@hadoop-01 hadoop]$ hadoop fs -rm -r -f -skipTrash /test.log
		Deleted /test.log
		[hadoop@hadoop-01 hadoop]$ hadoop fs -ls
		Found 1 items
		drwx------   - hadoop supergroup          0 2017-12-19 14:06 .Trash
		[hadoop@hadoop-01 hadoop]$ hadoop fs -ls .Trash
		Found 1 items
		drwx------   - hadoop supergroup          0 2017-12-19 14:09 .Trash/Current
		[hadoop@hadoop-01 hadoop]$ hadoop fs -ls .Trash/Current
		[hadoop@hadoop-01 hadoop]$ 		
		
	
	