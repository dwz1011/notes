# MapReduce之wordcount(词频统计) 

- 创建文件

		[hadoop@hadoop-01 ~]$ touch test.log
		[hadoop@hadoop-01 ~]$ vi test.log
		1
		2
		3
		4
		5
		6
		7
		8
		9
		1
		2
		3
		4
		5
		6
		7
		8
		9
		1
		2
		3
		3
		1
		2
		3
		1
		2
		"test.log" 27L, 54C written                                                                                                                                              
		[hadoop@hadoop-01 ~]$ 
		
- hadoop创建目录及上传

		[hadoop@hadoop-01 ~]$ hadoop fs -mkdir /testdir
		[hadoop@hadoop-01 ~]$ hadoop fs -put test.log /testdir/
		[hadoop@hadoop-01 ~]$ hadoop fs -ls /testdir
		Found 1 items
		-rw-r--r--   1 hadoop supergroup         54 2017-12-19 16:19 /testdir/test.log
		[hadoop@hadoop-01 ~]$ 		

- 查看官方封装好的函数,选取wordcount

		[hadoop@hadoop-01 ~]$  cd /opt/software/hadoop/share/hadoop/mapreduce/
		[hadoop@hadoop-01 mapreduce]$ ll
		total 5120
		-rw-rw-r--. 1 hadoop hadoop  562906 Jun  2  2017 hadoop-mapreduce-client-app-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop  782746 Jun  2  2017 hadoop-mapreduce-client-common-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop 1571185 Jun  2  2017 hadoop-mapreduce-client-core-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop  195006 Jun  2  2017 hadoop-mapreduce-client-hs-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop   31539 Jun  2  2017 hadoop-mapreduce-client-hs-plugins-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop   67004 Jun  2  2017 hadoop-mapreduce-client-jobclient-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop 1587163 Jun  2  2017 hadoop-mapreduce-client-jobclient-2.8.1-tests.jar
		-rw-rw-r--. 1 hadoop hadoop   75501 Jun  2  2017 hadoop-mapreduce-client-shuffle-2.8.1.jar
		-rw-rw-r--. 1 hadoop hadoop  301938 Jun  2  2017 hadoop-mapreduce-examples-2.8.1.jar
		drwxrwxr-x. 2 hadoop hadoop    4096 Jun  2  2017 jdiff
		drwxrwxr-x. 2 hadoop hadoop    4096 Jun  2  2017 lib
		drwxrwxr-x. 2 hadoop hadoop    4096 Jun  2  2017 lib-examples
		drwxrwxr-x. 2 hadoop hadoop    4096 Jun  2  2017 sources
		[hadoop@hadoop-01 mapreduce]$ hadoop jar hadoop-mapreduce-examples-2.8.1.jar
		An example program must be given as the first argument.
		Valid program names are:
		  aggregatewordcount: An Aggregate based map/reduce program that counts the words in the input files.
		  aggregatewordhist: An Aggregate based map/reduce program that computes the histogram of the words in the input files.
		  bbp: A map/reduce program that uses Bailey-Borwein-Plouffe to compute exact digits of Pi.
		  dbcount: An example job that count the pageview counts from a database.
		  distbbp: A map/reduce program that uses a BBP-type formula to compute exact bits of Pi.
		  grep: A map/reduce program that counts the matches of a regex in the input.
		  join: A job that effects a join over sorted, equally partitioned datasets
		  multifilewc: A job that counts words from several files.
		  pentomino: A map/reduce tile laying program to find solutions to pentomino problems.
		  pi: A map/reduce program that estimates Pi using a quasi-Monte Carlo method.
		  randomtextwriter: A map/reduce program that writes 10GB of random textual data per node.
		  randomwriter: A map/reduce program that writes 10GB of random data per node.
		  secondarysort: An example defining a secondary sort to the reduce.
		  sort: A map/reduce program that sorts the data written by the random writer.
		  sudoku: A sudoku solver.
		  teragen: Generate data for the terasort
		  terasort: Run the terasort
		  teravalidate: Checking results of terasort
		  wordcount: A map/reduce program that counts the words in the input files.
		  wordmean: A map/reduce program that counts the average length of the words in the input files.
		  wordmedian: A map/reduce program that counts the median length of the words in the input files.
		  wordstandarddeviation: A map/reduce program that counts the standard deviation of the length of the words in the input files.
		[hadoop@hadoop-01 mapreduce]$ 

