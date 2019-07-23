---
title: UsuallyUsedCommand
catalog: true
date: 2019-05-15 21:36:41
subtitle:
header-img:
tags:
- Usually
---
> 日常用的命令, 速查
>

# Ubuntu

* enable root user
```bash
sudo passwd root
```
-----------------------
* disabled root user

```bash
sudo passwd -l root
```
-----------------------
* list listening tcp port
```bash
ss -lnt
```

# Docker

* list all container

```bash
# list all container
docker ps -a
```
-----------------------
* stop container
```bash
docker stop f3c322
```
* start container
```bash
docker start f3c322
```
-----------------------
* list all images
```bash
docker images -a
```
-----------------------
* remove images
```bash
docker rmi f3c322
```
-----------------------
* use dockerfile built on remote server
```bash
docker -H tcp://192.168.1.24:2375 build -t blog/190320 .
```
-----------------------
* built container with alias
```bash
docker run --name myBlog -d -p 8082:8080 4b22a4f3c322
# 4b22a4f3c322 is images id
```
-----------------------
* make a tomcat images with a Dockerfile
>before this you need download jdk-8u171-linux-x64.tar.gz and apache-tomcat-8.5.39.tar.gz file

```Dockerfile
# this is a Dockerfile
FROM ubuntu:16.04
# now add java and tomcat support in the container
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-8.5.39.tar.gz /usr/local/
# configuration of java and tomcat ENV
ENV JAVA_HOME /usr/local/jdk1.8.0_171
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.39
ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.39
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
# container listener port
EXPOSE 8080
CMD /usr/local/apache-tomcat-8.5.39/bin/catalina.sh run
```
> run this docker file
```bash
docker build -t sample/tomcat8.5.39 .
```

* run tomcat, 

```bash
# host port 7022 reflect container 8080 , 
# --restart=always   means always start when server start
# -v means set a volume 
# b8df790a5ce7 is images id
docker run -d -p 7022:8080 --restart=always --name tomcat8.5.39 -v /media/disk1/tomcat8.5.39/webapps:/usr/local/apache-tomcat-8.5.39/webapps b8df790a5ce7
```

# Gradle
* build.gradle file replace source
```
plugins {
	id 'org.springframework.boot' version '2.1.4.RELEASE'
	id 'java'
	id 'war'
}

apply plugin: 'io.spring.dependency-management'

group = 'com.jackshmily'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	// mavenCentral()
	maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	runtimeOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'mysql:mysql-connector-java'
	providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.security:spring-security-test'
}
```