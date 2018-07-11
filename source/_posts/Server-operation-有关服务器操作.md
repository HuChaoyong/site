---
title: Server operation(有关服务器操作)
catalog: true
date: 2018-04-28 00:03:40
subtitle:
header-img: "mnt.jpg"
tags:
- Linux
catagories:
- Linux
---
> 平时玩服务器，这些个东西得记下来(我懒)

# ssh 连接服务器
```bash
# login the remote server by port 32627, ssh default port is 22
> ssh -p 32627 root@23.106.122.111
# then input the password
```

# scp (uplaod or download file)
```bash
# upload local file to server, by port 32627
scp -P 32627 ./jdk-8u151-linux-x64.tar.gz root@23.111.222.333:~/Downloads
# download file from server
scp -P 32627 root@23.111.222.333:~/Downloads/1.jpeg ./
```

# tar and zip (zip or unzip file)

```bash
# unzip jdk file to the '/usr/program'  directory,if not set '-C' or '-d' means
# unzip in current directory
# if you don't special the directory, it will unzip in current directory
tar xzvf jdk-8u151-linux-x64.tar.gz -C /usr/program
unzip jdk-8u151.zip -d /usr/program

# zip the directory 'bootstrap' to 'btsp.tar.gz'
tar czvf btsp.tar.gz bootstrap
zip -r btsp.zip bootstrap
# zip the all '*.jpg' file to 'picture.tar.gz'
tar czvf picture.tar.gz *.jpg
```
# wget 

> wget is a tool for download

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```