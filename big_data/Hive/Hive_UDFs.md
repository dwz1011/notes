# Hive自定义函数(UDF)

- **临时函数**

	1. idea编写udf
	2. 打包
			
			Maven Projects ---->Lifecycle ---->package ----> 右击 Run Maven Build
			
	3. rz上传至服务器
	4. 添加jar包 hive>add jar jar_filepath;
	5. 查看jar包 hive>list jars;
	6. 创建临时函数 

			create temporary function my_lower as 'com.example.hive.udf.Lower';
	
- **持久函数**

	1~5步如临时函数一样
	
	6. 上传至hdfs
	7. CREATE FUNCTION myfunc AS 'myclass' USING JAR 'hdfs:///path/to/jar';
	
	```
	注意点：1. 此方法在show functions时是看不到的，但是可以使用
			2. 需要上传至hdfs
	```

- **持久函数，并注册**
	
	1. idea编写udf    如：xxx.java
	2. 源码下载 http://archive.cloudera.com/cdh5/cdh/5/hive-1.1.0-cdh5.7.0-src.tar.gz 
	3. 解压，进入解压后的hive-1.1.0-cdh5.7.0文件夹
	4. 将xxx.java包cp至 .../hive-1.1.0-cdh5.7.0/ql/src/java/org/apache/hadoop/hive/ql/udf目录下
	5. 将 package com.xxx.xxx.xxx; 修改为 package org.apache.hadoop.hive.ql.udf;
	6. 修改.../hive-1.1.0-cdh5.7.0/ql/src/java/org/apache/hadoop/hive/ql/exec/FunctionRegistry.java
	
		    a.import org.apache.hadoop.hive.ql.udf.xxx;
		    b.static中添加
		        system.registerUDF("xxx", xxx.class, false);
		        
	7. 编译---->进入hive-1.1.0-cdh5.7.0目录，mvn clean package -DskipTests -Phadoop-2 -Pdist
	8. 编译成功的包默认在hive-1.1.0-cdh5.7.0/packaging/target/apache-hive-1.1.0-cdh5.7.0-bin.tar.gz
	9. 重新部署，配置 [参考](https://github.com/dwz1011/notes/blob/master/big_data/Hive/Hive_Install_Conf.md)

	```
	注意点：1. 重新配置环境变量
			2. MySQL的驱动包千万不能忘记
	```
	





	
	
	
	
	