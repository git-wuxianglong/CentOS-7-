# CentOS-7 常用操作

## 查看系统版本 
	cat /etc/redhat-release 
	
	显示当前路径
	pwd

## 更新系统软件
	yum update

## FTP
	安装vsftpd
	yum install vsftpd

	设置开机启动vsftpd ftp服务
	chkconfig vsftpd on
	
	添加一个名为ftptest的用户 test_ftp为指定目录
	/usr/sbin/adduser -d /opt/test_ftp -g ftp -s /sbin/nologin ftptest

	为该用户设置密码
	passwd ftptest

	启动vsftpd服务
	service vsftpd start

	重启服务
	/sbin/service vsftpd restart
	
## Java
	查找系统已安装的jdk组件
	rpm -qa | grep -E '^open[jre|jdk]|j[re|dk]'

	查看java版本
	java -version

	卸载以前已有的jdk
	yum remove java-1.6.0-openjdk
	yum remove java-1.7.0-openjdk

	解压
	tar -xvf jdk-8u111-linux-x64.tar.gz

	添加到环境变量
	vi /etc/profile
	在export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL下面添加如下代码：
	#jdk
	export JAVA_HOME=/usr/local/jdk1.8.0_111
	export PATH=$JAVA_HOME/bin:$PATH
	export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

	执行配置
	source /etc/profile

	验证是否安装成功
	java -version

## Tomcat
	解压
	tar -xvf apache-tomcat-8.5.15.tar.gz
	
	修改权限
	chown -R tomcat:tomcat apache-tomcat-8.5.15

	启动Tomcat
	/usr/local/apache-tomcat-8.5.15/bin/startup.sh

## Nginx
	先安装以下lib库
	yum install gcc-c++  
	yum install pcre pcre-devel  
	yum install zlib zlib-devel  
	yum install openssl openssl--devel 

	是否已经安装有nginx
	find -name nginx

	卸载
	yum remove nginx

	下载
	cd /usr/local
	wget http://nginx.org/download/nginx-1.7.4.tar.gz

	解压
	tar -zxvf nginx-1.7.4.tar.gz

	进入到解压出来的目录
	cd nginx-1.7.4

	安装（默认安装在/usr/local/nginx）
	./configure
	make
	make install

	查看nginx的安装目录
	whereis nginx

	启动
	/usr/local/nginx/sbin/nginx

## 文件操作
	删除文件
	rm file.txt

	删除home目录下的test文件夹 
	rm -rf /home/test
	
	移动文件
	mv file.txt /home/local 
	
	复制文件 
	cp apache-tomcat-8.5.20.tar.gz /home/javaweb/

	解压文件
	tar -xvf jdk-8u111-linux-x64.tar.gz 
	
	搜索文件夹（搜索名为java的文件夹） 
	find / -name java 
	
	重命名
	mv 旧文件名 新文件名

## mysql 
	安装
	yum install mysql 
	yum install mysql-server 
	yum install mysql-devel 
	
	安装mysql和mysql-devel都成功，但是安装mysql-server失败 
	[root@yl-web yl]# yum install mysql-server
	Loaded plugins: fastestmirror
	Loading mirror speeds from cached hostfile
	 * base: mirrors.sina.cn
	 * extras: mirrors.sina.cn
	 * updates: mirrors.sina.cn
	No package mysql-server available.
	Error: Nothing to do 
	
	有两种解决办法： 
	方法一：安装mariadb 
	yum install mariadb-server mariadb 
	
	mariadb数据库的相关命令是：
	systemctl start mariadb  #启动MariaDB
	systemctl stop mariadb  #停止MariaDB
	systemctl restart mariadb  #重启MariaDB
	systemctl enable mariadb  #设置开机启动
	
	先启动数据库: 
	systemctl start mariadb 
	然后就可以正常使用mysql了
	mysql -u root -p
	
	方法二：官网下载安装mysql-server 
	# wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
	# rpm -ivh mysql-community-release-el7-5.noarch.rpm
	# yum install mysql-community-server
	
	安装成功后重启mysql服务 
	# service mysqld restart 
	
	------------------------------------------------------------------------- 
	
	初次安装mysql，root账户没有密码， 
	MariaDB [(none)]> set password for 'root'@'localhost' =password('要设置的密码'); 
	Query OK, 0 rows affected (0.00 sec) 
	不需要重启数据库即可生效
	
	查看Mysql安装位置
	whereis mysql 
	
	查看mysql版本 
	MariaDB [(none)]> status
	
	配置 
	vi /etc/my.cnf 
	最后加上编码配置 
	default-character-set =utf8 
	这里的字符编码必须和/usr/share/mysql/charsets/Index.xml中一致。 
	<charset name="utf8">
	  <family>Unicode</family>
	  <description>UTF-8 Unicode</description>
	  <alias>utf-8</alias>
	  <collation name="utf8_general_ci"     id="33">
	   <flag>primary</flag>
	   <flag>compiled</flag>
	  </collation>
	  <collation name="utf8_bin"            id="83">
	    <flag>binary</flag>
	    <flag>compiled</flag>
	  </collation>
	</charset>

	
	远程连接设置 
	把在所有数据库的所有表的所有权限赋值给位于所有IP地址的root用户。
	MariaDB [(none)]> grant all privileges on *.* to root@'%'identified by 'password';
	Query OK, 0 rows affected (0.00 sec) 
	
	如果是新用户而不是root，则要先新建用户
	create user 'username'@'%' identified by 'password';
	
