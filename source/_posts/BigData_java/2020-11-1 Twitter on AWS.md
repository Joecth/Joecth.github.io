---
layout: post
categories: BigData_java
tag: []
date: 2020-11-1

---



# Twitter on AWS

- Kafka 

- Best: Consumer == Broker amount
- Broker - 分布式存、topic, partition, offsets給consumer去讀msg的
- consumer間獨立的



- earlist -- default是這個，就是說當前的consuer要是還沒有消費過任何一個topic，它是第一次去run, 我默認的是consumer從最早的開始讀起
- Group_id, 作LB的，這個topic有三個partition，告訴kfk我們有三個不同的consumer要來消費你，所以kfk會讓每個partition被一一對應；有多個不同的consumer的組織，如果我們都定義的是同個group_id，那



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a3j0zixj20wi0as3zg.jpg" alt="image-20201101212643942" style="zoom: 67%;" />

- Twitter client幫做了很多事

- hbc-core dependency in pom.xml, to know client APIs

  - Twitter hbc client --> 當前的 hbc repo
  - 功能型的dependency就知道「怎麼用」
    <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gka0e9a4uaj30rg0gaaib.jpg" alt="image-20201101214836345" style="zoom:50%;" />

  

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a3nbjsqj20te0m8acd.jpg" alt="image-20201101214946756" style="zoom: 33%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a3qyfu8j20vk0iu3zp.jpg" alt="image-20201101215206525" style="zoom: 33%;" />

- BlockingQ - 內存裡的buffer，邊寫邊抓；寫的時候不能寫太多

  - Producer受不了
  - 亂碼呢？－－＞沒data來，要丟出exception嗎
  - Throttle 限行它，等producer拿出寫去kfk

- ACK all -- data存在所有的 broker上

- kfk 1.1 大的改變

  - retries 由三個組成
    <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a3w3kfbj20sa06c0tb.jpg" alt="image-20201101224346503" style="zoom:50%;" />

    120秒後就丟了它

  - Backoff

    - 你現在很忙，我不一直催你，我等個100ms, 回頭再去敲你，讓你error有恢復的時間

  - 這些都是producer已經define好的參數
    <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gka28kbi1ej30ue0ia112.jpg" alt="image-20201101225220312" style="zoom:50%;" />

    - 當非 idempotent 會有問題
    - so, enable.idempotence = true

- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gka2um85ntj30x80ck79l.jpg" alt="image-20201101231332090" style="zoom:50%;" />

  - linger、batch 就是做了個 Cache裡的概念

    - 當前的cache不可能無限等待，當我設計等待的時間，當我的數據超超超兇來，我受不了，所以我還會設一個batch_size
    - 不能等太久，也不能太大還不沖出去

    ![image-20201101231633429](https://tva1.sinaimg.cn/large/0081Kckwgy1gka2xrc4yuj30ra03k77q.jpg)

  - kfk存的是 compressed後的 data
    ![image-20201101231749281](https://tva1.sinaimg.cn/large/0081Kckwgy1gka2z2l5qwj315s07o79r.jpg)

  - <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a44qtgyj21n00twdop.jpg" alt="image-20201101231825564" style="zoom:67%;" />

    - RT -- ReTweet

  - <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a48d8nzj21mg0kodkg.jpg" alt="image-20201101232022395" style="zoom:67%;" />

  - ![image-20201101232052099](https://tva1.sinaimg.cn/large/0081Kckwgy1gka328tq0oj316s0a80z7.jpg)





#### Home Settings

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a4cystij21c80mqgqu.jpg" alt="image-20201105165148390" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a4elwupj21ms0u0gup.jpg" alt="image-20201105183429254" style="zoom:67%;" />



![image-20201105191508324](https://tva1.sinaimg.cn/large/0081Kckwgy1gkeiftxhk1j319m0u0e82.jpg)



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a4k2n0xj211q0u0wjn.jpg" alt="image-20201105191524424" style="zoom:50%;" />



## Consumer

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgcojtgy9j30zc0ru153.jpg" alt="image-20201107092704795" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgdbvo4njj317q0340y5.jpg" alt="image-20201107094930303" style="zoom:50%;" />

- 三次握手

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgdeh3easj314y02g40g.jpg" alt="image-20201107095200584" style="zoom:50%;" />



- mvn clean; mvn compile; man exec:java 



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgdkl4fl7j31700aee2f.jpg" alt="image-20201107095752042" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgdm4ak2lj317g046n10.jpg" alt="image-20201107095921677" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a4xka01j216u0nak0p.jpg" alt="image-20201107100122141" style="zoom:50%;" />



## Issues on my current analysis

未五句話全用權重，只用了最長的



## Producer Elegancys

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkgdvdmjlmj30us0pwk1d.jpg" alt="image-20201107100814465" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a50t6e1j215q03wt9m.jpg" alt="image-20201107100847304" style="zoom:50%;" />

- 存著
- 可能還有人沒寫出，所以要shundown hook，先flush(), 再去 close()了



# Twitter Developers & APIs

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkeapu2012j310z0u0tfs.jpg" alt="image-20201105144800175" style="zoom:50%;" />

#### 1 In your words

In English, please describe how you plan to use Twitter data and/or APIs. The more detailed the response, the easier it is to review and approve.



#### 2 The specifics

Please answer each of the following with as much detail and accuracy as possible. Failure to do so could result in delays to your access to the Twitter developer platform or rejected applications.

------

Are you planning to analyze Twitter data?

Yes

**Please describe how you will analyze Twitter data including any analysis of Tweets or Twitter users.**



#### 3

Will your app use Tweet, Retweet, like, follow, or Direct Message functionality?

Yes

**Please describe your planned use of these features.**



#### 4

Do you plan to display Tweets or aggregate data about Twitter content outside of Twitter?

No

------

#### 5

Will your product, service or analysis make Twitter content or derived information available to a government entity?

No

In general, schools, colleges, and universities **do not** fall under this category.



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkebd5r9hxj30om0dyjsj.jpg" alt="image-20201105151024914" style="zoom: 33%;" />



### Sample

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkec3hn4czj30uz0u04a8.jpg" alt="image-20201105153542746" style="zoom:50%;" />



![image-20201105153752201](https://tva1.sinaimg.cn/large/0081Kckwgy1gkec5pulxaj31170u0796.jpg)







