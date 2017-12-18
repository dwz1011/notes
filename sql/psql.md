# postgresql

### 创建用户
	
	CREATE USER 'username' WITH PASSWORD 'rdyhdenali' SUPERUSER ;
	
### 创建databaase
	
	CREATE DATABASE dbname WITH OWNER=postgres;
	
### 连接数据库

	psql -h hostname -U username -d postgres -p post -W password 
	
### 基本命令

	➜  ~ createdb test
	➜  ~ psql -d test
	psql (9.6.1)
	Type "help" for help.
	test=#

	test=# \l 查看系统中现存的数据库 
	test=# \q 退出客户端程序psql 
	test=# \c 从一个数据库中转到另一个数据库中
	test=# \dt [tableName] 查看表 
	test=# \d [tableName] 查看表结构
	test=# \du 查看数据库的用户信息
	test=# \di 查看索引 
	test=# \conninfo 出当前数据库和连接的信息
	test=# \e 打开文本编辑器。
	test=# \conninfo 列出当前数据库和连接的信息。
	test=# \dn 或者 \dnS 查看当前database下的schema
	test=# \dt 查看当前database的当前搜索路径下schema的表
	test=# SELECT * FROM pg_stat_activity;  查看当前对数据库的所有操作进程
	
### 基本的数据库操作，就是使用一般的SQL语言。 

- 创建新表 

		CREATE TABLE usertbl(name VARCHAR(20), signupdate DATE); 

- 插入数据 

		INSERT INTO usertbl(name, signupdate) VALUES(‘张三’, ’2013-12-22′); 

-  选择记录 

		SELECT * FROM user_tbl; 

-  更新数据 

		UPDATE user_tbl set name = ‘李四’ WHERE name = ‘张三’; 

- 删除记录 
		
		DELETE FROM user_tbl WHERE name = ‘李四’ ; 

- 添加栏位 
		
		ALTER TABLE user_tbl ADD email VARCHAR(40); 

- 更新结构 
		
		ALTER TABLE usertbl ALTER COLUMN signupdate SET NOT NULL; 

- 更名栏位 
		
		ALTER TABLE usertbl RENAME COLUMN signupdate TO signup; 

- 删除栏位 
		
		ALTER TABLE user_tbl DROP COLUMN email; 

- 表格更名 
		
		ALTER TABLE usertbl RENAME TO backuptbl; 

- 删除表格 
		
		DROP TABLE IF EXISTS backup_tbl; 
		
### 配置search_path路径,为了能够找到schema的表等

- 查看当前的schema值

		postgres=# SHOW search_path;
	   		search_path
		-----------------
		 "$user", public
		(1 row)
		
- 创建schema值

		test=# create schema test;
		CREATE SCHEMA
		
- 将新schema test加入到搜索路径

		postgres=# set search_path to test,public;
		SET
		postgres=# SHOW search_path;
		 search_path
		--------------
		 test, public
		(1 row)
		
- 创建schema下的表(test2)

		postgres=# create table test.test2;
		
- 删除schame
		
		postgres=# drop schema test;
		
### 数据的导入导出

- 导出

`pg_dump [option]...[DBNAME]`

	常用的options:
		-f --file=FILENAME 		导出到文件或文件夹
		-F --format=c|d|t|p		导出文件的格式(文本，文件夹，tar包)
		-Z --compress=0-9		被压缩格式的压缩级别
		-V --versoin       		输出版本信息, 然后退出
		-a --data-only         	只转储数据,不包括模式
		-c --clean             	在重新创建之前，先清除（删除）数据库对象
		-C --create            	在转储中包括命令,以便创建数据库
		-n --schema=SCHEMA    	只转储指定名称的模式
		-s --schema-only       	只转储模式, 不包括数据
		-t --table=TABLE       	只转储指定名称的表
		------
		 -h --host=HOSTNAME    	主机名
		 -p --port=PORT			端口号
		 -U --username=NAME		用户名
		 -w --no-password		无密码登录
		 -W --password
		 -d --dbname=DBNAME
		 
- 导入

	导入数据时首先创建数据库再用psql导入
		
		➜  ~ createdb test
	
	- psql(文本格式的导入)
		
			➜  ~ psql -d test -U username -f test(文件路径)
			SET
			...
			ALTER TABLE
		
	- pg_restore(其他格式的导入)

			➜  ~ pg_restore -U username -h localhost -d test t.tar(文件路径)