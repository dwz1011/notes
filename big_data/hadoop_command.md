# Hadoop 常用命令

### hadoop fs

	# 将文件上传至hadoop的根目录
	hadoop fs -put filename / 		# '/'不是Linux的根目录，表示hadoop的根目录
	# 查看hadoop里的文件
	hadoop fs -ls /
	# 查看hadoop里的文件的内容
	hadoop fs -cat filename
	
	