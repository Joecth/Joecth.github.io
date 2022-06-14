---

layout: post
categories: BigData_java
tag: []
date: 2020-10-24
---



## Kafka



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk04dmxpk5j3102032di4.jpg" alt="image-20201024083045739" style="zoom:50%;" />![image-20201024083519950](https://tva1.sinaimg.cn/large/0081Kckwgy1gk04idtk1hj30zw0480vt.jpg)



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk04j5lnuhj30zg0b8k1x.jpg" alt="image-20201024083605168" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk04jvx04vj30zm04wte3.jpg" alt="image-20201024083647240" style="zoom:50%;" />



### Demo: 創建一個 Topic

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a1rd9iij20vm05emyc.jpg" alt="image-20201024085524024" style="zoom: 50%;" />

- 未發送 key，按默認的 round-robin
- Offset 就是個 id，跟 訊息長短沒關

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk056hkxnkj30vu03gacf.jpg" alt="image-20201024085829960" style="zoom:50%;" />



### Back-door 我可優化的

Kafka 其中一台掛了怎辦？備份

- 兩個放在同個機房(rack) --> latency不會過大
- 另一個在不同城市，我要replicate的時候比較慢，但ok
- 所以在latency、reliability找了平衡



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk05k9xsnfj30vi0f8wpp.jpg" alt="image-20201024091144456" style="zoom:67%;" />



- TERMs: ack、latency、Throughput



### On IntelliJ

- Zookeeper要先在背后起来
- 機器之間一定是通過network，network時就只能序列化變一個個地byte，
  - int serializer
  - string serializer.  --> 大多數情況都是這傢伙



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk0735ftpqj30xs06c79h.jpg" alt="image-20201024100429381" style="zoom:50%;" />

- producer.flush() -- 會幫把數據直接沖到kafka queue裡去
  - 存在問題! 
  - 只是個blocking call，但不是同步
  - 就跟你这个info绑定了，要是你没跟我说你成功，我就会有个线程，一是在盯你，我一直浪费
- Producer.send(recordNoKey) 一定是 Async的

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a1unxh2j219e0esjuf.jpg" alt="image-20201024101109235" style="zoom:50%;" />

- Producer 自己就是個 client之一
- 三次握手也不需我們實現不然太蝦



- 那个是partition id，不是replica 的id



#### Maven

軟體管理工具 -- FB、Uber都有用

項目管理工具





#### Producer Keyhash

- key如果被detect到是有東西，就會有hash去hash 去決定把這個partition發送到哪個 broker上面去；否則就是 round-robin



#### Producer onCompletion

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a1yw56xj20zs0l840w.jpg" alt="image-20201025230649208" style="zoom: 67%;" />

- production 不要用! 會變單線程
  - 難道因為一個人的login失敗了，我就要stop整個producer嗎？
  - 我要做的事應該是producer不傻
- 一般是async時也過陣子來查看
- 一般在prod不需要知道發去了哪裡，在roundrobin裡不需要知道
  - 如果想知道話，也不是好奇；而是假設consumer的處理性特殊 -- groupby key -- 要是roundrobin就會分到10台機器，現在的shuffle洗牌才能把它們放到一台機器上去；要是很常，我的處理就意味不高效! 為了高效，我就會不再roundrobin，而是hashbykey!
    - 所有的bigdata key 都在一台機器上，就不再要洗牌，雖然是全局的groupby；一旦沒有全局的groupby，我就很快
    - 即然我指定了我的key，我就可以很有信心知道這條要去哪台機器，因為kafka會去handle



#### Consumer

- 比 producer 的設置裡多了一個 GroupID



### Kafka 再論

- Kafka 啟動時會設定這個cluster要幾台機器組成

