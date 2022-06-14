---
layout: post
categories: BigData_java
tag: []
date: 2020-10-23
---



## Kafka

### My Installation

```zsh
10882* brew cask install java
10885* /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"\n
10886* echo 'PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile
10896* brew cask install homebrew/cask-versions/adoptopenjdk8
10897* brew install kafka
10904* java -version
10905* vim /usr/local/etc/kafka/server.properties		↓ 
															uncomment "listeners=PLAINTEXT://:9092"
```



### Servers of Zookeeper & Kafka

```zsh
10906* zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
10907* kafka-server-start /usr/local/etc/kafka/server.properties
```

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gjzkxdfcs3j31xk0ng7dc.jpg" alt="image-20201023211741754" style="zoom:67%;" />



### Topics

```zsh
10909* kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
	>> Created topic test.
	
10911* kafka-topics --list --bootstrap-server localhost:9092
	>> test
	
10913* kafka-topics --describe --topic test --bootstrap-server localhost:9092
	>> Topic: test	PartitionCount: 1	ReplicationFactor: 1	Configs: segment.bytes=1073741824
					Topic: test	Partition: 0	Leader: 0	Replicas: 0	Isr: 0

10914* kafka-run-class kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic test
	>> test:0:0
```



### Producer & Consumer

```zsh
10915* kafka-console-producer --broker-list localhost:9092 --topic test
10916* kafka-console-consumer --bootstrap-server localhost:9092 --topic test
10918* kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a1fowkwj220m0n2n1g.jpg" alt="image-20201023212415260" style="zoom:67%;" />





## Maven Proj

- ⌘ + N -- Generate

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a1i4boaj21870u0dlv.jpg" alt="image-20201024003840912" style="zoom:50%;" />





### 