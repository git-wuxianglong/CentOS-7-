# CentOS-7-

##更新系统软件  	yum update

##FTP
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
	
##Java
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

##Tomcat
	解压
	tar -xvf apache-tomcat-8.5.15.tar.gz
	
	修改权限
	chown -R tomcat:tomcat apache-tomcat-8.5.15

	启动Tomcat
	/usr/local/apache-tomcat-8.5.15/bin/startup.sh

##Nginx
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

##文件操作
	删除文件
	rm file.txt

	删除home目录下的test文件夹 
	rm -rf /home/test
	
	移动文件
	mv file.txt /home/local

	解压文件
	tar -xvf jdk-8u111-linux-x64.tar.gz
