#SQL语法(mysql)

###一、数据库操作:
**1.查看数据库**

	mysql> show databases;

**2.创建数据库**

	create database db_name;　　//db_name为数据库名

	mysql> create database dwz_test;
	
**3.使用数据库**

	use db_name;
	
**4.删除数据库**

	drop database db_name;
	
###二、表的操作
**1.创建表**

	CREATE TABLE tb_name(
	
	id TINYINT UNSIGNED NOT NULL AUTO_INCREMENT,      //id值，无符号、非空、递增——唯一性，可做主键。
	
	name VARCHAR(60) NOT NULL,

	score TINYINT UNSIGNED NOT NULL DEFAULT 0,　　　　//设置默认列值

	PRIMARY KEY(id)

	)ENGINE=InnoDB　　　　//设置表的存储引擎，一般常用InnoDB和MyISAM；InnoDB可靠，支持事务；MyISAM高效不支持全文检索

	DEFAULT charset=utf8;　　//设置默认的编码，防止数据库中文乱码

如果有条件的创建数据表还可以使用   >CREATE TABLE IF NOT EXISTS tb_name(........

**2.复制表**

	CREATE TABLE tb_name2 SELECT * FROM tb_name;
	
	或者部分复制：
	
	CREATE TABLE tb_name2 SELECT id,name FROM tb_name;

**3.创建临时表**

	CREATE TEMPORARY TABLE tb_name(这里和创建普通表一样);

**4.查看数据库中可用的表**

	SHOW TABLES;

**5.查看表的结构**

	DESCRIBE tb_name;

	也可以使用：

	SHOW COLUMNS in tb_name; 　　　　//from也可以
	
**6.查看创建表的语法**

	SHOW CREATE tb_name;

**7.删除表**

	DROP [ TEMPORARY ] TABLE [ IF EXISTS ] tb_name[ ,tb_name2.......];

	实例：

	DROP TABLE IF EXISTS tb_name;

**8.表重命名**

	RENAME TABLE name_old TO name_new;

	还可以使用：

	ALTER TABLE name_old RENAME name_new;
	
**9.更新表结构**

	ALTER TABLE tb_name ADD[CHANGE,RENAME,DROP] ...要更改的内容...

###三、数据的增删改查

**1.插入数据**

	<a>插入数据

	INSERT INTO tb_name(id,name,score)VALUES(NULL,'张三',140),(NULL,'张四',178),(NULL,'张五',134);

	这里的插入多条数据直接在后边加上逗号，直接写入插入的数据即可；主键id是自增的列，可以不用写。

	<b>插入检索出来的数据

	INSERT INTO tb_name(name,score) SELECT name,score FROM tb_name2;

**2.删除数据**
	
	DELETE FROM tb_name WHERE columnName='';
	
**3.更新数据**

	UPDATE tablename SET columnName=NewValue [ WHERE condition ]
	
**4.查询数据**
	


	