- Kafka 是個 Pull 模型，因為

  - Scalibility : 它是個很好的decouple的作用，把 producer、consumer很好地分開了；各自壞了不會影響到對方。

    - 如果這個Topic很多下游在訂閱，kafka要是push的話，它會忙死，而且下游搞不好還沒要你這更新
    - 它忙壞了，根本無法 scale-up

  - Diversity : -- LEVERAGE & STALIBILITY

    - 每個consumer都有自己的配置、自己的處理方式和節奏，kafka應該把處理信息的能動性(Leverage) 給 consumer。

    - 要是這個組比較窮，或他們的priority低，他們處理數據不來，而且也沒budget；或許可以48hr再生一個結果就好。

      consumer可更穩定 (STALIBILITY)



- 每次我的好友建tweet時，該pull or push？
- consumer 跟 partition有綁定的關係！只會給一個consumer去讀
  - consumer去監聽partition
  - 平行就一定會有race condition
    1. 第 0 條該誰去讀？consumer0還是consumer4?
    2. 假設真的通過層層複雜的設計，各自讀一條
       - 我讀完時我要是早處理完時？
       - 我讀0時你讀1，你又不去commit，一方面在race了，一台機器上是idempotency，跨機器時呢？consumer不知道1號partition有沒有被讀成，怎辦？那consumer0跟consumer4還要有一個交流的體系
       - 交替讀的時候還是會出現問題
  - 所以沒必要多台consumer監聽一個partion的必要
  - 系統裡不是最快就是最好的，當數據小時沒差。要是平行時的overhead大時，開一台就ok啦

- 中轉，一週後刪了；它的作用就不是保持數據的，才能scalible
  往上強堆機器又要花錢!
- 問題就是: 工程師寫consumer會處理錯

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk1yqkmwomj30rq0agwks.jpg" alt="image-20201025224640508" style="zoom: 67%;" />



# AWS



## Charging

MSK: https://aws.amazon.com/msk/pricing/
 EC2: https://aws.amazon.com/ec2/pricing/on-demand/



## AWS, Create Kafka Producer

### Amazon MSK

service - MSK

- 2.2.1 (recommended)

#### Config

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a21llf0j20mw0p6n0d.jpg" alt="image-20201030211156060" style="zoom: 33%;" />

#### Network

- 被建在網路集群中
- VPC -- vertual private cloud 虛擬的網路環境
  1. 一個具有安全性的網路集群，跟外界隔離
     - 如果通過amazon別的service或我自己的電腦要去訪問其中的節點是做不到的，因為不屬於 security network的。
  2. cluster裡的所有機器之間可以進行「安全」的相互通信，是有保證的
3. VPC 會有個默認的
  
  <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a23uydbj20bq04l0st.jpg" alt="image-20210303154906134" style="zoom: 50%;" />
- Number of Availability Zones
  - Seattle, Australia, ... 
  - 假設我的機器集群活躍在兩個地方，SF斷電了，Seattle還可以提供服務
  - 其實**２**就很夠了，AWS不會掛的啦。。人家大公司
- Zones & Subnet
  
  - 就直接選吧
  
    <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a25s6ydj20l90iaq40.jpg" alt="image-20210303155659042" style="zoom:50%;" />



#### Brokers

- 對於 kafka一定是要多台機器來組成多線程的一個機器群體，所以在當前的kafa cluster裡，一定有多台機器。每台都被稱作broker，多台在一起就是brokers。
- 我在cluster裡，這個instance要有多大？如果預測每台都比較累，就是選大的
- kafka.t3.small，ok了，可做小實驗
  <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7oycisrej30vo09agmw.jpg" alt="image-20201030214136615" style="zoom:50%;" />

- 共四台目前在兩個不同的區域上



#### Tag

好區分名而已

 

#### Storage

- 1000GB 很很夠了



#### Encryption

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a291fvrj20ny06edgg.jpg" alt="image-20201030214316835" style="zoom:50%;" />

- 這次懶得加密也OK啦. 所以選擇了Both



#### Encrypt data at rest

