# HDFS命令


### dfsadmin

- 查看文件系统的基本信息和统计信息

		hdfs dfsadmin -report
		
- 安全模式命令

		hdfs dfsadmin -safeadmin enter|leave|get|wait
		安全模式是NameNode的一种状态，在这种状态下，NameNode不接受对名字空间的更改（只读）；不复制或删除块。
		
**多台机器的磁盘存储分布不均匀**

解决方案:

	1. 不加新机器，原机器的磁盘分布不均匀:
	   [hadoop@hadoop-01 ~]$ hdfs dfsadmin -setBalancerBandwidth  52428800
	   Balancer bandwidth is set to 52428800
	  
	   [hadoop@hadoop-01 sbin]$ ./start-balancer.sh  
	   等价
	   [hadoop@hadoop-01 sbin]$ hdfs balancer 
	   

	   Apache Hadoop集群环境:  shell脚本每晚业务低谷时调度
	   CDH集群环境: 忽略

	2. 加新机器，原机器的磁盘比如450G(500G),现在的新机器磁盘规格是5T
	    在业务低谷时，先将多台新机器加入到HDFS，做DN；
	    然后选一台的DN下架掉，等待hdfs自我修复块，恢复3份（网络和io最高的，也是最有风险性的）
	    
	    
**一台机器的多个磁盘分布不均匀**

解决方案:    
	    
	1.无论加不加磁盘，且多块磁盘的分布不均匀  
	hdfs diskbalancer -plan node1.mycluster.com
	hdfs diskbalancer -execute /system/diskbalancer/nodename.plan.json
	    

