---
layout: post
categories: SystemDesign
tag: []
date: 2020-01-08
---



## System Desing vs OOD

|                         | Points                                      | Format                              | Problems  |
| ----------------------- | ------------------------------------------- | ----------------------------------- | --------- |
| System Design           | Dataflow<br />Architecture<br />Scalability | 系统設計圖<br />DB設計<br />API設計 | Instagram |
| OOD (不是個web service) | Class Interface<br />Inheritance            | Code<br />Data Structure            | 停車場    |



###### Product Design = System Design - Scalability





## Misunderstandings

### 1) Ignored Non-functional Req

### 2) Ignored some data stream

####  e.g. corner case

- e.g. Whatsapp裡的四條data streaming

- e.g. celebrity 需要去區分不同的data streaming 作優化 <= 普通人 vs 名人

### 3) 背書

- 自己把info作長篇大論作背誦；--> better to used in "deep-dive"

### 4) Trade-off

- 黑客版的網爬，我們可以做出五種不同的構件 <= 權衡做好並作利弊

- Ins's Push(low latency) vs Pull(Normalization in SQL)

### 5) 過度依賴Tips/Guides

- 自己得要想問題去問 --> 
  一般人會follower多少人? 會有幾個人follower他?

- 多少人看每天？多久po文一次? --> 去了解讀跟寫

### 6) 答題不分主次

- 自己未把「abtraction」、「encapsulation」分清楚
- Level-wised details, to know how to deep-dive
- e.g. 網爬 --> 網爬's details

### 7) 過度強調技術選型

- 只知某種原理的細節 --> 只知某種特定的Cache <== 容易被深挖就尷了。。
- 不同種之間的廣度對比作優劣判斷
- 結論: 會把自己繞坑

- e.g. SQL vs NoSQL
  - NoSQL: Wide column! 别特定進去到 Cassandra

### 8) 混亂的系統設計圖

- 如果無法修復好的話，會很亂!
- 要清晰，不然看的人會忘

### 9) 答得過慢

- 時間不夠用，但別糾在末節
  - e.g. Non-fn req: 沒到10mins, 就 3mins
  - e.g. resource estimation:別到誇張到 8 mins 
- 節奏得要是穩步向前，所以時間得able to control

### 10) 忽略流量模式!

Ins - 大v



Memcached: 只是kv store; 

Kafka 难被替代

: 

## DB

- DB是存儲實時訪問數據的永久存儲 vs warehouse <== 非實時訪問 (Hive QL 類似map reduce，apache的一個項目，幫大家去存大量的歷史data)，存在hard disk裡作長期存儲，一般在服務不會去訪問它
- SQL vs NoSQL
  - SQL : hd是很稀缺的，注重把data壓到很少，因為hd很貴。所以 JOIN
    - 好懂 --> 2D表格表示事務 --> 符合大腦 vs NoSQL(有時有3D)
    - 好維護 --> Consistency!!! 歷經實踐檢驗、慢慢掛 vs NoSQL cascaded 地全掛
    - 靈活 --> 語言強大，一些事在DB層就可用 vs NoSQL都只能在應用層來做
    - CONS:
      - 鎖 -> 慢
      - 不靈活，表難擴展、結構難改
      - 只有partition:單機分到不同地方，没有sharding:機器間的分配
        - Proxy SQL可以幫sql起到sharding，讓proxy去協調哪個機器存什麼

###### kafka topics數量不能太大；但也是看它怎麼用，topics可以分partition就可以很大 10萬或Million這級別；每個partition都是有順序

		- event in topic 看什麼是partition key --> 分partition
		- E.G. img post : img `topics`; user_id as `partition`



###### Data lake(可能可以是hive): offline data就是相對於stream (溪流)而言、etl raw data可是個data lake部分，比較是包含很多技術的總稱



###### Data warehouse不能立即返回，他用的是跟sql類似的言語；hive不含sql所有功能，傾向是map-reduce去處理，而不是都在ram裡處理掉，時間跑得長，不能立即返回結果；不是文件系統，HIVE是HDFS上包裏了一層，提供了一個固定的schema表格的型式去存data的機制，並提供了語言去處理如跟map-reduce結合處理



