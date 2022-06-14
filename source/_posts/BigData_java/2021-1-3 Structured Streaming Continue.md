---
layout: post
categories: BigData_java
tag: []
date: 2021-1-3
---





- Default: Micro-batch != Real-Time

如何trigger-steaming job <== depends, 我是可以define trigger模式的。

每次的batch特別小，這個就是micro-batch, 我想實時看到結果

- Fixed interval: still micro-batch, with user-defined interval, 如就是hdfs，我就配合30分鐘就好啦！
  - 是上個開始和下個開始的時間之間的差別
    - 要是上個特長呢？做完上個超時完後，再接著下個job
    - 我設interval一定是知道freq 很低了，大概率，我做research的，就是知道10分鐘不會進來太多，就是我這個micro-batch能搞完的
  - 用的人可能會出錯…
- Once mode: 做一次而已，就是Batch Spark job, 做完就自己關掉了！



- Continuous mode:  別用…Latency: 100ms，雖然不是真的實時，但已經滿足大多數的scenario；Flink可以真的實時。。。近乎0
  - 兩年前開始實驗此一持續性模式，但support 功能還很陽春，因為底層還是batch。不然就是全翻掉重做了。。



- WINDOW! 
  - 要維護的不僅僅是當下的window,  還有之前的也要記得維護著；更像是一個Window History, 會去紀錄下來我上個interval、這個interval的WINDOWs各出現了什麼
  - 變深的就是當前的data frame裡我更改的變化
  - 在res table不斷地update 的；res 就是要統計、計算過的所有數據所生成的結果是什麼，而這結果都是存去disk上
  - <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38aa4tyksj21em0p6td1.jpg" alt="image-20210315164555094" style="zoom:67%;" />



- 很久如兩天前的要維護嗎？
  -  要是用戶選了complete mode，他還有個下游，他的下游把最後return的complete數據再extract出來最新的 time window，我當然還是維護
  - 但要是 append/update mode，就不需要維護了…
- 不理想的狀態下，什麼情況我還要去維護已經處理過的window 呢？
  - 如需要排序這類的時候，這時候就要用到舊的data；
    - 同理我還是會有個window，只要去處理window裡的排序，對window外的就不去CARE, 但是↓
      - 很常見的一個現象跟問題就是希望Streaming app可以去解決的，這個問題就是

- 理想狀態定義：
  - 沒有 Late Data, 就是 8:10 event time的會如時過來
    - 但是！Spark 有 Fault Tolerance特性，要是有個executor掛了，要有另個executor快去頂上，把還沒搞完的data快速搞完
    - 要是user要groupby time-window, 對structure-streaming而言，他就是要groupby, 他只關心這一個小時裡的topk的增量
  - 手機上被trigger了10/1，到kafka是10/30! Why? 有些地方網路差，過了半小時一小時後才出現；手機上操作很多今天蒐了地址，手機就發了很多個log去kfk，又不想打車了，就划掉了app，手機把payload cache著，半年後又想打車時，所有東西就重新發送。。就late了半年。。。
    - 但我又想保證正確性，意味，過去的state完全保存在memory當中，result table一直增長，但是distributed，就一直存在disk很ok., 但state很危險！＜＝＝ input table有個state, 紀當前增量到了哪個位置，記一個增量的卡點；window也有state也全是在memory裡．
    - 要是所有的window都存在memory裡，就很容易OOM
  - **Watermark 解決 OOM -- late data**
    - 並保證最終結果的穩定
    - memory裡只保存 watermark所指定的時間範圍內的data
  - 就是只維護要的window，而不是維護所有的windows在memory裡
  
