# Hadoop基础

### 大数据概述

	可以用“5V + 1C”来概括：
		Variety (多样化）
		Volume (海量）
		Velocity (快速）
		Vitality (灵活）
		Value (价值性）
		Complexity (复杂）
	
### Hadoop与Hadoop生态圈

- Hadoop

		狭义: 软件
		褒义: 以hadoop为主的生态圈
		
- Hadoop1.x

		hdfs: 分布式文件管理系统				    存储
		mapreduce1: 执行引擎  				    计算+资源、作业调度
				
- Hadoop2.x三大组件

		hdfs: 分布式文件管理系统				    存储
		mapreduce2: 执行引擎                   计算
		yarn: 资源(memory cpu)和作业调度平台	    资源
		
### Hadoop编译
	
	[root@hadoop-01 ~]# cd /opt/
	[root@hadoop-01 opt]# mkdir sourcecode software
	[root@hadoop-01 opt]# cd sourcecode
	[root@hadoop-01 sourcecode]# pwd
	/opt/sourcecode
	
- hadoop源代码下载
	
		# 将hadoop-2.8.1-src.tar.gz下载（或者用rz上传）到sourcecode目录
		[root@hadoop-01 sourcecode]# ll
		total 33756
		-rw-r--r--. 1 root root 34523353 Aug 20 12:14 hadoop-2.8.1-src.tar.gz
		
		# 解压
		[root@hadoop-01 sourcecode]# tar -xzvf hadoop-2.8.1-src.tar.gz
		[root@hadoop-01 sourcecode]# ll
		total 33760
		drwxr-xr-x. 17 root root     4096 Jun  2 14:13 hadoop-2.8.1-src
		-rw-r--r--.  1 root root 34523353 Aug 20 12:14 hadoop-2.8.1-src.tar.gz
	
		[root@hadoop-01 sourcecode]# cd hadoop-2.8.1-src
	
- JAVA安装
		
		[root@hadoop-01 ~]# mkdir -p /usr/java
		[root@hadoop-01 ~]# cd /usr/java
		[root@hadoop-01 java]# rz #上传jdk-8u45-linux-x64.gz
		[root@hadoop-01 java]# tar -xzvf jdk-8u45-linux-x64.gz
		
		# 修改用户和用户组
		[root@hadoop-01 java]# chown -R root:root jdk1.8.0_45
		
		# 设置环境变量
		[root@hadoop-01 java]# vi /etc/profile
		# 在最底下加入
		export JAVA_HOME=/usr/java/jdk1.8.0_45
		export PATH=$JAVA_HOME/bin:$PATH
		# 生效
		[root@hadoop-01 java]# source /etc/profile


- Maven安装

		[root@hadoop-01 ~]# cd /opt/software/
		[root@hadoop-01 software]# rz  #上传apache-maven-3.3.9-bin.zip
		[root@hadoop-01 software]# ll
		total 8432
		-rw-r--r--. 1 root root 8617253 Aug 20 12:35 apache-maven-3.3.9-bin.zip
		# 解压
		[root@hadoop-01 software]# unzip apache-maven-3.3.9-bin.zip
		# 设置环境变量
		[root@hadoop-01 software]# vi /etc/profile
		export MAVEN_HOME=/opt/software/apache-maven-3.3.9
		export MAVEN_OPTS="-Xms256m -Xmx512m"
		export PATH=$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
		# 生效
		[root@hadoop-01 software]# source /etc/profile
		# 查看
		[root@hadoop-01 software]# mvn -version
		Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
		Maven home: /opt/software/apache-maven-3.3.9
		Java version: 1.8.0_45, vendor: Oracle Corporation
		Java home: /usr/java/jdk1.8.0_45/jre
		Default locale: en_US, platform encoding: UTF-8
		OS name: "linux", version: "2.6.32-431.el6.x86_64", arch: "amd64", family: "unix"

- Findbugs安装
		
		[root@hadoop-01 ~]# cd /opt/software/
		[root@hadoop-01 software]# rz #上传findbugs-1.3.9.zip
		# 解压
		[root@hadoop-01 software]# unzip findbugs-1.3.9.zip
		# 设置环境变量
		[root@hadoop-01 software]# vi /etc/profile
		export FINDBUGS_HOME=/opt/software/findbugs-1.3.9
		export PATH=$FINDBUGS_HOME/bin:$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
		# 生效
		[root@hadoop-01 software]# source /etc/profile
		# 查看
		[root@hadoop-01 software]# findbugs -version
		1.3.9

- protobuf安装
		
		[root@hadoop-01 ~]# cd /opt/software/
		[root@hadoop-01 software]# rz #上传protobuf-2.5.0.tar.gz
		# 解压
		[root@hadoop-01 software]# tar -xzvf protobuf-2.5.0.tar.gz
		[root@hadoop-01 software]# cd protobuf-2.5.0
		[root@hadoop-01 protobuf-2.5.0]# yum install -y gcc gcc-c++ make cmake
		[root@hadoop-01 protobuf-2.5.0]# ./configure --prefix=/usr/local/protobuf
		[root@hadoop-01 protobuf-2.5.0]# make && make install
		# 设置环境变量
		[root@hadoop-01 java]# vi /etc/profile
		export PROTOC_HOME=/usr/local/protobuf
		export PATH=$PROTOC_HOME/bin:$FINDBUGS_HOME/bin:$MAVEN_HOME/bin:$JAVA_HOME/bin:$PATH
		# 生效
		[root@hadoop-01 protobuf-2.5.0]# source /etc/profile
		# 查看
		[root@hadoop-01 protobuf-2.5.0]# protoc --version
		libprotoc 2.5.0

- 其他依赖

		yum install -y openssl openssl-devel svn ncurses-devel zlib-devel libtool
		yum install -y snappy snappy-devel bzip2 bzip2-devel lzo lzo-devel lzop autoconf automake

- 编译

		[root@hadoop-01 sourcecode]# cd hadoop-2.8.1-src
		[root@hadoop-01 sourcecode]# mvn clean package -Pdist,native -DskipTests -Dtar

目录
	
	/opt/sourcecode/hadoop-2.8.1-src/hadoop-dist/target/hadoop-2.8.1.tar.gz











		