---
title: Ubuntu常用软件安装
catalog: true
date: 2018-04-07 12:39:46
subtitle: "记录软件安装过程"
header-img: "back1.jpg"
tags:
- Linux
- Ubuntu
---
> 常用软件安装
-------------------------

# java

> download from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html), I use java8
```bash
# before you install java, you should uninstall 'openjdk'
sudo apt-get remove openjdk*
sudo apt-get autoremove
# unzip file on '/usr/progrm' directory
tar xzvf jdk-8u151-linux-x64.tar.gz -C/usr/program

# set environment
sudo vi ~/.bashrc 
```
> add flow on the last (将下面添加到.bashrc文件的末尾)
```bash
#set oracle jdk environment
export JAVA_HOME=/usr/program/jdk-8u151-linux-x64
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH 
```
> active the setting (激活配置)
```bash
source ~/.bashrc
# check
java -version
```
# tomcat

> downlaod from [Apache tomcat](https://tomcat.apache.org/download-80.cgi), I use tomcat8
```bash
# upzip file on directory '/usr/program'
tar xzvf apache-tomcat-8.5.29.tar.gz -C/usr/program
```
> set 'startup.sh' and 'shutdown.sh' on 'apache-tomcat-8.5.29/bin' directory
>
>insert flow before last line of 'startup.sh'
```
#set java environment
export JAVA_HOME=/usr/program/jdk-8u151-linux-x64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.29
```
>
>insert flow before last line of 'shutdown.sh'
```
#set java environment
export JAVA_HOME=/usr/program/jdk-8u151-linux-x64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.29
```
>
> run tomcat
```
# if you has changed the port ,like '80', please use 'sudo' run this command
/usr/tomcat/apache-tomcat-8.5.29/bin startup.sh
```
>
> stop tomcat
```
/usr/tomcat/apache-tomcat-8.5.29/bin shutdown.sh
```

# nodejs
> download from [NodeJs.org](https://nodejs.org/en/download/)-- select source code
```
tar xzvf node-v8.11.1.tar.gz -C/usr/program
```
> 
> set environment
```
vi /etc/profile
```
>
> add flow on the last
```
export NODE_HOME=/usr/program/node-v8.11.1
export PATH=$PATH:$NODE_HOME/bin
export NODE_PATH=$NODE_HOME/lib/node_modules
```
>
> active file
```
source /etc/profile
```
>
> check
```
node -v
npm -v
```
>
> if there are some permission like
> "checkPermissions Missing write access to /home/hcy/node/lib/node_module"
```
sudo chown -R $(whoami) /home/hcy/node/
```

# shadowsocks (ss)
```bash
# install shadowsocks
sudo apt-get update 
sudo apt-get install python-gevent python-pip
pip install shadowsocks

# set config
sudo vi /usr/program/ss.json
# copy flow lines
{
    "server":"23.106.129.100",
    "server_port":1994,
    "local_port":1080,
    "password":"your service psd",
    "timeout":600,
    "method":"rc4-md5"
}
# save ss.json ,then run ss
sslocal -c /etc/ss.json
```
there are two method,  1. PAC  2. global
open system setting => Network => Network proxy
 1. PAC , download pac file from [here](/download/autoproxy.zip), unzip you will get a 'autoproxy.pac' file
 on the 'Network proxy' setting page, select 'Automatic'
 type the  'autoproxy.pac' path. like
 ```bash
 file:///usr/program/autoproxy.pac
 ```
 then Apply system wide

 2. global
 on the 'Network proxy' setting page, select 'Manual'
 on the 'Socks Host' line, type like flow, '1080' is your 'local_port' in ss.json file
host: 127.0.0.1
port: 1080
 then Apply system wide

# nginx
```bash
# install 
sudo apt-get update
sudo apt-get install nginx
# show and check config file
nginx -t
# start
nginx -c /etc/nginx/nginx.conf
# stop
pkill -9 nginx
```
> the config file is on 'nginx.conf'
> config the 80 proxy to 8080(tomcat)
```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	root /var/www/html;
	index index.html index.htm
	server_name 127.0.0.1:8080;
	location / {
		proxy_pass http://127.0.0.1:8080;
	}
}
```

> config the static file to some subdomain,
> like in the director  /www/api
```
server {
    listen 80;
    server_name api.domains.com;
    root /www/api;
    index index.html;
    location / {
	root /www/api;
	index index.html;
    }
}
```

> config the proxy some request , like google map

```
server {
	listen	80;
	server_name gmap.domain.com;
	client_max_body_size 20M;
	fastcgi_buffers 4 4K;
	sendfile on;
	send_timeout 600s;
	
	location /kh {
		proxy_redirect off;
		proxy_set_header Host khm2.google.com;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://khm2.google.com;
	}
        location /vt {
                proxy_redirect off;
                proxy_set_header Host mt0.google.cn;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://mt0.google.cn;
        }

}

```
# mount (disk)

> look disk info, find disk sign like '/dev/sda1'
```
sudo fdisk -l
```
>find file system type
```
sudo blkid
```
> edit config
```
sudo vi /etc/fstab
```
> in the last line ,add flow
```
/dev/sda1		/media/disk		ntfs	defaults	0 	0
> /dev/sda1  is disk sign
> /media/disk		where the disk mount
> ntfs		file system
>0 (first)	disk backup
>0 (second)	disk check (0 means don't backup  don't check
```
# idea

> 1. download idea ultimate
> 2. type  active by server 'http://btsha.com:41017'