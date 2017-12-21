# Hadoop伪分布式安装

### Hadoop安装的三种模式：

- 单机模式（standalone）
		
		单机模式是Hadoop的默认模式。当首次解压Hadoop的源码包时，Hadoop无法了解硬件安装环境，便保守地选择了最小配置。
        在这种默认模式下所有3个XML文件均为空。当配置文件为空时，Hadoop会完全运行在本地。
        因为不需要与其他节点交互，单机模式就不使用HDFS，也不加载任何Hadoop的守护进程。
        该模式主要用于开发调试MapReduce程序的应用逻辑。
**此程序一般不建议安装，网络上很少这方面资料**
 
- 伪分布模式（Pseudo-Distributed Mode）
		
		伪分布模式在“单节点集群”上运行Hadoop，其中所有的守护进程都运行在同一台机器上。
    	该模式在单机模式之上增加了代码调试功能，允许你检查内存使用情况，HDFS输入输出，以及其他的守护进程交互。
		比如namenode，datanode，secondarynamenode，jobtracer，tasktracer这5个进程，都能在集群上看到。
     
- 全分布模式（Fully Distributed Mode）
		
		Hadoop守护进程运行在一个集群上。
		意思是说master上看到namenode,jobtracer，secondarynamenode可以安装在master节点，也可以单独安装。slave节点能看到datanode和tasktracer
		
### Hadoop伪分布安装

**环境要求java、ssh**

- 添加hadoop用户

		[root@hadoop-01 ~]# useradd hadoop
		[root@hadoop-01 ~]# vi /etc/sudoers
		# 找到root 	ALL=(ALL) 	ALL，添加
		hadoop 	ALL=(ALL)       NOPASSWD:ALL
	
- 上传并解压

		[root@hadoop-01 software]# rz #上传hadoop-2.8.1.tar.gz
		[root@hadoop-01 software]# tar -xzvf hadoop-2.8.1.tar.gz	
- 软连接

		[root@hadoop-01 software]# ln -s /opt/software/hadoop-2.8.1 /opt/software/hadoop
		
- 设置环境变量

		[root@hadoop-01 software]# vi /etc/profile
		export HADOOP_HOME=/opt/software/hadoop
		export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PROTOC_HOME/bin:$FINDBUGS_HOME/bin:$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
		[root@hadoop-01 software]# source /etc/profile
		
- 设置用户用户组	
		
		[root@hadoop-01 software]# chown -R hadoop:hadoop hadoop
		[root@hadoop-01 software]# chown -R hadoop:hadoop hadoop/*
		[root@hadoop-01 software]# chown -R hadoop:hadoop hadoop-2.8.1
				
		[root@hadoop-01 software]# cd hadoop
		[root@hadoop-01 hadoop]# rm -f *.txt
		
		
- 切换用户hadoop
		
		[root@hadoop-01 software]# su - hadoop
		[root@hadoop-01 hadoop]# ll
		total 32
		drwxrwxr-x. 2 hadoop hadoop 4096 Jun  2 14:24 bin
		drwxrwxr-x. 3 hadoop hadoop 4096 Jun  2 14:24 etc
		drwxrwxr-x. 2 hadoop hadoop 4096 Jun  2 14:24 include
		drwxrwxr-x. 3 hadoop hadoop 4096 Jun  2 14:24 lib
		drwxrwxr-x. 2 hadoop hadoop 4096 Aug 20 13:59 libexec
		drwxr-xr-x. 2 hadoop hadoop 4096 Aug 20 13:59 logs
		drwxrwxr-x. 2 hadoop hadoop 4096 Jun  2 14:24 sbin
		drwxrwxr-x. 4 hadoop hadoop 4096 Jun  2 14:24 share	
		
		# bin:		可执行文件
		# etc: 		配置文件
		# sbin:		shell脚本，启动关闭hdfs,yarn等
		
		
- 配置文件

		[hadoop@hadoop-01 ~]# cd /opt/software/hadoop
		[hadoop@hadoop-01 hadoop]# vi etc/hadoop/core-site.xml
		<configuration>
		    <property>
		        <name>fs.defaultFS</name>
		        <value>hdfs://192.168.137.130:9000</value>    # 配置自己机器的IP
		    </property>
		</configuration>
		
		[hadoop@hadoop-01 hadoop]# vi etc/hadoop/hdfs-site.xml
		<configuration>
		    <property>
		        <name>dfs.replication</name>
		        <value>1</value>
		    </property>
		</configuration>
		
		

