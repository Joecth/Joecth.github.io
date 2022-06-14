---
layout: post
categories: BigData_java
tag: []
date: 2020-11-13

---





## 前情

- Spark VS. Flync, 阿里promote、ckin
- Spark's RDD
  - partitioning
  - lazy evaluation
    - 不需直接全塞進 memory
  - lazy initilization 讀檔
- Persistence -- 因為知道它會被多次利用，所以不要殺了
- Immutability - 不能修改，只能被重創建
- Failure recovery -- 不需要多設置
- Transformation
  - Narrow
  - Wide -- 多個分區間的data轉移跟交換
    - RDD往回找dependency 之間的關係





# Pokemon Go!



## RDD vs. Dataset vs. DataFrame

### RDD

- data每一row是個黑盒，具體的row裡的schema的info是被藏起來的
  - 對於Spark也是個黑盒，不只對我而言
  - Spark 讀RDD只能按位置讀data, 性能、可讀性都不好
  - 當想get col時只能row.get()然後用index去看



### DataFrame

- Catalyst可以捨棄data, spark內部執行分析的method，去優化query、filter掉 col 之類
- 有schema，可以更好優化
- Partition 一定以row作劃分，十萬row分到三個機器上去
  - col 的概念，分叫sharding或schema也可以
- 當type錯時，因為非強關係型的full structural data, 所以讀進來時dataframe無法去辨認的，問題會在read、iterate時
  - 在 foreach每一個row時，在parse long時，要是讀到個string，會RE
- 半結構化的數據，允許只在精靈第一代存在，第二代卻不存在了的問題
- 支持 SparkSQL 的訪問
- in memory當data很大很大時，很占資源, job就跑得慢



### Code

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a6ojgvqj20xe0k0tdl.jpg" alt="image-20201114095137792" style="zoom:50%;" />





##### Type issue when Error -- Semi-Structural

![image-20201114100533902](https://tva1.sinaimg.cn/large/0081Kckwgy1gkoh4qznu9j313i0jetn9.jpg)



#### DF

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkohau8ub1j315g0mo1av.jpg" alt="image-20201114101124260" style="zoom:50%;" />

- Catalyst 幫處理了DAG上的路徑、DF裡的議題



#### DF VS. RDD

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkohd0z53mj30si09s41x.jpg" alt="image-20201114101332175" style="zoom:50%;" />



- ★★ DF裡的info 我們可以直接去select某一個column，因為它知道這個table裡有幾個Column; 但是，當我去select這個table的如第一row當下，想去得到這個row的某個value也只能用index 了，就是也還是得去解析了。。

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkohj1l8amj30d803m74u.jpg" alt="image-20201114101918675" style="zoom: 50%;" />。



##### Delimiter 的 ',' 在讀進來時已經分好了



#### SQL Table, 其實更像 View

- 因為只有abstraction layer of view, 讓別人在access時覺得是個table, 只是data都存成了schema的database的型式
- String of query much EASIER than cascade of APIs

- 易有人為失誤



#### DF VS. Dataset

- Dataset讀進來時已經是個Pokemon class 的obj了；DF讀進來時是個Row class 的obj

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a6thisuj20s40go0u6.jpg" alt="image-20201114103553415" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a6vt75hj20vk0ao759.jpg" alt="image-20201114104120385" style="zoom:50%;" />



##### DF RE vs. Dataset Compiling Error

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkoi7nfn3jj313e0e2ds9.jpg" alt="image-20201114104257475" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkoi8du8f2j30hm03igm5.jpg" alt="image-20201114104340363" style="zoom: 50%;" />

- 方法名錯就叫不了，因為class裡沒定義
- 但readability 強



### Sum

- RDD for 完全沒結構化時的好工具
- DSet 缺點 不希望code base存幾十種不同的class且還要去跟遠程access
- DF 可能比較平均
  - 如果data不是很raw，已被先處理過了有很強的schema的了，已sanitize了的schema存儲，那read-in的data就不用去擔心型態問題，只是這個col對應到的val可能是個NA的問題，DF既不用自定義一堆class 又有Catalyst的優化



## DF APIs

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a70hh71j20xy0iw411.jpg" alt="image-20201114105806698" style="zoom:50%;" />



###### Bug

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkoips6ol5j30xk04otaj.jpg" alt="image-20201114110022848" style="zoom:50%;" />



### Union vs Join

#### Union

- when identical schema btw diff tables
- append tableB to the tail of tableA

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkoj13vebwj30w007kwhp.jpg" alt="image-20201114111115758" style="zoom:50%;" />



###### 先 filter or 先 sort()?

- Spark 會做優化，先filter了後好，所以就算我寫反，也沒差



#### Intersection

![image-20201115212831583](https://tva1.sinaimg.cn/large/0081Kckwgy1gkq6ht80x9j317g0bwgs3.jpg)





### Shuffle

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkq78xf644j311s0jygt7.jpg" alt="image-20201115215442243" style="zoom:50%;" />





### Transformation

#### Coalesce

- 寫出時候變少
- 以挪動最小的數據為代價，幫把partition個數降低，會以平均的方式挪過去



#### Repartition

- 會嘗試以平均分配的方式，把當前的partition全部進行重新分配



### Action