![image-20201030214403963](https://tva1.sinaimg.cn/large/0081Kckwgy1gk7p0wno8zj313a09k40f.jpg)





#### Authentication

暫時還不需要



#### Monitoring

##### CloudWatch

- 監測系統

- 會把當前在Amazon雲上所run的每一個服務，幫單獨地拽出來，幫分析

  - traffic

  - throughput

  - input/outputs

  - 弄成一張很美美的圖表，知做了什麼事，在哪些時間做了事

    

- Basic Monitoring
- Broker 可能有多個 Topic, 各自的Topic有它們自己的partition
  
  - 可知道每個Topic都 go through了什麼數據

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2bffr1j20r60aedh6.jpg" alt="image-20201030214910491" style="zoom:50%;" />





##### Broker log delivery

- 當我們在run服務時，每個操作都會發個log給我；如發送數據了、發送成功了、consumer去讀了、consumer讀成功了，它話超多的！如果我一天發幾百萬條，一定是存log不能全念給我聽。透過log來分析的，找「10分鐘前發來，我在９分鐘前就該收到，但到了現在都還沒有收到」，這時候，我需要去找log才能分析，需要地方存我的 log

  1. CloudWatch

     - 有自己的 log group

       ![image-20201030215205786](https://tva1.sinaimg.cn/large/0081Kckwgy1gk7p99b9w0j31co0oan2e.jpg)

     - 好處就是當我的這些log我可以直接搜索，類似grep！

     ##### Log Group
  
   - 足夠強了，一般而言, 我可以去蒐當時的這個log
       ![image-20201030215829133](https://tva1.sinaimg.cn/large/0081Kckwgy1gk7pfwi9gsj313o0e8dm5.jpg)

  2. S3 -文件系統
  
   - 大批量
     - 好處是S3大，可以dump去離線如本地的大批量處理，去做bulk分析

  3. Data Firehouse
  
   - 可以對數據轉化，最後再發去 elasticsearch, 讓我作更細化的分析
     - 太 overkill
  
  <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7pi4vbcvj312k0lo3zz.jpg" alt="image-20201030220038315" style="zoom:33%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7pihdpfhj315k0h475n.jpg" alt="image-20201030220058590" style="zoom:33%;" />

- data warehouse - 每天數據進來了，如用戶的post進來了，觸發API到FB去。只是個concept都收進到寫到**HDFS**，有些已有結構化了，如手機上的SDK已定義好了schema (這已經比較像 Data Lake了)、**hive** table





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7pk4nilvj31j60lktc7.jpg" alt="image-20201030220232929" style="zoom: 50%;" />



###### CloudWatch Log

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2dw2vpj20oh0nnacn.jpg" alt="image-20210303165153271" style="zoom:50%;" />



#### Create cluster! 



## AWS - 建EC2, 還缺新的機器做P & C, So, create another client 

- 還缺 producer, consumer去做事！需要創建他們！

### EC2 Service

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2gmm8oj210n0u0793.jpg" alt="image-20201030220808970" style="zoom:50%;" />	



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2jdvjnj20zc0ct76r.jpg" alt="image-20210303172849633" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2lrp50j21g40tktdp.jpg" alt="image-20201030221432582" style="zoom: 67%;" />





#### Access 遠程的EC2機器

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1go6ukkgr91j20wf0u04qp.jpg" alt="image-20201030222232631" style="zoom:50%;" />

- 通信，得要去找kafka的代理產生的，kafka的client就是這個代理
  - 要下載 kafka client代理的包

```bash
[ec2-user@ip-172-31-16-188 ~]$ curl https://downloads.apache.org/kafka/2.3.1/kafka_2.12-2.3.1.tgz -o kafka.tgz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 59.4M  100 59.4M    0     0  11.0M      0  0:00:05  0:00:05 --:--:-- 12.6M
[ec2-user@ip-172-31-16-188 ~]$ targ -xvzf kafka.tgz
-bash: targ: command not found
[ec2-user@ip-172-31-16-188 ~]$ tar -xvzf kafka.tgz
```

##### bin/*.sh, 都是好練習

```bash
[ec2-user@ip-172-31-16-188 bin]$ pwd
/home/ec2-user/kafka_2.12-2.3.1/bin
[ec2-user@ip-172-31-16-188 bin]$ ls
connect-distributed.sh               kafka-reassign-partitions.sh
connect-standalone.sh                kafka-replica-verification.sh
kafka-acls.sh                        kafka-run-class.sh
kafka-broker-api-versions.sh         kafka-server-start.sh
kafka-configs.sh                     kafka-server-stop.sh
kafka-console-consumer.sh            kafka-streams-application-reset.sh
kafka-console-producer.sh            kafka-topics.sh
kafka-consumer-groups.sh             kafka-verifiable-consumer.sh
kafka-consumer-perf-test.sh          kafka-verifiable-producer.sh
kafka-delegation-tokens.sh           trogdor.sh
kafka-delete-records.sh              windows
kafka-dump-log.sh                    zookeeper-security-migration.sh
kafka-log-dirs.sh                    zookeeper-server-start.sh
kafka-mirror-maker.sh                zookeeper-server-stop.sh
kafka-preferred-replica-election.sh  zookeeper-shell.sh
kafka-producer-perf-test.sh
```

- 要記得下載dependency

```bash
[ec2-user@ip-172-31-16-188 bin]$ sudo yum install java
```





## MSK, Go!

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2rmlolj21fg0tgtd4.jpg" alt="image-20201030223315509" style="zoom:50%;" />

#### EC2 connects to MSK

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2v6y0mj20u00u278k.jpg" alt="image-20201030223418415" style="zoom:50%;" />

- 當前這麼多kafka機器的地址、端口分別是多少，有四個ip address

- 我任選一個

- 先簡單跟Kafka cluster talk一下，問他當前機器有幾個topic呢？

  ###### TIMEOUT ISSUE

  - 為何超超超慢，卡了？

    - 不是Kafka cluster有問題，也不是client 機器有問題

    - 問題在哪？

      - vpc 內可以自由通信，但地外的沒資格進行對話！-- 在創建 MSK時的事！MSK裡的VPC可以幫完成當前的集群四台機器無阻的要求。
      - 後來的EC2，是地外的機器，是沒資格作交流的！so, timeout

      ```bash
      [ec2-user@ip-172-31-16-188 bin]$ ./kafka-topics.sh --bootstrap-server b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --list
      Error while executing topic command : org.apache.kafka.common.errors.TimeoutException: Timed out waiting for a node assignment.
      [2020-10-30 14:42:15,234] ERROR java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.TimeoutException: Timed out waiting for a node assignment.
      	at org.apache.kafka.common.internals.KafkaFutureImpl.wrapAndThrow(KafkaFutureImpl.java:45)
      	at org.apache.kafka.common.internals.KafkaFutureImpl.access$000(KafkaFutureImpl.java:32)
      	at org.apache.kafka.common.internals.KafkaFutureImpl$SingleWaiter.await(KafkaFutureImpl.java:89)
      	at org.apache.kafka.common.internals.KafkaFutureImpl.get(KafkaFutureImpl.java:260)
      	at kafka.admin.TopicCommand$AdminClientTopicService.getTopics(TopicCommand.scala:272)
      	at kafka.admin.TopicCommand$AdminClientTopicService.listTopics(TopicCommand.scala:197)
      	at kafka.admin.TopicCommand$.main(TopicCommand.scala:64)
      	at kafka.admin.TopicCommand.main(TopicCommand.scala)
      Caused by: org.apache.kafka.common.errors.TimeoutException: Timed out waiting for a node assignment.
       (kafka.admin.TopicCommand$)
      ```

    - 也要允許 EC2 過來！加額外的 permission

  - 改變 kafka 的 default Security Group 所允許的網路的traffic!
    ![image-20201030224646396](https://tva1.sinaimg.cn/large/0081Kckwgy1gk7qu5kgboj31la0gsjuy.jpg)

    - **加到 inbound, 這樣EC2的request 才能夠進去到 MSK**
    - outbound: 我想出去

    <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7qyreomij317g0u0jw3.jpg" alt="image-20201030225110743" style="zoom:67%;" />

    

  

  - 重 run 剛剛的

    ```bash
    [ec2-user@ip-172-31-16-188 bin]$ ./kafka-topics.sh --bootstrap-server b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --list
    
    [ec2-user@ip-172-31-16-188 bin]$
    ```

    - 連結上了！！！

- 創建 Topic!

  ```bash
  [ec2-user@ip-172-31-16-188 bin]$ ./kafka-topics.sh --bootstrap-server b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --create --topic test-kafka --partition 4 --replication-factor 2
  Exception in thread "main" joptsimple.UnrecognizedOptionException: partition is not a recognized option
  	at joptsimple.OptionException.unrecognizedOption(OptionException.java:108)
  	at joptsimple.OptionParser.handleLongOptionToken(OptionParser.java:510)
  	at joptsimple.OptionParserState$2.handleArgument(OptionParserState.java:56)
  	at joptsimple.OptionParser.parse(OptionParser.java:396)
  	at kafka.admin.TopicCommand$TopicCommandOptions.<init>(TopicCommand.scala:578)
  	at kafka.admin.TopicCommand$.main(TopicCommand.scala:49)
  	at kafka.admin.TopicCommand.main(TopicCommand.scala)
  [ec2-user@ip-172-31-16-188 bin]$ ./kafka-topics.sh --bootstrap-server b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --create --topic test-kafka --partitions 4 --replication-factor 2
  [ec2-user@ip-172-31-16-188 bin]$ ./kafka-topics.sh --bootstrap-server b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --list
  test-kafka
  [ec2-user@ip-172-31-16-188 bin]$
  
  
  -----
  [ec2-user@ip-172-31-37-109 bin]$ ./kafka-topics.sh --bootstrap-server b-1.bigdata-msk-0305.e95lct.c3.kafka.us-east-2.amazonaws.com:9092 --list
  __amazon_msk_canary
  __consumer_offsets
  [ec2-user@ip-172-31-37-109 bin]$ ./kafka-topics.sh --bootstrap-server b-1.bigdata-msk-0305.e95lct.c3.kafka.us-east-2.amazonaws.com:9092 --create --topic test-kafka-0305 --partitions 4 --replication-factor 2
  [ec2-user@ip-172-31-37-109 bin]$ ./kafka-topics.sh --bootstrap-server b-1.bigdata-msk-0305.e95lct.c3.kafka.us-east-2.amazonaws.com:9092 --list
  __amazon_msk_canary
  __consumer_offsets
  test-kafka-0305
  
  ```
  
  ​	



## Producer Send msg & Consumer Read msg

如何來模擬 producer & consumer

- 先基於腳本作操作就 OK
- --producer-props 指的是 producer的 properties 最重要的就是我得告诉他我要發送data的cluster它的IP或地址，它得知道bootstrap.servers地址、選ack機制

```bash
[ec2-user@ip-172-31-16-188 bin]$ ./kafka-producer-perf-test.sh --topic test-kafka --num-records 10 --throughput 10 --producer-props bootstrap.servers=b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 acks=all --record-size 128
10 records sent, 10.741139 records/sec (0.00 MB/sec), 163.70 ms avg latency, 483.00 ms max latency, 181 ms 50th, 483 ms 95th, 483 ms 99th, 483 ms 99.9th.
[ec2-user@ip-172-31-16-188 bin]$ ./kafka-consumer-perf-test.sh --broker-list b-3.testkafka.dt6pwu.c2.kafka.us-east-2.amazonaws.com:9092 --messages 10 --print-metrics --show-detailed-stats --topic test-kafka
time, threadId, data.consumed.in.MB, MB.sec, data.consumed.in.nMsg, nMsg.sec, rebalance.time.ms, fetch.time.ms, fetch.MB.sec, fetch.nMsg.sec

Metric Name                                                                                             Value
consumer-coordinator-metrics:assigned-partitions:{client-id=consumer-1}                               : 4.000
consumer-coordinator-metrics:commit-latency-avg:{client-id=consumer-1}                                : 31.000
consumer-coordinator-metrics:commit-latency-max:{client-id=consumer-1}                                : 31.000
consumer-coordinator-metrics:commit-rate:{client-id=consumer-1}                                       : 0.033
consumer-coordinator-metrics:commit-total:{client-id=consumer-1}                                      : 1.000
consumer-coordinator-metrics:heartbeat-rate:{client-id=consumer-1}                                    : 0.000
consumer-coordinator-metrics:heartbeat-response-time-max:{client-id=consumer-1}                       : NaN
consumer-coordinator-metrics:heartbeat-total:{client-id=consumer-1}                                   : 0.000
consumer-coordinator-metrics:join-rate:{client-id=consumer-1}                                         : 0.033
consumer-coordinator-metrics:join-time-avg:{client-id=consumer-1}                                     : 3034.000
consumer-coordinator-metrics:join-time-max:{client-id=consumer-1}                                     : 3034.000
consumer-coordinator-metrics:join-total:{client-id=consumer-1}                                        : 1.000
consumer-coordinator-metrics:last-heartbeat-seconds-ago:{client-id=consumer-1}                        : 1604070397.000
consumer-coordinator-metrics:sync-rate:{client-id=consumer-1}                                         : 0.033
consumer-coordinator-metrics:sync-time-avg:{client-id=consumer-1}                                     : 57.000
consumer-coordinator-metrics:sync-time-max:{client-id=consumer-1}                                     : 57.000
consumer-coordinator-metrics:sync-total:{client-id=consumer-1}                                        : 1.000
consumer-fetch-manager-metrics:bytes-consumed-rate:{client-id=consumer-1, topic=test-kafka}           : 45.415
consumer-fetch-manager-metrics:bytes-consumed-rate:{client-id=consumer-1}                             : 45.412
consumer-fetch-manager-metrics:bytes-consumed-total:{client-id=consumer-1, topic=test-kafka}          : 1371.000
consumer-fetch-manager-metrics:bytes-consumed-total:{client-id=consumer-1}                            : 1371.000
consumer-fetch-manager-metrics:fetch-latency-avg:{client-id=consumer-1}                               : 21.750
consumer-fetch-manager-metrics:fetch-latency-max:{client-id=consumer-1}                               : 32.000
consumer-fetch-manager-metrics:fetch-rate:{client-id=consumer-1}                                      : 0.132
consumer-fetch-manager-metrics:fetch-size-avg:{client-id=consumer-1, topic=test-kafka}                : 342.750
consumer-fetch-manager-metrics:fetch-size-avg:{client-id=consumer-1}                                  : 342.750
consumer-fetch-manager-metrics:fetch-size-max:{client-id=consumer-1, topic=test-kafka}                : 412.000
consumer-fetch-manager-metrics:fetch-size-max:{client-id=consumer-1}                                  : 412.000
consumer-fetch-manager-metrics:fetch-throttle-time-avg:{client-id=consumer-1}                         : 0.000
consumer-fetch-manager-metrics:fetch-throttle-time-max:{client-id=consumer-1}                         : 0.000
consumer-fetch-manager-metrics:fetch-total:{client-id=consumer-1}                                     : 4.000
consumer-fetch-manager-metrics:records-consumed-rate:{client-id=consumer-1, topic=test-kafka}         : 0.331
consumer-fetch-manager-metrics:records-consumed-rate:{client-id=consumer-1}                           : 0.331
consumer-fetch-manager-metrics:records-consumed-total:{client-id=consumer-1, topic=test-kafka}        : 10.000
consumer-fetch-manager-metrics:records-consumed-total:{client-id=consumer-1}                          : 10.000
consumer-fetch-manager-metrics:records-lag-avg:{client-id=consumer-1, topic=test-kafka, partition=0}  : 0.000
consumer-fetch-manager-metrics:records-lag-avg:{client-id=consumer-1, topic=test-kafka, partition=1}  : 0.000
consumer-fetch-manager-metrics:records-lag-avg:{client-id=consumer-1, topic=test-kafka, partition=2}  : 0.000
consumer-fetch-manager-metrics:records-lag-avg:{client-id=consumer-1, topic=test-kafka, partition=3}  : 0.000
consumer-fetch-manager-metrics:records-lag-max:{client-id=consumer-1, topic=test-kafka, partition=0}  : 0.000
consumer-fetch-manager-metrics:records-lag-max:{client-id=consumer-1, topic=test-kafka, partition=1}  : 0.000
consumer-fetch-manager-metrics:records-lag-max:{client-id=consumer-1, topic=test-kafka, partition=2}  : 0.000
consumer-fetch-manager-metrics:records-lag-max:{client-id=consumer-1, topic=test-kafka, partition=3}  : 0.000
consumer-fetch-manager-metrics:records-lag-max:{client-id=consumer-1}                                 : 0.000
consumer-fetch-manager-metrics:records-lag:{client-id=consumer-1, topic=test-kafka, partition=0}      : 0.000
consumer-fetch-manager-metrics:records-lag:{client-id=consumer-1, topic=test-kafka, partition=1}      : 0.000
consumer-fetch-manager-metrics:records-lag:{client-id=consumer-1, topic=test-kafka, partition=2}      : 0.000
consumer-fetch-manager-metrics:records-lag:{client-id=consumer-1, topic=test-kafka, partition=3}      : 0.000
consumer-fetch-manager-metrics:records-lead-avg:{client-id=consumer-1, topic=test-kafka, partition=0} : 2.000
consumer-fetch-manager-metrics:records-lead-avg:{client-id=consumer-1, topic=test-kafka, partition=1} : 3.000
consumer-fetch-manager-metrics:records-lead-avg:{client-id=consumer-1, topic=test-kafka, partition=2} : 3.000
consumer-fetch-manager-metrics:records-lead-avg:{client-id=consumer-1, topic=test-kafka, partition=3} : 2.000
consumer-fetch-manager-metrics:records-lead-min:{client-id=consumer-1, topic=test-kafka, partition=0} : 2.000
consumer-fetch-manager-metrics:records-lead-min:{client-id=consumer-1, topic=test-kafka, partition=1} : 3.000
consumer-fetch-manager-metrics:records-lead-min:{client-id=consumer-1, topic=test-kafka, partition=2} : 3.000
consumer-fetch-manager-metrics:records-lead-min:{client-id=consumer-1, topic=test-kafka, partition=3} : 2.000
consumer-fetch-manager-metrics:records-lead-min:{client-id=consumer-1}                                : 2.000
consumer-fetch-manager-metrics:records-lead:{client-id=consumer-1, topic=test-kafka, partition=0}     : 2.000
consumer-fetch-manager-metrics:records-lead:{client-id=consumer-1, topic=test-kafka, partition=1}     : 3.000
consumer-fetch-manager-metrics:records-lead:{client-id=consumer-1, topic=test-kafka, partition=2}     : 3.000
consumer-fetch-manager-metrics:records-lead:{client-id=consumer-1, topic=test-kafka, partition=3}     : 2.000
consumer-fetch-manager-metrics:records-per-request-avg:{client-id=consumer-1, topic=test-kafka}       : 2.500
consumer-fetch-manager-metrics:records-per-request-avg:{client-id=consumer-1}                         : 2.500
kafka-metrics-count:count:{client-id=consumer-1}                                                      : 64.000
[ec2-user@ip-172-31-16-188 bin]$
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a2zlnw2j20z10u0wn6.jpg" alt="image-20201030231046314" style="zoom: 50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a335dbsj21830u0k3a.jpg" alt="image-20201030231059776" style="zoom:67%;" />



## Check Metrics & Close machines

#### CloudWatch

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a36ygn5j213f0u00ye.jpg" alt="image-20201030231615009" style="zoom:67%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gk7rrtvyaoj31c60boq5s.jpg" alt="image-20201030231908873" style="zoom:67%;" />



##### Mine:

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glt8qicv2cj314l0u0qd2.jpg" alt="image-20201219162312374" style="zoom:50%;" />



###### kafka -- can only be deleted, and then reconfigured next time

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glt8yk8kuyj31f40fkdlf.jpg" alt="image-20201219163057742" style="zoom:50%;" />



## Qs