- 配置hadoop用户的ssh信任关系

		# 公钥/密钥   配置无密码登录
		[hadoop@hadoop-01 ~]# ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
		[hadoop@hadoop-01 ~]# cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
		[hadoop@hadoop-01 ~]# chmod 0600 ~/.ssh/authorized_keys
		
		# 查看日期，看是否配置成功
		[hadoop@hadoop-01 ~]# ssh hadoop-01 date
		The authenticity of host 'hadoop-01 (192.168.137.130)' can't be established.
		RSA key fingerprint is 09:f6:4a:f1:a0:bd:79:fd:34:e7:75:94:0b:3c:83:5a.
		Are you sure you want to continue connecting (yes/no)? yes   # 第一次回车输入yes
		Warning: Permanently added 'hadoop-01,192.168.137.130' (RSA) to the list of known hosts.
		Sun Aug 20 14:22:28 CST 2017
		
		[hadoop@hadoop-01 ~]# ssh hadoop-01 date   #不需要回车输入yes,即OK
		Sun Aug 20 14:22:29 CST 2017
		
		[hadoop@hadoop-01 ~]# ssh localhost date
		The authenticity of host 'hadoop-01 (192.168.137.130)' can't be established.
		RSA key fingerprint is 09:f6:4a:f1:a0:bd:79:fd:34:e7:75:94:0b:3c:83:5a.
		Are you sure you want to continue connecting (yes/no)? yes   # 第一次回车输入yes
		Warning: Permanently added 'hadoop-01,192.168.137.130' (RSA) to the list of known hosts.
		Sun Aug 20 14:22:28 CST 2017
		[hadoop@hadoop-01 ~]# ssh localhost date   #不需要回车输入yes,即OK
		Sun Aug 20 14:22:29 CST 2017

- 格式化和启动

		[hadoop@hadoop-01 hadoop]# bin/hdfs namenode -format
		[hadoop@hadoop-01 hadoop]# sbin/start-dfs.sh
		ERROR:
			hadoop-01: Error: JAVA_HOME is not set and could not be found.			localhost: Error: JAVA_HOME is not set and could not be found.
		解决方法:添加环境变量
		[hadoop@hadoop-01 hadoop]#  vi etc/hadoop/hadoop-env.sh
		# 将export JAVA_HOME=${JAVA_HOME}改为
		export JAVA_HOME=/usr/java/jdk1.8.0_45
		
		[hadoop@hadoop-01 hadoop]# sbin/start-dfs.sh
		ERROR:
			mkdir: cannot create directory `/opt/software/hadoop-2.8.1/logs': Permission denied
		解决方法:
		[hadoop@hadoop-01 hadoop]# exit
		[root@hadoop-01 hadoop]# cd ../
		[root@hadoop-01 software]# chown -R hadoop:hadoop hadoop-2.8.1
		[root@hadoop-01 software]# su - hadoop
		[root@hadoop-01 ~]# cd /opt/software/hadoop
		
		# 继续启动
		[hadoop@hadoop-01 hadoop]# sbin/start-dfs.sh

- 检查是否成功

		[hadoop@hadoop-01 hadoop]# jps
		19536 DataNode
		19440 NameNode
		19876 Jps
		19740 SecondaryNameNode
	
访问： http://192.168.137.130:50070

- 修改dfs启动的进程，以hadoop-01启动

		启动的三个进程：
		namenode: hadoop-01    bin/hdfs getconf -namenodes
		datanode: localhost    datanodes (using default slaves file)   etc/hadoop/slaves
		secondarynamenode: 0.0.0.0
		
		[hadoop@hadoop-01 ~]# cd /opt/software/hadoop
		[hadoop@hadoop-01 hadoop]# echo  "hadoop-01" > ./etc/hadoop/slaves 
		[hadoop@hadoop-01 hadoop]# cat ./etc/hadoop/slaves 
		hadoop-01
		
		[hadoop@hadoop-01 hadoop]# vi ./etc/hadoop/hdfs-site.xml
		<property>
			<name>dfs.namenode.secondary.http-address</name>
			<value>hadoop-01:50090</value>
		</property>
		<property>
			<name>dfs.namenode.secondary.https-address</name>
			<value>hadoop-01:50091</value>
		</property>
		
		# 重启
		[hadoop@hadoop-01 hadoop]# sbin/stop-dfs.sh
		[hadoop@hadoop-01 hadoop]# sbin/start-dfs.sh
		













	
	
	