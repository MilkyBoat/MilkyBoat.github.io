---
title: 当你买了台新的ubuntu服务器(2)
date: 2020-05-07 15:46:02
tags: 
    - 服务器 
    - 运维
    - 服务器配置
---

# 当你买了台新服务器-ubuntu服务器常见设置


## 安装各种服务器软件

#### nginx

```bash
sudo apt-get install nginx
```
安装完成之后，直接使用ip地址访问服务器，就可以看到nginx的默认欢迎页面了。

> 如果安装报错比如80端口占用，说明此前安装了其它网络服务器，建议全都关掉再安装，并且让nginx独占80端口作为反向代理服务器

安装完成之后配置反向代理，使用whereis nginx命令找到配置文件所在文件夹，找到nginx.conf后进行如下修改（记得备份该文件）
```bash
$ whereis nginx
# 显示nginx相关目录，挨个看看找到nginx.conf的位置，不同的系统可能不一样
$ cd /etc/nginx  # 我的nginx配置在这里，不同版本会有区别
$ sudo cp nginx.conf nginx.conf.bk  # 备份
$ sudo vi nginx.conf
```
我的配置如下
```
############### nginx.conf ###############
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
	use epoll;
	worker_connections 51200;
	multi_accept on;
}

http {

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	server_tokens off;

	server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /data/wwwlogs/nginx/access.log;
	error_log /data/wwwlogs/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;
	gzip_disable "msie6";

	gzip_vary on;
	gzip_proxied any;
	gzip_comp_level 6;
	gzip_buffers 16 8k;
	gzip_http_version 1.1;
	gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

############# basic web #############

	server {
		listen 80;  # 监听80端口
		# server_name milkyship.cn  www.milkyship.cn;
		access_log /data/wwwlogs/nginx/access.log combined;  # 日志文件位置
		root /data/wwwroot;  # 网页根目录
		index index.html index.php index.jsp; # 主页文件

		#error_page 404 /404.html;
		#error_page 502 /502.html;

		location /nginx_status {
			stub_status on;
			access_log off;
			allow 127.0.0.1;
			deny all;
		}

		location ~ .*\.(gif|jpg|jpeg|png|bmp|mp3|wma|wmv|swf|flv|mp4|mkv|avi|ico|txt|pdf|rar|zip|7z|gz)$ {
			expires 30d;
			access_log off;
		}

		location ~ .*\.(js|css)?$ {
			expires 7d;
			access_log off;
		}

		location ~ .*\.[(php)(html)(htm)]$ {
			proxy_pass http://127.0.0.1:8090;  # 转发Apache服务器
			proxy_next_upstream http_502 http_504 error timeout invalid_header;
			proxy_set_header Host $host;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
		}

		location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
			deny all;
		}

		location ~.*$ {
      			proxy_pass http://127.0.0.1:8080;  # 转发TomCat服务器
			# include proxy.conf;
		}
	}



	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}
```
最后使用
```bash
$ service nginx restart
```
重启nginx服务器，如果报错，检查刚刚设置过程中有没有拼写错误，设置的网站根目录有没有建立好

#### apache

```bash
sudo apt-get install apache2
```
> ！！！下方操作前记得备份！！！
> 
安装完成后在 /etc/apache2/ports.conf 中修改所有的80到8090，443到8091以避开nginx监听端口

再在 /etc/apache2/sites-enabled/000-default.conf 中修改DocumentRoot到个人的网站根目录

最后使用
```bash
$ service apache2 restart
```
重启apache服务器，如果报错，检查刚刚设置过程中有没有拼写错误，设置的网站根目录有没有建立好

#### tomcat

```bash
$ sudo apt-get install tomcat8
$ service tomcat8 start
```
这里没什么配置要改，前面该改的都弄好了

现在服务器上有3个web服务器在运行，nginx监听80端口做前台反向代理，apache监听8090端口做php服务器，tomcat监听8080端口做java服务器，并且使用的时候使用nginx正则匹配url分配流量到正确的服务器上（这个还需要后面进一步设置）

#### ftp

```bash
$ sudo apt-get install vsftpd
$ sudo vi /etc/vsftpd.conf  # 进入配置
```
文件尾追加：
```
#配置ftp服务器的上传下载文件所在的目录。
local_root=/home/ftpfile
```
如果想要使用管理员权限登入以访问非ftp指定目录，在配置文件中找到如下属性，如下修改：
```
chroot_local_user=YES
chroot_list_enable=YES
# (default follows) 允许chroot_list文件中配置的用户登录此ftp服务器。
chroot_list_file=/etc/vsftpd.chroot_list # 本行需要新增
```
随后配置允许管理员权限登入的用户，每行写入一个允许管理员权限登入的用户的用户名
```bash
$ sudo vi /etc/vsftpd.chroot_list
```
最后重启ftp服务
```bash
$ service vsftpd restart
```
此时便可以使用ftp工具登录服务器进行文件传输

