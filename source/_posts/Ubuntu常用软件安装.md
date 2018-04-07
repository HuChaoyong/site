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
> 编程常用软件安装
-------------------------
# [Live Demo]()
---
![pic](./back.png)

# java

> download from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html), I use java8
```bash
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
sudo source ~/.bashrc
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

