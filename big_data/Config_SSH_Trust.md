# 配置多台机器SSH相互通信信任 

- 2台机器分别执行`ssh-keygen`生成公钥和密钥

		[root@hadoop-01 ~]# ssh-keygen
		生成.ssh文件夹及id_rsa和id_rsa.pub
		[root@hadoop-01 .ssh]# ll
		total 16
		-rw-------. 1 root root 1675 Dec 13 21:37 id_rsa
		-rw-r--r--. 1 root root  396 Dec 13 21:37 id_rsa.pub
		
- 选取第一台,生成`authorized_keys`文件

		[root@hadoop-01 ~]# cd .ssh
		[root@hadoop-01 .ssh]# cat ./id_rsa.pub >> ./authorized_keys
		
- 将另一台`id_rsa.pub`内容,手动`copy`到第一台的`authorized_keys`文件

		[root@hadoop-02 .ssh]# more id_rsa.pub
		拷贝至`authorized_keys`文件(注意copy时,最好先放到记事本中,将回车去掉,成为一行)
		
- 设置每台机器的权限

		[root@hadoop-01 ~]# chmod 700 -R ~/.ssh
		[root@hadoop-01 ~]# chmod 600 ~/.ssh/authorized_keys 
		
- 将第一台的`authorized_keys` `scp` 给`hadoop-02`(第一次传输,需要输入密码)

		[root@hadoop-01 ~]#  scp authorized_keys root@hadoop-02:/root/.ssh
		
- 配置`/etc/hosts`(两台机器都要配置)

		[root@hadoop-01 ~]# vi /etc/hosts
		将两台机器的IP和机器名都写入
		127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
		::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
		192.168.137.130 hadoop-01
		192.168.137.131 hadoop-02
		
- 验证(每台机器上执行下面的命令,只输入yes,不输入密码,则这两台互相通信了)

		[root@hadoop-01 ~]# ssh root@hadoop-02 date
		[root@hadoop-02 ~]# ssh root@hadoop-01 date
		

