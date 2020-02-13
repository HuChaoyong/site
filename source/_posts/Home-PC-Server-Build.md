---
title: Home PC Server Build
catalog: true
date: 2019-03-23 00:31:00
subtitle: Use your Home PC to create a server
header-img:
tags:
- Network
catagories:
- Network
---
# By En-us
* what we need ?
1. A home PC
2. A router
3. A method can cross The Wall (墙)

## 1. Get a domain
* open the site [no-ip](https://www.noip.com), and then, registry a count.
* create a host for free (every 30 days, it will confirm once)
> what every port is ok, when you set your no-ip hosts.

## 2. Set the ip
* check your router, use the PPPoe way to link the internet.
> if you don't know the username and password for the PPPoe acount,<br>
you can call your ISP a tell to resolve.

## 3. Set the Forward on router
* login in your router manage page (usually the url is [192.168.1.1](http://192.168.1.1))
* find the <b>Forward<b> setting, and forward the outer port to you inner port.
## 4. check 
* use any server on your pc, and set outer port forward to your pc.
* open the url
> how to install noip-clien at the end of the arctle.

## Congratulation ！！！ that'all!
> now you can use the domain to visit your home PC.


-----------------------------------

# By Ch-zn
* 需要些什么？
1. 一台个人电脑
2. 一台路由器
3. 能够翻♂墙的渠道

## 1.获取一个域名
* 打开[no-ip](https://www.noip.com)网站，注册一个账户
> 打开这个是要翻♂墙的
* 注册完后，创建一个免费的域名，目前是只能以  *.ddns.net 结尾.
>(每30天会发信息给你，提示确认,毕竟免费)<br>
一定要安装no-ip的那个客户端.安装了才能保证你再变了外网ip后能够通过域名找到你的电脑

## 2.设置ip
* 检查路由器，使用 PPPoe(拨号上网)的方式来连接外网.
>如果没有拨号上网的账号，请电话联系你的网络提供商

## 3.设置端口转发
* 登陆路由器的管理界面
* 设置端口转发，比如设置外网的8890转发到你电脑的8080端口

## 4.检查
* 任意部署一个服务在你的电脑上，并设置外网端口转发到该服务
* 通过no-ip的地址+端口，打开你的服务, 能打开就说明成功了！
> 具体安装 no-ip客户端在文末尾.

## 恭喜你，
> 如上步骤仅仅是大致流程，细节请参考搜索具体博客.论坛.


# install no-ip client (linux)

## 1.download client file. 
```bash
wget https://www.noip.com/client/linux/noip-duc-linux.tar.gz

# after unzip it.

tar zxvf noip-duc-linux.tar.gz -C ~/program
```

## 2. install

```bash
cd ~/program/noip-2.1.9-1
# 
make

make install
# after that. will input some info.
# 1. account for no-ip
# 2. password for your account
# 3. set interval time(minutes) default is 30.
# 4. Do you wish to run something at successful (N).
```

## 3. run it.
```bashr
sudo /usr/local/bin/noip2
```