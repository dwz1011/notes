# MySQL 5.6.23 Install



### Download and Check MD5

	[root@hadoop-01 ~]$ cd /usr/local
	[root@hadoop-01 local]$ wget https://downloads.mariadb.com/archives/mysql-5.6/mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz
	[root@hadoop-01 local]$ wget https://downloads.mariadb.com/archives/mysql-5.6/mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz.md5
	
	[root@hadoop-01 local]$ vi mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz.md5
	61affe944eff55fcf51b31e67f25dc10  mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz
	
	[root@hadoop-01 local]$ md5sum mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz
	61affe944eff55fcf51b31e67f25dc10  mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz

### Check isnot install

	[root@hadoop-01 local]$ ps -ef|grep mysqld
	
### tar and mv

	[root@hadoop-01 local]$ tar xzvf mysql-5.6.23-linux-glibc2.5-x86_64.tar.gz
	[root@hadoop-01 local]$ mv mysql-5.6.23-linux-glibc2.5-x86_64 mysql
	
### Create group and user
	
	[root@hadoop-01 local]$ groupadd -g 101 dba
	[root@hadoop-01 local]$ useradd -u 514 -g dba -G root -d /usr/local/mysql mysqladmin
	[root@hadoop-01 local]$ id mysqladmin
	uid=514(mysqladmin) gid=101(dba) groups=101(dba),0(root)
	
	[root@hadoop-01 local]$ passwd mysqladmin
	
	# copy 环境变量配置文件至mysqladmin用户的home目录中,为了以下步骤配置个人环境变量
	[root@hadoop-01 local]$ cp /etc/skel/.* /usr/local/mysql
	
### Create /etc/my.cnf(640)

	[root@hadoop-01 mysql]$ cd /etc/my.cnf
	删除内容，并将以下内容复制进去
	[client]
	port            = 3306
	socket          = /usr/local/mysql/data/mysql.sock
	 
	[mysqld]
	port            = 3306
	socket          = /usr/local/mysql/data/mysql.sock
	
	skip-external-locking
	key_buffer_size = 256M
	sort_buffer_size = 2M
	read_buffer_size = 2M
	read_rnd_buffer_size = 4M
	query_cache_size= 32M
	max_allowed_packet = 16M
	myisam_sort_buffer_size=128M
	tmp_table_size=32M
	
	table_open_cache = 512
	thread_cache_size = 8
	wait_timeout = 86400
	interactive_timeout = 86400
	max_connections = 600
	
	# Try number of CPU's*2 for thread_concurrency
	thread_concurrency = 32
	
	#isolation level and default engine 
	default-storage-engine = INNODB
	transaction-isolation = READ-COMMITTED
	
	server-id  = 1
	basedir     = /usr/local/mysql
	datadir     = /usr/local/mysql/data
	pid-file     = /usr/local/mysql/data/hostname.pid
	
	#open performance schema
	log-warnings
	sysdate-is-now
	
	binlog_format = MIXED
	log_bin_trust_function_creators=1
	log-error  = /usr/local/mysql/data/hostname.err
	log-bin=/usr/local/mysql/arch/mysql-bin
	#other logs
	#general_log =1
	#general_log_file  = /usr/local/mysql/data/general_log.err
	#slow_query_log=1
	#slow_query_log_file=/usr/local/mysql/data/slow_log.err
	
	#for replication slave
	#log-slave-updates 
	#sync_binlog = 1
	
	#for innodb options 
	innodb_data_home_dir = /usr/local/mysql/data/
	innodb_data_file_path = ibdata1:500M:autoextend
	innodb_log_group_home_dir = /usr/local/mysql/arch
	innodb_log_files_in_group = 2
	innodb_log_file_size = 200M
	
	# rember change
	innodb_buffer_pool_size = 2048M
	innodb_additional_mem_pool_size = 50M
	innodb_log_buffer_size = 16M
	
	innodb_lock_wait_timeout = 100
	#innodb_thread_concurrency = 0
	innodb_flush_log_at_trx_commit = 1
	innodb_locks_unsafe_for_binlog=1
	
	#innodb io features: add for mysql5.5.8
	performance_schema
	innodb_read_io_threads=4
	innodb-write-io-threads=4
	innodb-io-capacity=200
	#purge threads change default(0) to 1 for purge
	innodb_purge_threads=1
	innodb_use_native_aio=on
	
	#case-sensitive file names and separate tablespace
	innodb_file_per_table = 1
	lower_case_table_names=1
	
	[mysqldump]
	quick
	max_allowed_packet = 16M
	
	[mysql]
	no-auto-rehash
	
	[mysqlhotcopy]
	interactive-timeout
	
	[myisamchk]
	key_buffer_size = 256M
	sort_buffer_size = 256M
	read_buffer = 2M
	write_buffer = 2M	
	
### chown and chmod privileges and try first install

	[root@hadoop-01 local]$ chown  mysqladmin:dba /etc/my.cnf 
	[root@hadoop-01 local]$ chmod  640 /etc/my.cnf  
	[root@hadoop-01 etc]$ ll my.cnf
	-rw-r----- 1 mysqladmin dba 2201 Aug 25 23:09 my.cnf	
	[root@hadoop-01 local]$ chown -R mysqladmin:dba /usr/local/mysql
	[root@hadoop-01 local]$ chmod -R 755 /usr/local/mysql 
	[root@hadoop-01 local]$ su - mysqladmin
	[mysqladmin@hadoop-01 ~]$ pwd
	/usr/local/mysql
	
	[mysqladmin@hadoop-01 ~]$ mkdir arch
	[mysqladmin@hadoop-01 ~]$ scripts/mysql_install_db ###import
	Installing MySQL system tables..../bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory  #缺少libaio.so 包
	
	[root@hadoop-01 local]$ yum -y install libaio
 	
	
### Again  install	

	[mysqladmin@hadoop-01 ~]$ scripts/mysql_install_db  --user=mysqladmin --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data 

### Configure mysql service and boot auto start	
	[root@hadoop-01 ~]$ cd /usr/local/mysql
	#将服务文件拷贝到init.d下，并重命名为mysql
	[root@hadoop-01 mysql]$ cp support-files/mysql.server /etc/rc.d/init.d/mysql 
	#赋予可执行权限
	[root@hadoop-01 mysql]$ chmod +x /etc/rc.d/init.d/mysql
	#删除服务
	[root@hadoop-01 mysql]$ chkconfig --del mysql
	#添加服务
	[root@hadoop-01 mysql]$ chkconfig --add mysql
	[root@hadoop-01 mysql]$ chkconfig --level 345 mysql on
	
### Start mysql and to view process and listening
	
	[root@hadoop-01 mysql]$ su - mysqladmin
	[mysqladmin@hadoop-01 ~]$ pwd
	/usr/local/mysql
	[mysqladmin@hadoop-01 ~]$ rm -rf my.cnf
	[mysqladmin@hadoop-01 ~]$  bin/mysqld_safe &
	
	[mysqladmin@hadoop-01 ~]$ ps -ef|grep mysqld
	
	[mysqladmin@hadoop-01 ~]$ netstat -tulnp | grep mysql
	
	[root@hadoop-01 local]$ service mysql status

### Login mysql 

	[mysqladmin@hadoop-01 ~]$ bin/mysql
	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| test               |
	+--------------------+
	
### Update password and Purge user	
	mysql> use mysql;
	
	mysql> update user set password=password('password') where user='root';

	
	
	
	
	
	
	
	
	