---
layout: post
categories: KaggleWinnerTips
tag: [] 
date: 2021-09-07
---







### Appendix:

 https://www.jiuzhang.com/seminar/293/



## PyTorch Hub

https://colab.research.google.com/github/pytorch/pytorch.github.io/blob/master/assets/hub/pytorch_vision_deeplabv3_resnet101.ipynb#scrollTo=interpreted-democracy



![image-20221011163550657](https://tva1.sinaimg.cn/large/008vxvgGgy1h71fpdqf6hj30x50u0gqp.jpg)



### Param sharing

![image-20221011164630536](https://tva1.sinaimg.cn/large/008vxvgGgy1h71g0f3f3zj31de0nsafn.jpg)



### Downsampling (Compression) -- Pooling





### AlexNet

![img](https://tva1.sinaimg.cn/large/008vxvgGgy1h71g88tcpej30yv0gpwhv.jpg)



### VGG

![image-20221011231121905](https://tva1.sinaimg.cn/large/008vxvgGgy1h71r4xcii2j31600mg0wr.jpg)



### ResNet

![image-20221011231732347](https://tva1.sinaimg.cn/large/008vxvgGgy1h71rb9v5faj316o0ne0vv.jpg)



![image-20221011231827669](https://tva1.sinaimg.cn/large/008vxvgGgy1h71rc8d980j310q0dgtaz.jpg)



### Receptive Field

![image-20221011232201081](https://tva1.sinaimg.cn/large/008vxvgGgy1h71rfyb28kj319g0kyjw0.jpg)

小的好處

1 權重少

2 非線性增多



### FC

![image-20221012004201570](https://tva1.sinaimg.cn/large/008vxvgGgy1h71tr746xwj317w0l8tea.jpg)





### DataSet

![image-20221012005500818](https://tva1.sinaimg.cn/large/008vxvgGgy1h71u4phnnhj30iw08u3z6.jpg)







### Model in .cache

![image-20221012104642160](https://tva1.sinaimg.cn/large/008vxvgGgy1h72b8ch38bj30g204adfw.jpg)





### Softmax + NLLLoss (neg log likelihood)

NLLLoss for Multi-classes Classification 任務的 Loss function

其意義為將 『Softmax』 過後的機率值『取 Log』並將正確答案的『機率值』加總取平均。



基本上，這種 Loss function 是越低越好，我們也可以經過實際的使用來發現，基本上機率值高的選項與我們的『標準答案』越一致，Loss 的確就會越小。







## Strategy

![image-20221012101122430](https://tva1.sinaimg.cn/large/008vxvgGgy1h72a7ljwpfj30wy0mqn04.jpg)

- A -- init
  - Task far from the model other's orig task
- B -- Freeze, when dataset
  - 小, < 10K, only train FC
  - 中, 10K~20K, only freeze former part, train FC and last 4 convs
  - 大… vice versa



#### Epoch

10~20

Best to 50



```
text = '''
[default]
aws_access_key_id = <your access key id> 
aws_secret_access_key = <your secret access key>
region = <your region>
'''
```





## SM

![image-20221013154951805](https://tva1.sinaimg.cn/large/008vxvgGgy1h73pm3rdqcj30ko1dk40p.jpg)





## Kernels

https://setosa.io/ev/image-kernels/

![image-20221013232452155](https://tva1.sinaimg.cn/large/008vxvgGgy1h742rkxwbpj32o00qy7ab.jpg)





![image-20221014092832004](https://tva1.sinaimg.cn/large/008vxvgGgy1h74k7mwz7cj31os0u0jxu.jpg)





![image-20221015214040099](https://tva1.sinaimg.cn/large/008vxvgGgy1h76azpuf56j31ps0rgqa5.jpg)

<img src="https://tva1.sinaimg.cn/large/008vxvgGgy1h75bhvvjg7j31380am0vo.jpg" alt="image-20221015011230215" style="zoom: 50%;" />



![image-20221015094409519](https://tva1.sinaimg.cn/large/008vxvgGgy1h75qa7clebj30ve0u0ael.jpg)





![image-20221015145849141](https://tva1.sinaimg.cn/large/008vxvgGgy1h75zdlrr56j315y0u07cf.jpg)



![image-20221015165148387](https://tva1.sinaimg.cn/large/008vxvgGgy1h762n5w5xbj31430u0ten.jpg)



![image-20221016182641874](https://tva1.sinaimg.cn/large/008vxvgGgy1h77b07f0rkj31300imgpn.jpg)





|      |
| ---- |
|      |
|      |
|      |
|      |
