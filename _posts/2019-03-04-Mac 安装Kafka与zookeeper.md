---
layout: post
title:  Mac 安装Kafka与zookeeper
categories: 安装
tags: 安装 zookeeper Kafka 
author: junwu152106
excerpt: Mac 安装Kafka与zookeeper
mathjax: true
---

### Mac 安装Kafka与zookeeper
1. brew install kafka
	如果没有安装zookeeper，也会一起安装
	+ 安装地址分别是`/usr/local/Cellar/kafka/2.1.0`与`/usr/local/Cellar/zookeeper/3.4.13`
	+ 安装的配置文件位置是`/usr/local/etc/kafka/server.properties `与` 
   /usr/local/etc/kafka/zookeeper.properties`
   
2. 启动zookeeper
	命令为`zkServer start`或者`brew services start zookeeper` **注意：好像是默认后台启动的** 
	
3. 	启动Kafka 
	- 命令为`brew services start kafka` **这样是后台启动的** 
	- **非后台启动**的话：进入到`/usr/local/Cellar/kafka/2.1.0/bin/`命令为`kafka-server-start /usr/local/etc/kafka/server.properties` 这样一旦关闭该终端，Kafka便不在提供服务。

4. 查看是否zookeeper与Kafka是否启动成功
	- losf -i:2181 查看zookeeper
	- losf -i:9092 查看Kafka
	
5. 关闭zookeeper
	- `zkServer stop` 或者`brew services stop zookeeper`  可再次查看2181端口
	- `brew services stop kafka`
	
6. 修改配置文件
	* /usr/local/etc/kafka 下 server.properties
		- advertised.listeners=PLAINTEXT://本地ip地址:9092    或者localhost:9092
		- num.partitions=2	分区为2个，可以有2个消费者
		- zookeeper.connect=ip地址:2181 	 或者localhost：2181
	

	
	
