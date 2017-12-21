# Hadoop伪分布式安装(MapReduce+Yarn)

	MapReduce:计算
	Yarn: 资源(CPU、内存等)调度和作业(程序)平台

- 修改mapred-site.xml
		
		[hadoop@hadoop-01 ~]# cd /opt/software/hadoop/etc/hadoop
		[hadoop@hadoop-01 hadoop]# cp mapred-site.xml.template  mapred-site.xml
		[hadoop@hadoop-01 hadoop]# vi mapred-site.xml
		<configuration>
			<property>
				<name>mapreduce.framework.name</name>
				<value>yarn</value>
			</property>
		</configuration>
		
- 修改yarn-site.xml

		[hadoop@hadoop-01 hadoop]# vi yarn-site.xml
		<configuration>
			<property>
				<name>yarn.nodemanager.aux-services</name>
        		<value>mapreduce_shuffle</value>
    		</property>
		</configuration>
	
- 启动
	
		[hadoop@hadoop-01 hadoop]# cd ../../
		[hadoop@hadoop-01 hadoop]# sbin/start-yarn.sh
		
访问：http://192.168.137.130:8088

- 关闭
		
		[hadoop@hadoop-01 hadoop]# sbin/stop-yarn.sh
	
		
		
		
		
		
		
		
		
		
		