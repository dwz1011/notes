# Hive数据存储


### 行式存储和列式存储

行式存储

	特点：
		保证一条记录里面的所有字段能够存放在同一个hdfs的block里
	
	优点：
		当查询所有(select * from tbname)时，能直接查询出来
		
	缺点：
		不同列的字段类型不同，压缩性能差，空间利用率差
		只查询某几列数据的时候，必须先把所有数据读取进来，在提取所需的几列，结果会增加磁盘IO，效率低
	
列式存储
	
	优点：
		每一列的数据类型是一样的，所有可以采用一定压缩方式，压缩性能好
		只查询某几列数据的时候，对于不需要的列，可以直接跳过
	
	缺点：
		当查询所有(select * from tbname)时，对于行的数据必然会重组
		
### 存储类型		

- TestFile(行式存储，默认)

	缺点：所有字段都是string类型，解析麻烦
	
	查看表的详细信息： `desc formatted tbname `
	
	![TestFile](./pic/testfile_info.png)
	
	
- SequenceFile(行式存储)

	![SequenceFile](./pic/SequenceFile.png)
	
	类似于二进制，是key-value存储的
	
	压缩只能压缩value，对比于TestFile，多了一些冗余信息，但是随机读写要好一些
	
- RcFile(行列混合) --> Available in Hive `0.6.0` and later

	![RcFile](./pic/rcfile.png)	
	
	先行后列	
	
	Row Group 只有4M
	
- ORCFile(优化过后的存储) --> Available in Hive `0.11.0` and later

	![orc](./pic/orc.png)
	
	orcFile是对rcFile的优化，其将文件分为多个stripe，默认每个stripe为250M，在stripe中默认对每10000行进行一个索引（index data）。这个索引记录了这些行中的max和min，对于查询，有更明显的优化

	指定ORC文件格式
		
		1. CREATE TABLE ... STORED AS ORC
		2. ALTER TABLE ... [PARTITION partition_spec] SET FILEFORMAT ORC
		3. SET hive.default.fileformat=Orc

	参数在TBLPROPERTIES中配置
	
	|    		key					|	defult	|        notes         |
	|:------------------------:|:------:|:---------------------------|
	|	  orc.compress			|  ZLIB   |high level compression (one of NONE, ZLIB, SNAPPY) |
	|    orc.compress.size     | 262144  |number of bytes in each compression chunk|
	|    orc.stripe.size       |67108864|number of bytes in each strip|
	|   orc.row.index.stride   | 10000 |number of rows between index entries (must be >= 1000)|
	|   orc.create.index       | true  |whether to create row indexes|
	| orc.bloom.filter.columns |""|comma separated list of column names for which bloom filter should be created|
	|    orc.bloom.filter.fpp  | 0.05   |false positive probability for bloom filter (must >0.0 and <1.0)|
	
	
- Parquet(列式存储) --> Available in Hive `0.13.0` and later









	
	
### 相同数据，分别以TestFile、SequenceFile、RcFile、ORC存储的比较
	
**TestFile(默认)**
	
![page_views](./pic/result-0.png)
	
**SequenceFile**

	create table page_views_seq( 	track_time string, 	url string, 	session_id string, 	referer string, 	ip string, 	end_user_id string, 	city_id string 	)ROW FORMAT DELIMITED FIELDS TERMINATED BY “\t” 	STORED AS SEQUENCEFILE;
	
	insert into table page_views_seq select * from page_views; 
		
![page_views_seq](./pic/sequencefile_info.png)
	
**RcFile**
	
	create table page_views_rcfile(	track_time string,	url string,	session_id string,	referer string,	ip string,	end_user_id string,	city_id string	)ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"	STORED AS RCFILE;
		
	insert into table page_views_rcfile select * from page_views; 

![page_views_rcfile](./pic/rcfile_info.png)
	
**ORC**
	
	create table page_views_orc	ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"	STORED AS ORC 	TBLPROPERTIES("orc.compress"="NONE")	as select * from page_views;
		
![page_views_orc](./pic/orc_info.png)
	
**ORC+Zlip**

	create table page_views_orc_zlib	ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"	STORED AS ORC 	TBLPROPERTIES("orc.compress"="ZLIB")	as select * from page_views;
	
![page_views_orc_zlib](./pic/orc_info_zlib.png)

**Parquet**
	
	create table page_views_parquet	ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"	STORED AS PARQUET 	as select * from page_views;

![page_views_parquet](./pic/parquet.png)

**Parquet+gzip**	

	set parquet.compression=gzip;	create table page_views_parquet_gzip	ROW FORMAT DELIMITED FIELDS TERMINATED BY "\t"	STORED AS PARQUET 	as select * from page_views;
	
![page_views_parquet_gzip](./pic/parquet_gzip.png)



























	
		


