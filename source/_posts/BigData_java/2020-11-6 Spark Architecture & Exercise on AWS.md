---
layout: post
categories: BigData_java
tag: []
date: 2020-11-6

---



# Spark Architecture



## What

- Data processing, **distributed (多台圍毆)** framework(工具包)
- framework vs computation model(單台)
- Batch processing (offline)全存去hdfs or hive, or Hive, VS. real-time

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a59ewfjj20o20icq4v.jpg" alt="image-20201107102121833" style="zoom: 33%;" />



- LinkedIn -- java
- FB -- php

- Scala 特性方便 compiling & runtime就是較快

#### Q: Hadoop MR 計算框架存在什麼問題？

- 太多硬盤讀寫 --> I/O 太多時間，每次data的讀寫。。。所以速度慢
  - 多開機器超浪費，才幾10個GB又要我多開。。

- 所以 Spark 出現了 --> 省了很多錢
  - 好的computation engine 可省很多錢
  - 分布式內存

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkges06cu8j30qe09mdib.jpg" alt="image-20201107103936335" style="zoom: 33%;" />



#### Q: 出現的第一個原因: Spark處理大數據很快，實時性很高



#### Q: 第二個原因: 幫助管理分布式機器和流程



## Data pipelin in Spark

1. 1 如讀了國家公園的data，讀入後，對於spark生成的不是 list of string, 
   而是 RDD, 就是個集合，包含源頭的所有數據。
2. 2 RDD會被partition了後的；分區的標準非常明確
3. 3 不可變的, immutable

- 對於數據的操作行為的當時狀態的copy；非內存地址的延用，而是真實
  數據的操作



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgg18ccvsj30sq0b2dme.jpg" alt="image-20201107112304043" style="zoom:50%;" />





## Basics in Spark

- 更快
- 可realtime, 是map-reduce做不了的

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5fbkucj20t40d80tk.jpg" alt="image-20201108210653409" style="zoom: 33%;" />



- 當新的RDD被生成，過程就是一條數據被讀進來，filter操作後，看這條data是不是contain California
  - Narrow - 未涉及別的RDD的變形
    - 這row不會依賴別row
  - Wide - 可以對當前的data進行計算。如知道當前每個park, state分別有多少個park?  --> GroupBy, ReduceBy, Union，跳躍於不同的RDD上的加和的動作。會對當前的record作紀錄
    - 沒人能保證多條Record在同個RDD, except we use kayhash for partition, otherwise, across RDD

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5i5o7yj212c084ab0.jpg" alt="image-20201108213059139" style="zoom: 33%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5krkb9j212q0r4q64.jpg" alt="image-20201108215941456" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gki48xejm4j314y0cywl5.jpg" alt="image-20201108220623119" style="zoom:50%;" />





# Exercise: Spark on AWS

##### EMR - elasitc map reduce

## Create Spark Cluster on AWS

### 1 SW

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gknc61jr2qj30w50u0jyw.jpg" alt="image-20201113102815402" style="zoom:50%;" />



- step 是和運行的code緊密相關的，可以code寫完後再設



### 2 HW

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5pm9egj20mp0riwh4.jpg" alt="image-20210305163613883" style="zoom:50%;" />

​		Task 没有 data node在run，相當只是對core的輔助，沒什麼必要，對大理解沒improvement

- SPOT 
  - 會去看現在的usage的情況，假設我半夜去開cluster，如果還是launch在US-eash，那時大家都在睡，我去開spot就可以很快；但如果是尖峰時，那現在available 的便宜機器就沒了，就會等待下一個available的機器來給我用；我很可能去等，但那台一直都沒得出現，cluster creation就會被浪費了; 反正我選c4large便宜點

- core: Data node; num core ↑; concurrency ↑
  - Data node的機器
- task: 沒有 data node 的demo在run



### 3 General Cluster Setting

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkndakrzqjj31d90u0wit.jpg" alt="image-20201113110702038" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gltgondf2sj30tc0eomyj.jpg" alt="image-20201219205815163" style="zoom:50%;" />



##### 最後也是需要個認證的 xxx.pem





### 4 Security

just next step



## Tools for AWS Spark

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkndfn8bvgj30yn0u0k0l.jpg" alt="image-20201113111204862" style="zoom:50%;" />



- Hadoop is the environvent base, for Spark and Hive, so not an Application
- 





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5vbe15j211v0u0wil.jpg" alt="image-20201120202325880" style="zoom:33%;" />





## S3's Usage

#### Create Bucket

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl6honxensj30u00vwq9e.jpg" alt="image-20201130000449672" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a5yk6p0j20u70u00vz.jpg" alt="image-20201120205035722" style="zoom: 50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkvxisc3p5j317c05sjz5.jpg" alt="image-20201120205135528" style="zoom:50%;" />



#### IAM

##### My Access Key

![image-20201120205359035](https://tva1.sinaimg.cn/large/0081Kckwgy1gkvxlbix3qj319g0u0dr1.jpg)



## Run Spark Application

### Add Step

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a6bnr3oj211m0lo765.jpg" alt="image-20201126195711764" style="zoom:50%;" />

- Cluster -- 可以把Code deploy 到 data node 上 ，也就是slave node上面去執行
- **Client -- 放在 Master上面去執行。當放到 Master時，它可以把任務分給其他的nodes!**

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a638tshj219q0oin0g.jpg" alt="image-20201126200139523" style="zoom:50%;" />



### Pending...

需要去蒐集資源，Spark需要在集群去找哪些機器可用，可設哪些參數呀？找到機器後就會進入Starting

![image-20201126200220489](https://tva1.sinaimg.cn/large/0081Kckwgy1gl2ttdcyb4j319o0fo785.jpg)





