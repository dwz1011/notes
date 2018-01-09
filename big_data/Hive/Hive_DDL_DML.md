# Hive DDL and DML

DDL([create/drop/alter](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-Create/Drop/Alter/UseDatabase))

DML([load/insert/update/delete/merge](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DML), [import/export](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ImportExport#LanguageManualImportExport-Overview))

## database
	
	对应HDFS上的一个文件夹
	默认default：/user/hive/warehouse

create语法
	
	CREATE (DATABASE|SCHEMA) [IF NOT EXISTS] database_name
	  [COMMENT database_comment]
	  [LOCATION hdfs_path]
	  [WITH DBPROPERTIES (property_name=property_value, ...)];
	  
	
	[hadoop@hadoop-01 ~]$ hadoop fs -ls /user/hive/warehouse
	COMMENT 注释
	LOCATION 指定hdfs的路径，默认是在/user/hive/warehouse路径下
	WITH DBPROPERTIES(key=value) 属性
	  
例：
	
	1. CREATE DATABASE IF NOT EXISTS dwz_hive;
	2. CREATE DATABASE IF NOT EXISTS dwz_hive3
		LOCATION '/dwz_hive2';
	3. CREATE DATABASE IF NOT EXISTS dwz_hive3
		COMMENT "it is a database"
		LOCATION '/dwz_hive3'
		WITH DBPROPERTIES("user"="dwz");
	
- 查看数据库 	`show databases;`
- 查看数据库详情 	`desc database table_name;`或`desc database extended dwz_hive3;`
	
drop语法

	DROP (DATABASE|SCHEMA) [IF EXISTS] database_name [RESTRICT|CASCADE];
	
例：

	hive>DROP DATABASE IF EXISTS dwz_hive2;
	OK
	Time taken: 0.548 seconds
	如果数据库中有表，会报错
	hive> DROP DATABASE IF EXISTS dwz_hive3;
	FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. 	InvalidOperationException(message:Database dwz_hive3 is not empty. One or more tables exist.)
	级联删除
	hive> DROP DATABASE IF EXISTS dwz_hive3 CASCADE;
	OK
	Time taken: 0.672 seconds
	注：CASCADE基本不用

alter语法

	ALTER (DATABASE|SCHEMA) database_name SET DBPROPERTIES (property_name=property_value, ...);   -- (Note: SCHEMA added in Hive 0.14.0)
 
	ALTER (DATABASE|SCHEMA) database_name SET OWNER [USER|ROLE] user_or_role;   -- (Note: Hive 0.13.0 and later; SCHEMA added in Hive 0.14.0)
 
	ALTER (DATABASE|SCHEMA) database_name SET LOCATION hdfs_path; -- (Note: Hive 2.2.1, 2.4.0 and later)
	
## table

**建表**

1. Create Table

		CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name    -- (Note: TEMPORARY available in Hive 0.14.0 and later)
		  [(col_name data_type [COMMENT col_comment], ... [constraint_specification])]
		  [COMMENT table_comment]
		  [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
		  [CLUSTERED BY (col_name, col_name, ...) [SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
		  [SKEWED BY (col_name, col_name, ...)                  -- (Note: Available in Hive 0.10.0 and later)]
		     ON ((col_value, col_value, ...), (col_value, col_value, ...), ...)
		     [STORED AS DIRECTORIES]
		  [
		   [ROW FORMAT row_format] 
		   [STORED AS file_format]
		     | STORED BY 'storage.handler.class.name' [WITH SERDEPROPERTIES (...)]  -- (Note: Available in Hive 0.6.0 and later)
		  ]
		  [LOCATION hdfs_path]
		  [TBLPROPERTIES (property_name=property_value, ...)]   -- (Note: Available in Hive 0.6.0 and later)
		  [AS select_statement];   -- (Note: Available in Hive 0.5.0 and later; not supported for external tables) 
		
		TEMPORARY ----------> 临时表
		EXTERNAL	----------> 外部表
		COMMENT	----------> 表/字段 备注
		constraint_specification	----------> 限制，基本不用
		PARTITIONED BY	----------> 分区表
		ROW FORMAT	---------->
				row_format:
					DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
			
		STORED AS file_format	----------> 文件格式
				file_format:
				  : SEQUENCEFILE
				  | TEXTFILE    -- (Default, depending on hive.default.fileformat configuration)
				  | RCFILE      -- (Note: Available in Hive 0.6.0 and later)
				  | ORC         -- (Note: Available in Hive 0.11.0 and later)
				  | PARQUET     -- (Note: Available in Hive 0.13.0 and later)
				  | AVRO        -- (Note: Available in Hive 0.14.0 and later)
				  | INPUTFORMAT input_format_classname OUTPUTFORMAT output_format_classname   可以自定义
		
		LOCATION 	----------> 指定hdfs的路径
		TBLPROPERTIES	----------> 表的属性
		
2. Create Table Like

		CREATE TABLE new_table_name LIKE old_table_name;
		
		LIKE仅仅拷贝的是表，不包含数据	
	
3. Create Table As Select (CTAS)

		CREATE TABLE new_table_name AS SELECT * FROM old_table_name;
		走mr作业
		
查看表信息

	desc table_name;
	desc extended table_name;
	desc formated table_name;

**修改**

	ALTER TABLE old_table_name RENAME TO new_table_name;     修改表名
	ALTER TABLE table_name SET TBLPROPERTIES ('comment' = new_comment);    修改字段名
	......
		
**删除**
	
	DROP TABLE [IF EXISTS] table_name [PURGE]; 

**导入数据**

	从本地文件系统中导入数据到Hive表
        load data local inpath "" overwrite into table table_name;
        hdfs dfs -put
    HDFS上导入数据到Hive表
        load data inpath "" overwrite into table table_name;
    从别的表中查询出相应的数据并导入到Hive表中
        create table table_name as select * from old_table_name;

**导出数据**

	INSERT OVERWRITE [LOCAL] DIRECTORY directory_name [ROW FORMAT row_format] [STORED AS file_format] SELECT ... FROM ... 
	hdfs dfs -get 

**EXPORT/IMPORT**

	EXPORT---------导出
	EXPORT TABLE tablename [PARTITION (part_column="value"[, ...])] TO 'hdfs_path' [ FOR replication('eventid') ]
	注：导出在hadoop上面的目录下
	
	
	IMPORT---------导入
	IMPORT [[EXTERNAL] TABLE new_or_original_tablename [PARTITION (part_column="value"[, ...])]] FROM 'source_path' [LOCATION 'import_target_path']

	实现数据的迁移
























		

	