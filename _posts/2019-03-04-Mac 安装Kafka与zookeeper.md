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
	
7. 终端简单的使用  
    1. 按照上面的内容，以后台启动zookeeper与Kafka
    2. 进入到kafka的安装地址处`/usr/local/Cellar/kafka/2.1.0/bin/`下

    	1. 创建一个名为test的topic,它有一个分区和一个副本 

    		> kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test  **注意mac 下可能不显示.sh所以 用kafka-topics  --create ... 下同**
    	2. 生产者端

    		> kafka-console-producer.sh --broker-list localhost:9092 --topic test

    	3. 新开一个终端，同样进入到`/usr/local/Cellar/kafka/2.1.0/bin/`下

    		> kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning **就能接收到生产者那发送的消息了**
    	
8. Python 中使用

	```
	pip install kafka
	from kafka import KafkaConsumer, KafkaProducer
	
	producer代码:
	producer = KafkaProducer(bootstrap_servers='localhost',auto_offset_reset='earliest', group_id='kafka_img_logo1')  # auto_offset_reset='earliest',添加上这个，就能消费之前添加的数据,group_id 是当有多个消费者时用到，表示在同一个组内
topic = 'my'
	def test():
	    print('begin')
	    n = 1
	    while (n <= 20):
	        a = dict()
	        a['name'] = 'zhangsan'
	        a['age'] = n
	        producer.send(topic, json.dumps(a).encode('utf-8'))
	        print("send" + str(n))
	        n += 1
	        time.sleep(0.001)  # 必须要有这个，否则报错
	
	consumer代码：
	consumer = KafkaConsumer('my', bootstrap_servers='localhost',auto_offset_reset='earliest', group_id='kafka_img_logo1')  
	for msg in consumer:
	    b = msg.value.decode('utf-8')
	    a = json.loads(b)
	    print(a)
	    print(a["name"], a['age'])
	
	```
	