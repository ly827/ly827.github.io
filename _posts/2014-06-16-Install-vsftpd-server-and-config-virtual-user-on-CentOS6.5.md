---
title: CentOS6.5下安装vsftpd软件及虚拟用户配置
title-en: Install vsftpd server and config virtual user on CentOS6.5
date: 2014-06-16 14:45:25 +0800
categories: linux 
tags: centos vsftpd
layout: post
---

### CentOS6.5下安装vsftpd软件及虚拟用户配置

#### 一：安装vsftpd

查看是否已经安装vsftpd
`rpm -qa | grep vsftpd`
如果没有，就安装，并设置开机启动	
`yum -y install vsftpd`
`chkconfig vsftpd on`

#### 二：基于虚拟用户的配置

所谓的虚拟用户就是没有使用真实的账户，只是通过映射道真实账户和设置权限的目的。虚拟用户不能登录CentOS系统。
修改配置文件
打开 /etc/vsftpd/vsftpd.conf,做如下配置
<pre><code>
	anonymouse_enable=NO  //设置不允许匿名访问
	lacal_enable=YES  //设定本地用户可以访问。注：如果使用虚拟宿主用户，在项目设定为NO的情况下所有虚拟用户将无法访问
	chroot_list_enable=YES  //使用用户不能离开主目录
	ascii_upload_enable=YES
	asci_download_enable=YES  //设定支持ASCII模式的上传下载功能
	pam_service_name=vaftpd  //PAM认证文件名。PAM将根据/etc/pam.d/vaftpd 进行认证
以下这些是关于vsftpd虚拟用户支持的重要配置项，默认vsftpd.conf中不包含这些设定项目，需要自己手动添加
	guest_enable=YES  //设定虚拟用户功能
	guest_username=ftp  //指定虚拟用户的宿主用户，CentOS中已经内置的ftp用户
	user_config_dir=/etc/vsftpd/vuser_conf  //设定虚拟用户个人vsftpd的CentOS FTP服务文件存放路径。存放虚拟用户个性的CentOS FTP服务文件（配置文件名=虚拟用户名）
</code></pre>

#### 三：进行认证

首先，安装Berkeley DB工具，很多人找不到db_load的问题就是没有安装这个包。
	`yum install db4 db4-utils`
然后，创建用户密码文本 /etc/vsftpd/vuser_passwd.txt，注意奇数行市用户名，偶数行市密码
	`test`
	`123456`
接着生成虚拟用户认证db文件
	`db_load -T -t hash -f /etc/vsftpd/vuser_passwd.txt /etc/vsftpd/vuser_passwd.db`
随后，编辑认证文件`/etc/pam.d/vsftpd`，全部注释掉原来语句，在增加以下语句
<pre><code>
	auth required pam_userdb.so db=/etc/vsftpd/vuser_passwd
	acccount required pam_userdb.so db=/etc/vsftpd/vuser_passwd
</code></pre>

最后创建虚拟用户配置文件
<pre><code>
	mkdir /etc/vsftpd/vuser_conf/
	vi /etc/vsftpd/vuser_conf/test  //文件名等于vuser_passwd.txt里面的账户名，否则下面设置无效  
</code></pre>
内容如下
<pre><code>
	local_root=/ftp/www  //虚拟用户根目录，根据实际情况修改
	write_enable=YES
	anon_umask=022
	anon_world_readable_only=NO
	anon_upload_enable=YES
	anon_mkdir_write_enable=YES
	anon_other_write_enable=YES
</code></pre>
设置Selinux
<pre><code>
	setsebool -P ftp_home_dir=1  //设置ftp可以使用home目录
	sersebool -P allow_ftpd_full_access=1 //设置ftp用户可以有所有权限
</code></pre>
设置FTP根目录权限
<pre><code>
	mkdir /ftp/www 
	chmod -R 755 /ftp
	chmod -R 777 /ftp/www
</code></pre>

最新的vsftpd要求对主目录不能有写的权限，所以ftp为755,主目录下面的子目录在设置777权限

#### 四：设置防火墙

打开/etc/sysconfig/iptables
在“-A INPUT -m state --state NEW -m tcp -p -dport 22 -j ACCEPT ”,下面添加：
	`-A INPUT m state --state NEW -m tcp -p -dport 21 -j ACCEPT` 
然后保存，并关闭文件，在终端运行下面的命令，刷新防火墙配置
	`service iptables restart`
OK,运行“Service vsftpd start”,你就可以访问你的FTP服务器了。

#### 五：配置PASV模式

vsftpd默认没有开启PASV模式，现在FTP只能通过PORT模式连接，要开启PASV默认需要通过下面的配置
打开/etc/vsftpd/vsftpd.conf，在末尾添加
<pre><code>
	pasv_enable=YES  //开启PASV模式
	pasv_min_port=40000 
	pasv_max_port=50000  //最大最小端口号
	pasv_promiscuous=YES
	</code></pre>
在防火墙配置内开启40000到40080端口
	`-A INPUT m state --state NEW -m tcp -p dport 40000:40080 -j ACCEPT`
重启iptables和vsftpd

	service iptables restart
	srevice vsftpd restart

现在可以使用PASV模式连接你的FTP服务器了

参考自：<http://www.jb51.net/os/RedHat/112444.html\>
http://jingyan.baidu.com/article/f96699bb9036f7894e3c1be1.html






