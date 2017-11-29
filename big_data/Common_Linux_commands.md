# [Linux 命令](http://man.linuxde.net/)

**pwd**

	查看当前目录

**cd**

	cd  和 cd ~			   进用户的家目录 标识是~   /home/用户名称
	cd /lib/alsa/init/ 		进目录
	cd ../        			退回上一层目录
	cd ../../     			退回上两层目录
	cd -          			退回上一次的目录
	
**hostname**

	hostname         	查看机器名称  (带后缀)
	hostname xxx  	 	设置机器名称
	hostname -i      	查看机器的IP

**ls**

	ls  			文件或文件夹名
	ls -l   		文件或文件夹详细列表
	ll    			等价 ls -l
	ll -a  			隐藏文件  以.为开头
	ll -h  			大小
	
**查看文件内容**

	cat  test.log		直接输出文件内容
	more test.log		一页页往下翻
	less test.log		一页页往上翻   
	tail -F xxx.log 	实时查看log文件 
	
**vi**

	vi bigdata01.txt 进入命令模式
	按i键 进入编辑模式
	......
	......
	.....
	按Esc键，退出编辑模式
	按shift+: 进入尾行模式,输入wq 保存文件并关闭
	
	gg			第一行第一个字母
	G			最后一行第一个字母
	shift+$ 	行的最后一个字母
	dd			删除当前行
	dG			删除光标以下的所有行
	ndd			删除光标所在的向下n行
	i 插入--> 编辑模式 
	编辑模式:
	ECS  退出-->尾行模式
	
	尾行模式:
	:q		退出
	vi		编辑器
	:w		保存修改的内容
	:wq		保存并退出
	:q!		强制退出，当对文本内容作了修改而不想要保存时
	:w!		强制保存，当没有文本的写权限时
	
	:set number或:set nu				显示行号
	:set nonumber或:set nonu		取消显示行号
	:/内容/ 或/内容					查找指定内容    //n将光标移动到下一个目标     //N上一个
	:n									跳转到第n行
	

**alias**	
	
	别名 http://blog.51cto.com/aixecc/787590
	
**touch**

	创建一个空白的文件

**mkdir**
	
	创建文件夹
	mkdir xxx  
	mkdir -p xxx1/xxx2
	
**rm**

	rm 文件名称
	rm -f 文件名称
	rm -r 文件夹
	rm -rf 文件夹
	
	-r 指的是递归 (目录)
	-f 指的是强制 不提示
	
**ps/netstat**	

	[root@hadoop001 ~]# ps -ef | grep ssh
	root      1450     1  0 13:51 ?        00:00:00 /usr/sbin/sshd
	root      1786  1450  0 13:52 ?        00:00:00 sshd: root@pts/0 
	root     22104  1790  0 15:13 pts/0    00:00:00 grep ssh
	
	第一列 root 用户
	第二列 pid  进程号
	最后一列    ssh运行的命令
	
	[root@hadoop001 ~]# netstat -nlp | grep 1450
	tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      1450/sshd           
	tcp        0      0 :::22                       :::*                        LISTEN      1450/sshd 
	
	查看当前的进程的端口号
	
	问题: 665端口号 ,查进程号
	netstat -nlp | grep 22
	ps -ef | grep 1450
	
**搜索**

	find / -name "ssh*" 全局搜索   (重要)

	locate java
	
	which  java
	
**echo**
	
	echo "xxxx" 
	echo "xxxx" > test.log
	echo "xxxx" >> test.log
	
**wget**

	wget http://ftp.iij.ad.jp/pub/db/mysql/Downloads/MySQL-5.6/mysql-5.6.35-linux-glibc2.5-x86_64.tar.gz 
	
**yum**

	yum -y install xxx

**mv/cp**

	mv 文件1/文件夹  文件11/文件夹
	mv test.log test.log20170813   原文件不存在的
	cp test.log20170813 test.log20170814   原文件存在的
	
**tar**

	tar -czf test.tar.gz test.log 压缩
	tar -xzvf test.tar.gz   解压
	
**磁盘 内存**

	[root@hadoop001 ~]# df -h
	Filesystem      Size  Used Avail Use% Mounted on
	/dev/sda3        38G  3.2G   33G   9% /
	tmpfs          1000M  228K 1000M   1% /dev/shm
	/dev/sda1       124M   34M   84M  29% /boot
	/dev/sr0        4.2G  4.2G     0 100% /media/CentOS_6.5_Final
	[root@hadoop001 ~]# 
	[root@hadoop001 ~]# 
	[root@hadoop001 ~]# free -m
	             total       used       free     shared    buffers     cached
	Mem:          1998       1197        801          0        118        605
	-/+ buffers/cache:        473       1525
	Swap:         1999          0       1999	
**top**

	查看机器负载load	
	
**用户和用户组**
	
	用户
	[root@hadoop001 ~]# ll /usr/sbin/user*
	-rwxr-x---. 1 root root 103096 Dec  8  2011 /usr/sbin/useradd
	-rwxr-x---. 1 root root  69560 Dec  8  2011 /usr/sbin/userdel
	-rwxr-x---. 1 root root  98680 Dec  8  2011 /usr/sbin/usermod
	创建用户
	[root@hadoop001 ~]# useradd ruoze
	创建已存在的用户
	[root@hadoop001 ~]# useradd dwz
	useradd: user 'dwz' already exists
	[root@hadoop001 ~]# id ruoze
	uid=515(ruoze) gid=515(ruoze) groups=515(ruoze)
	给用户设置密码
	[root@hadoop001 ~]# passwd ruoze
	查看已有的用户
	[root@hadoop001 ~]# ll /home/
	total 8
	drwx------. 27 jepson jepson 4096 Aug 13 14:31 dwz
	drwx------.  4 ruoze  ruoze  4096 Aug 16 21:38 ruoze
	
	用户组
	[root@hadoop001 ~]# ll /usr/sbin/group*
	-rwxr-x---. 1 root root 54968 Dec  8  2011 /usr/sbin/groupadd
	-rwxr-x---. 1 root root 46512 Dec  8  2011 /usr/sbin/groupdel
	-rwxr-x---. 1 root root 50800 Dec  8  2011 /usr/sbin/groupmems
	-rwxr-x---. 1 root root 61360 Dec  8  2011 /usr/sbin/groupmod
	创建用户组
	[root@hadoop001 ~]# groupadd bigdata
	用户组的文件
	[root@hadoop001 ~]# cat /etc/group
	将dwz用户添加到bigdata组
	[root@hadoop001 ~]# usermod -a -G bigdata jepson
	
	给一个普通用户添加sudo权限
	vi /etc/sudoers
	#在 root  	ALL=(ALL)       ALL 下面添加一行
	dwz  	ALL=(ALL)       ALL
	
**切换用户su**

	[root@hadoop001 ~]# su - dwz
 	
	[dwz@hadoop001 ~]$ su -  
	Password:  #输入root密码，切到root 	
	
**修改文件权限**

	chmod  750 test.log    rwxr-x---
	chmod -R 777 ruoze 
	chown  dwz:dwz test.log
	chown -R dwz:dwz ruoze
	chmod/chown 
	-R 递归  文件夹
		
	
	
	
	
	
	
	
	
	
	
	
	