- 最後寫去的，還是那三個地方
  - kfk <== update complete模式都很好，就是無限append的操作，就是無腦intsert, 無腦update
  - hdfs <== 不建議；讀是沒問題的，但寫的話。。。就是複雜，它的data就是不能update的，只能replace就是重寫了，不然就是append當前data
    - 如我要write到當前的folder時，我不會update當前folder的文件所對應的data，而只會去無腦append；這樣我這樣用hdfs跟用kfk就沒有區別，因為我也沒有按col作更新，我就只是寫入一組data，然後我又來了新的data，就生出新的文件說什麼又更新了一下，hdfs它的small file會很多，name space就會out of quota, (HDFS 就是一個namespace 下就是20w個文件)；這時要是batch又沒有設置interval，這樣namespace很快就會用完。
    - 就是要先想清楚用的場景、跟怎麼用
    - 所以最好是「流對流」、「batch 對 batch」
    - Kfk retention時間 default是一週，很多公司會縮到２、３天。大多數都是 log
    - 然後一般再按照data partition dump去HDFS上，HDFS上還可以設一個retention
      - 好處一：一次地dump，文件的大小、個數是由injection job決定的，就是很長時間內都不會有 small file問題, 如就每天200個文件
      - 好處二: HDFS可以設retention；分析用的data，也不可能去存十年陳年老data。一般就是２年。所以就是可以 date partition + retention
  - console



- Input 不再是words, 而是我在模擬機器發送一個完整的log, 就是要有event time，是event time，不是land到kfk的時間；kfk會給land到kfk的時間。所以手機上發去的時間戳是payload過去的。



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38aa87dnvj20sy02kq2y.jpg" alt="image-20210316154559107" style="zoom:67%;" />





### Kafka Consumer 

![image-20210316165252703](https://tva1.sinaimg.cn/large/e6c9d24egy1golui5db3fj21480am0z1.jpg)

那時用了很多kfk的API做了kfk的consumer

### Structure-streaming Consumer

取代了kfk consumer，這傢伙有自己的consumer，只是跟kfk自帶的api不同，

不再用之前說的kfk自帶的api, 如kfk consumer的pull , offset, commit這類的

structured-streaming有自己的流程



## Twitter Consumer 的問題們

1. Itempotency
   - kfk 以前可以保證exactly once，前提是得用kfk streaming，其他的第三方的application，都只能保證 ***at most* or *at least*　once** , 就是無法保證 exactly once；
     - 所以，導致，要是我模型不能保證itempotency，我就設at most once，因為我不怕丟data丟失
     - 我要是怕丟失，它重複讀了進來或retry 很多次，就... 一直加到爆
2. 之前不想rely on 自己的commit offset，我自己做得過程出問題了，雖然它覺得已經commit成功了，我不想要自己的auto commit --> 實時處理 process_id，手動的同步操作，只能blocking，等做完，才能commit 這個offset。



我展示的還是batch process，只有我把它integrate到一個streaming app，才能保證我真的**實時**

實時處理完的很可能有下游要用，所以餵給kfk，讓下游去聆聽我output sink丟去的kfk

實時蒐集完，然後展示

在selective時作bias而不是造假ＸＤ．．．　就是個流式分析！



分布式計算：都已經是分布式機器，每個機器有自己的executor，每個executor有自己的core, 每個core會執行一個task，只是它不是個multi-thread, 而是一個multi-process的操作；自己已經是個多線程操作，我沒必要再自己去開一堆。



# Structured Streaming Cont



- streaming
- fault tolerance



Window: 就是個GroupBy操作；2mins的window, 30secs step

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gmaw01fw7mj30l607yn16.jpg" alt="image-20210103224247930" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38aacy3a7j213c0fawi9.jpg" alt="image-20210103224231269" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gmaw6jljiij31jo0tgk9z.jpg" alt="image-20210103224902437" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38aaf8h03j21as0pu0v6.jpg" alt="image-20210103224919270" style="zoom:50%;" />



###### Watermark API很好用, TODO!





### Tweeter Consumer

- Itempotency!
- 紀錄 process_id, 不要 auto-commit；得要是個blocking thread；不可一邊commit，這要手動搞



自己維護靜態的內容，然後自己groupby

- Create_time 就是event_time, 它和我去拉下來的trigger的時間可能差很大的



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38aaiwwoxj20p00jodgt.jpg" alt="image-20210103231100449" style="zoom:50%;" />

MCN公司建網紅，公司付錢用Ins, or Twitter, FB APIs把data service出去，第三方的software抓來做dashboard, 以達到data driven，知道話題，如健身的時間、quanrantine、workout ... ==> build homegym

網紅的流量，訂閱看我的流量，知道我的view；我知道我相對去年的view、like、如何

個人的影響就可以分析

我不需要生數據，也不用創新只要用現有數據去建model、精準metrics、推廣app!



Consumer: 從普通的java app變成了 structure-streaming app, 是比較強的framework

