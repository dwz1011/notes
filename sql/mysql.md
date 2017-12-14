# mysql

* [mysql](http://www.mysql.com/)

#### 安装mysql
    ubuntu
    sudo apt-get install mysql-server
    sudo apt-get isntall mysql-client
    mac
    1.先安装Homebrew
    2. brew install mysql
    
#### 启动mysql
  
  sudo service mysql start
    
#### 检查mysql是否安装成功
  
    sudo netstat -ant | grep 3306
    或sudo ps -ef|grep mysql
    
#### 关闭mysql
  
  sudo service mysql stop
    
#### 登录mysql

    sudo mysql -u root (-p -h -P)
    *  -u表示选择登录的用户名
    *  -p表示登录的用户密码，若在安装时未设置，则无需输入-p
    *  -h表示登录的主机地址
    *  -P表示登录的端口号

	解决 Ubuntu 16.04 上不能直接用 root 用户登录
	sudo mysql -u root -e "update mysql.user set plugin='mysql_native_password' WHERE User='root’;


    	
    
    
## mysql的简单操作
显示数据库
>show databases;

创建数据库
>create database db_name;

创建用户
>create USER 'username'@'host' IDENTIFIED BY 'password';

授权
>GRANT privileges ON db_name.* TO 'username'@'host' ;
>
>flush privileges;

撤销用户权限
>REVOKE privilege ON db_name.* FROM 'username'@'host'; 
>
>flush privileges;

更改用户密码
>UPDATE user SET password=PASSWORD("new password") WHERE user='username';
>
>flush privileges;

删除用户
>DROP USER 'username'@'host';

删除数据库
>drop database db_name;