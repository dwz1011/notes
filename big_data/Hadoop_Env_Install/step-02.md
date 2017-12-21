# CentOS6.5 修改主机名

### 编辑 /etc/sysconfig/network
	
	
	[root@localhost ~]# vi /etc/sysconfig/
	networkNETWORKING=yes
	HOSTNAME=hadoop-01
	
### hostname直接命名

	[root@localhost ~]#hostname hadoop-01
	
### 编辑/etc/hosts

	[root@localhost~]# vi/etc/hosts
	127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4:
	:1         localhost localhost.localdomain localhost6 localhost6.localdomain6
	# 添加如下：
	192.168.137.130 hadoop-01

# 配置静态IP和可访问外网

[参考](./CentOS6.5配置静态IP和可访问外网.pdf)

