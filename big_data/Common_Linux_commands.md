# [Linux 命令](http://man.linuxde.net/)

**pwd**

	查看当前目录

**cd**

	cd  和 cd ~		        进用户的家目录 标识是~   /home/用户名称
	cd [表情]b/alsa/init/ 		进目录
	cd ../        			退回上一层目录
	cd ../../     			退回上两层目录
	cd -          			退回上一次的目录
	
**hostname**

	hostname         	查看机器名称
	hostname xxx  	 	设置机器名称（临时）
	hostname -i      	查看机器的IP

**ls**

	ls  			文件或文件夹名
	ls -l   		文件或文件夹详细列表
	ll    			等价 ls -l
	ll -a  			隐藏文件  以.为开头
	ll -h  			文件大小 
	
**查看文件内容**

	cat  test.log		直接输出文件内容
	more test.log		一页页往下翻
	less test.log		一页页往上翻   
	tail -F xxx.log 	实时查看log文件
	tail -f xxx.log 	查看log文件
	# 注：
		参数区别： -F==》-f --retry
	
**查看文件(文件夹)的大小**
	
	du -sh filename
	
**vi**

	vi test.txt 	        进入命令模式
	gg			第一行第一个字母
	G			最后一行第一个字母
	shift+$ 	        行的最后一个字母
	dd			删除当前行
	dG			删除光标以下的所有行
	ndd			删除光标所在的向下n行
	i 			插入--> 编辑模式 
	ECS  		        退出编辑模式-->尾行模式
	
	尾行模式:
	:q		退出
	vi		编辑器
	:w		保存修改的内容
	:wq		保存并退出
	:q!		强制退出，当对文本内容作了修改而不想要保存时
	:w!		强制保存，当没有文本的写权限时
	
	:set number或:set nu		        显示行号
	:set nonumber或:set nonu		        取消显示行号
	:/内容/ 或/内容				查找指定内容    //n将光标移动到下一个目标     //N上一个
	:n					跳转到第n行
	

**alias**	
	
	别名 http://blog.51cto.com/aixecc/787590
	临时设置，重启后无效
		alias 别名='原命令 参数'
		例：alias vi='vim'
	永久设置，需要重启后才生效
		更改~/.bashrc或/etc/bashrc
	
**touch**

	创建一个空白的文件

**mkdir**
	
	创建文件夹
	mkdir xxx  
	mkdir -p xxx1[表情]x2
	
**rm**

	删除文件或文件夹
	rm 文件名称
	rm -f 文件名称	
	rm -r 文件夹
	rm -rf 文件夹
	
	-r 指的是递归 (目录)
	-f 指的是强制 不提示
	
**ps/netstat**	

	[root@hadoop-01 ~]# ps -ef|grep ssh

	查看当前的进程的端口号
	[root@hadoop-01 ~]# netstat -nlp|grep 1450
	[root@hadoop-01 ~]# ps -ef|grep 1450
	
	根据端口号查看进程
	netstat -nlp|grep 22
	
**搜索**

	find / -name 'name' 		全局搜索

	locate java
	
	which  java
	
**echo**
	
	echo "xxxx" 					输出
	echo "xxxx" > test.log				覆盖
	echo "xxxx" >> test.log				追加
	
**wget**
	
	下载
	wget http://ftp.iij.ad.jp/pub/db/mysql/Downloads/MySQL-5.6/mysql-5.6.35-linux-glibc2.5-x86_64.tar.gz 
	
**yum**

	yum -y install xxx
	
**搜索、卸载rpm**
	
	rpm -qa |grep xxx
	
	rpm --nodeps -e xxx
	# 注:
		--nodeps 不验证包的依赖关系 强行卸载

**mv、cp**

	mv 移动
	cp 复制
	
	mv test.log test.log20170813 
	mv 文件1/文件夹  文件11/文件夹
	  
	cp test.log20170813 test.log20170814 
	cp -r 文件1/文件夹  文件11/文件夹

	
**tar、zip**

	tar -czf test.tar.gz test.log 		压缩
	tar -xzvf test.tar.gz   		解压
	
	zip(压缩)/unzip(解压)
	
**磁盘和内存**

	查看磁盘大小及其使用情况
	[root@hadoop-01 ~]# df -h
	查看内存大小及其使用情况
	[root@hadoop-01 ~]# free -m
	
**用户和用户组**
	
	用户
	[root@hadoop-01 ~]# ll /usr/sbin/user*
	-rwxr-x---. 1 root root 103096 Dec  8  2011 /usr/sbin/useradd
	-rwxr-x---. 1 root root  69560 Dec  8  2011 /usr/sbin/userdel
	-rwxr-x---. 1 root root  98680 Dec  8  2011 /usr/sbin/usermod
	创建用户
	[root@hadoop-01 ~]# useradd ruoze
	创建已存在的用户
	[root@hadoop-01 ~]# useradd dwz
	useradd: user 'dwz' already exists
	[root@hadoop-01 ~]# id dwz01
	uid=515(dwz01) gid=515(dwz01) groups=515(dwz01)
	给用户设置密码
	[root@hadoop-01 ~]# passwd dwz01
	查看已有的用户
	[root@hadoop-01 ~]# ll /home/
	total 8
	drwx------. 27 dwz dwz 4096 Aug 13 14:31 dwz
	drwx------.  4 dwz01  dwz01  4096 Aug 16 21:38 dwz01
	
	用户组
	[root@hadoop-01 ~]# ll /usr/sbin/group*
	-rwxr-x---. 1 root root 54968 Dec  8  2011 /usr/sbin/groupadd
	-rwxr-x---. 1 root root 46512 Dec  8  2011 /usr/sbin/groupdel
	-rwxr-x---. 1 root root 50800 Dec  8  2011 /usr/sbin/groupmems
	-rwxr-x---. 1 root root 61360 Dec  8  2011 /usr/sbin/groupmod
	创建用户组
	[root@hadoop-01 ~]# groupadd bigdata
	用户组的文件
	[root@hadoop-01 ~]# cat /etc/group
	将dwz用户添加到bigdata组
	[root@hadoop-01 ~]# usermod -a -G bigdata dwz
	
	给一个普通用户添加sudo权限
	vi /etc/sudoers
	#在 root  	ALL=(ALL)       ALL 下面添加一行
	dwz  	ALL=(ALL)       ALL
	
**切换用户su**

	[root@hadoop-01 ~]# su - dwz
 	
	[dwz@hadoop-01 ~]$ su -  
	Password:  #输入root密码，切到root 	
	
**修改文件权限**

	chmod  750 test.log    rwxr-x---
	chmod -R 777 test
	chown  dwz:dwz test.log
	chown -R dwz:dwz test
	
	-R 递归  文件夹
		
**软连接**
	
	ln -s 原始目录 目标目录
