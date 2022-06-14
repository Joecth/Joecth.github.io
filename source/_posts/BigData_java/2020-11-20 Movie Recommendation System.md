---
layout: post
categories: BigData_java
tag: []
date: 2020-11-20
---



# Movie Recommendation System



1. 用戶沒有明確蒐索時的推薦
   - 點進Netflix瀏覽時
   - 抖音 -- 看你停留的時間、按讚
2. 用戶有明確蒐索目標時
   - 在 Google 索東西時
     - 當BigData時，相關放前面
       - 但不同人想的 BigData 可能不一樣



## 幾種設計方法

### Collaborative Filtering

底層原理之一，以更好地結合 Spark

#### User-based

基於用戶的相似性來推薦物品

- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkwj4okczpj313609oabt.jpg" alt="image-20201121091913608" style="zoom:33%;" />



#### Item-based

- <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a7dzn3zj20rs08emxi.jpg" alt="image-20201121092248091" style="zoom: 33%;" />
  - User C 喜歡 Movie1, 所以想找像 Movie1 這樣子的好電影



#### 哪個好? ==> 沒一定

- 不同的 use-case 沒一定



## In Proj - Item-Based CF

1. Item 數量比 user少，計算量也會減少
2. Item 的改變更加低頻，計算量比夜少
3. 更加具有說服力 (?!)
   - Vs 新聞、理財產品



### Co Occurence Matrix

- 如果有愈多人同時rate過這兩部電影，我們認為這兩部的相關性愈大
  - Why this Assumption?
    1. Rating 的前提是感興趣的
    2. 但如果IronMan + 貞子呢？
       - 當 pool變足夠大時，一個average的行為可信；相關的電影的相關性會愈來愈大
       - 只會變 Noise

##### Self-~join on userId



### Source:

on www.kaggle.com

- features: user_id, movie_id, rating
- 找到每個user所看的所有 movie的組合 (movie1, movie2, rating1, rating2)





## Coding

### Co Occurence Matrix

#### New DF

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl4n71au7ej30hs070gnr.jpg" alt="image-20201128094423479" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkwkyxqkckj30u006mtby.jpg" alt="image-20201121102253957" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkwl1yvezoj30f608q0un.jpg" alt="image-20201121102549289" style="zoom: 50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl4mp7r3djj310s0jon7n.jpg" alt="image-20201128092712951" style="zoom:50%;" />



##### DeDup by setting Rule

- Rule : movie1 < movie2



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkwm12852bj312k0g4wjt.jpg" alt="image-20201121105932505" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl4nne9fx1j30y80dydka.jpg" alt="image-20201128100007159" style="zoom:50%;" />



##### Coalesce

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl4peg4qz5j31fk0gsncz.jpg" alt="image-20201128110042938" style="zoom:50%;" />



![image-20201128110336591](https://tva1.sinaimg.cn/large/0081Kckwgy1gl4phg8zzfj31ei0o6wth.jpg)



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl4pj34pedj30qm0hgtgz.jpg" alt="image-20201128110511202" style="zoom:50%;" />

