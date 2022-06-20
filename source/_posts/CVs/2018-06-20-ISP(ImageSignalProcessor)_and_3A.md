---
layout: post
categories: CV
date: 2018-06-20
tag: [] 
---





 Camera! Let's go!



# ISP Architecture (Image Signal Processor)



**Important features in Camera Principle**

### 

### Notes:

isp晶片 會有個sensor 會裝個 len，會有rgb的pixels e.g.1920 1080 的方格，每一個都是一個r、g、b

接收不同光的是不同的像素格，在光進來後再做處理，進isp，isp再去處理，變成了後制成圖片

廣角、distortion重，

影像邊邊是彎彎的，魚眼鏡頭180度(水平最大的視角)，通常會有鏡頭到120、130就算很廣了

這種鏡頭的邊邊的distortion就很重

Isp就是可以校正這種畸變

針對鏡頭就算同型號畸變還是會有差

生產時會校正

同組參用在同型號的鏡頭

Isp的好壞會影響校出來的結果 

校出來的是另一個





sensor的光會進isp (pipeline)，

​	isp做distortion、還有像lens shading 邊邊入光量比較小就比較暗，邊邊的gain就要拉高一點，gain拉高，noise就會也一起高了

​	isp也要做noise reduction，noice太大，鏡頭光圈小入光就小，得用sensor 的gain拉大，這樣noise也大，就得靠isp去降噪

​	先mosaiac 還原，各個有各自的功能

​	3a只是其中的功能 aito white



![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h3es8d1qs4j20ug0j6abd.jpg)

![img](https://tva1.sinaimg.cn/large/e6c9d24egy1h3es9u7fkhj20ny0cyt9u.jpg)



ref: https://zhuanlan.zhihu.com/p/98820927







# 3A in Camera

### Auto focus

​	isp 會吐個focus value看銳俐度 —> isp有硬體可以偵測影像的銳俐度，在算pixel的微分

​	

​	會拿到focus value，會去動鏡頭，即對焦，再得到下一個focus value

​	很快推到最高點，鏡頭有個焦距，它原理就是看不同鏡頭對焦，

​	有個壓電效應在做的

​	**壓電效應 (Piezoelectricity)**，電流愈大，壓力就愈大，裡面有液體，壓力大，鏡頭就被會壓然後被推

​	就是電流去控焦距



​	input 就是focus value，output就是我要送多大的電流，也要判斷場景是不是有變

​	拿手機去對焦也會發現自動對焦會對錯或是不對，是代表演算法沒有很精準



### Auto White-Balance

白平衡難! 色彩學!

​	假設室內是黃光，我們台灣喜歡白光，黃光的室內的話就會把畫面變白，他會希望你看到的畫面是白的

​	人眼就是有自動白平衡的功能

​	假設我們看一個apple，有人打藍光，我還是知道apple是紅色，我人眼還是可以分得出原色，不會被黃光給影響到覺得是黃apple

​	當他偵測到r比較多，r的gain就得要小一點，b的gain就得要大一點，讓畫面沒有那麼紅!

​	光源 or 原色造成? 不知道

​	偶有過白! 



### Auto Exposure

​	光源不希望太亮或太暗

​	eg 行車紀錄器，我要進出tunnel，光會突然變暗，使用者會希望亮度馬上可以調到看得到裡面的東西

​	所以一sense到變暗，gain就要拉起來，過頭就會太亮，所以「怎麼拉」很重要

​	不可以給感覺一下子太亮或太暗