- 
  - NoSQL
    - 讀寫強! <== lock用得少
    - 存儲機制靈活多樣 <== 四種大樣，且支持靈活的 schema
    - 原生支持高併發 <== 易橫向擴展
      - 横(用多台機器)、縱(單一台)有特定含義的!
    - cons:
    - 較低的一致性
    - 性能理解不足，cassandra易一串failure，如其consistent hash ring易加減機器，但系統一旦有1/4機器出包，系統會連續愈來愈多掛掉，讓一個系統很快degrade… 
      - 當時多台就是為了想stable，缺乏在大量req下deploy 服務的經驗
    - 不支持JOIN --> 提倡 denormalization



### 怎選

#### 1) 是否要ACID?

- a:原子性 --> 两個寫的要求，打包發生
- c:一致 <==有一些沒一致是ok的  --> data寫的時候是準確的，讀出來還是要同個data，沒有打包這個含義 
- i:不同data不互影響
- d:長期在db

#### 2) 是否需要JOIN?

- E.g. 設計db去解決要不要JOIN的問題

#### 3) 是否要scalability

- Nosql 就是可以橫向擴展

#### 4) data model?

- 如果是個friend graph 如fb上
  - 想找到跟用戶A有在3個結點都找到 <-- neo4j graph DB 可原生解決它
    - SQL也可以寫三層、六層XD... 很累啊
  - 如果kv就ok那就是簡單的memcache就ok



###### mongo這兩年可以join，从一個table拿出field再去query另個table，不是面試中要關心的

###### 捨棄P --> 無法多機了。。就只有CA了

###### sql即使有sharding也是讀寫比較不如nosql因為有鎖

###### c的cae: DB的改寫並不會去通知到要改cache

###### availability: 指的不是容災，而是可被有正確值丟出 <== TODO: 11:56



### CAP

###### mongodb 我們不覺得他會常當機無法訪問



#### Document Store

Relational VS MongoDB(靈活 json)



#### KV Store

現今都是redis over memcached

- memcached: value只能是simple string，非常簡單的system，source code很短，10w行以下
  - 比較簡單的in memory 的kv
- redis: 可以是一堆特別的data structure、還有transaction、sharding、監控、lock的系統
  - 還會用一些disk、可以persistency



#### Wide-Column Store

- 來自  Big-Table 論文，
  - 一個cell可以對應一堆時間!
  - column family(少的): column(多的)
  - Sparse DB <== 後話



#### Graph Store

- Neo4j 比較好query



###### SQL & NoSQL界線正在變模糊





## Cache

#### 熱點data

- LRU



#### Session storage

- instagram預加載分頁



#### 低latency reading

- whatsapp上線後快速讀到別人發來的



#### custermer side's cache

- Autocomplete `客戶端預載`補全結果，本地解决，減少server壓力



### 讀

#### Cache-Aside TODO:12:18

先讀cache(並不在主流程中，就算cache掛了，也會去db讀)，再讀db，

cache當了不會讓整個當

### 

#### Read-Through



### 寫

#### Write-Through

寫時先寫到cache，cache再寫去db，兩個綁在一起叫完成



#### Write-Back

Cache你何時寫給db你的事，我寫給你我ok就好了!



#### Write-Around

我直接寫給DB，Cache在旁邊



|                  | Write-Through | Write-Around | Write-Back |
| ---------------- | ------------- | ------------ | ---------- |
| **Cache-Aside**  | N/A           |              |            |
| **Read-Through** |               |              |            |



###### TODO: 12:33



#### Redis 永久化方案

|                                | 全稱                 | 優點                                 |
| ------------------------------ | -------------------- | ------------------------------------ |
| RDB 記狀態<br />snapshot       | Redis DB Backup File | file小，不影響讀寫速度，快速災後修復 |
| AOF 記過程<br />會降低讀寫效率 | Append-Only File     | 最大程度降低data丟失，紀錄每次讀寫   |

###### MySQL本來就在DB，不需要討論持久化



### CDN

E.g. Netflix 我覺得他肯定在某個地方會變超popular，諾蘭的新片會很熱門! 離永戶愈近愈好!

降低consume file 的成本、提高讀大文件的效率、減慢latency







### MQ

###### TODO:!!!