- 运行wordcount

	hadoop jar 官方模板jar包    函数    输入目录 输出目录(未创建)
	
		[hadoop@hadoop-01 mapreduce]$ hadoop jar hadoop-mapreduce-examples-2.8.1.jar wordcount /testdir /testout
		17/12/19 16:24:04 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
		17/12/19 16:24:05 WARN hdfs.DataStreamer: Caught exception
		java.lang.InterruptedException
		        at java.lang.Object.wait(Native Method)
		        at java.lang.Thread.join(Thread.java:1245)
		        at java.lang.Thread.join(Thread.java:1319)
		        at org.apache.hadoop.hdfs.DataStreamer.closeResponder(DataStreamer.java:927)
		        at org.apache.hadoop.hdfs.DataStreamer.endBlock(DataStreamer.java:578)
		        at org.apache.hadoop.hdfs.DataStreamer.run(DataStreamer.java:755)
		17/12/19 16:24:05 INFO input.FileInputFormat: Total input files to process : 1
		17/12/19 16:24:05 WARN hdfs.DataStreamer: Caught exception
		java.lang.InterruptedException
		        at java.lang.Object.wait(Native Method)
		        at java.lang.Thread.join(Thread.java:1245)
		        at java.lang.Thread.join(Thread.java:1319)
		        at org.apache.hadoop.hdfs.DataStreamer.closeResponder(DataStreamer.java:927)
		        at org.apache.hadoop.hdfs.DataStreamer.endBlock(DataStreamer.java:578)
		        at org.apache.hadoop.hdfs.DataStreamer.run(DataStreamer.java:755)
		17/12/19 16:24:05 INFO mapreduce.JobSubmitter: number of splits:1
		17/12/19 16:24:06 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1513653312357_0003
		17/12/19 16:24:06 INFO impl.YarnClientImpl: Submitted application application_1513653312357_0003
		17/12/19 16:24:06 INFO mapreduce.Job: The url to track the job: http://hadoop-01:8088/proxy/application_1513653312357_0003/
		17/12/19 16:24:06 INFO mapreduce.Job: Running job: job_1513653312357_0003
		17/12/19 16:24:15 INFO mapreduce.Job: Job job_1513653312357_0003 running in uber mode : false
		17/12/19 16:24:15 INFO mapreduce.Job:  map 0% reduce 0%
		17/12/19 16:24:21 INFO mapreduce.Job:  map 100% reduce 0%
		17/12/19 16:24:28 INFO mapreduce.Job:  map 100% reduce 100%
		17/12/19 16:24:29 INFO mapreduce.Job: Job job_1513653312357_0003 completed successfully
		17/12/19 16:24:29 INFO mapreduce.Job: Counters: 49
		        File System Counters
		                FILE: Number of bytes read=78
		                FILE: Number of bytes written=272705
		                FILE: Number of read operations=0
		                FILE: Number of large read operations=0
		                FILE: Number of write operations=0
		                HDFS: Number of bytes read=163
		                HDFS: Number of bytes written=36
		                HDFS: Number of read operations=6
		                HDFS: Number of large read operations=0
		                HDFS: Number of write operations=2
		        Job Counters 
		                Launched map tasks=1
		                Launched reduce tasks=1
		                Data-local map tasks=1
		                Total time spent by all maps in occupied slots (ms)=3960
		                Total time spent by all reduces in occupied slots (ms)=4199
		                Total time spent by all map tasks (ms)=3960
		                Total time spent by all reduce tasks (ms)=4199
		                Total vcore-milliseconds taken by all map tasks=3960
		                Total vcore-milliseconds taken by all reduce tasks=4199
		                Total megabyte-milliseconds taken by all map tasks=4055040
		                Total megabyte-milliseconds taken by all reduce tasks=4299776
		        Map-Reduce Framework
		                Map input records=27
		                Map output records=27
		                Map output bytes=162
		                Map output materialized bytes=78
		                Input split bytes=109
		                Combine input records=27
		                Combine output records=9
		                Reduce input groups=9
		                Reduce shuffle bytes=78
		                Reduce input records=9
		                Reduce output records=9
		                Spilled Records=18
		                Shuffled Maps =1
		                Failed Shuffles=0
		                Merged Map outputs=1
		                GC time elapsed (ms)=120
		                CPU time spent (ms)=990
		                Physical memory (bytes) snapshot=321728512
		                Virtual memory (bytes) snapshot=4130537472
		                Total committed heap usage (bytes)=202379264
		        Shuffle Errors
		                BAD_ID=0
		                CONNECTION=0
		                IO_ERROR=0
		                WRONG_LENGTH=0
		                WRONG_MAP=0
		                WRONG_REDUCE=0
		        File Input Format Counters 
		                Bytes Read=54
		        File Output Format Counters 
		                Bytes Written=36
		[hadoop@hadoop-01 mapreduce]$ 

- 验证wordcount，词频统计

		[hadoop@hadoop-01 mapreduce]$ hadoop fs -ls /testout
		Found 2 items
		-rw-r--r--   1 hadoop supergroup          0 2017-12-19 16:24 /testout/_SUCCESS
		-rw-r--r--   1 hadoop supergroup         36 2017-12-19 16:24 /testout/part-r-00000
		[hadoop@hadoop-01 mapreduce]$ hadoop fs -text /testout/part-r-00000
		1       5
		2       5
		3       5
		4       2
		5       2
		6       2
		7       2
		8       2
		9       2
		[hadoop@hadoop-01 mapreduce]$ 














