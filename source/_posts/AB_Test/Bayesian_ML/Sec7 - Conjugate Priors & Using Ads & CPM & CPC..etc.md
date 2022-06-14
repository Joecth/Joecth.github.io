---
layout: post
categories: BayesianML
tag: [] 
date: 2018-11-23

---





# Conjugate Priors & Using Ads & CPM & CPC

###### CPM: Cost Per 1000 Impression

###### CPC: Cost Per Click



## Exercises on Conjugate Priors

### GOAL:

> Thompson Sampling / modifying posterior distribution for the params in question in REAL-TIME

--> Every sample I collect, I can update my beliefs and my estimate of the parameter becomes better! 

![image-20201209232556997](https://tva1.sinaimg.cn/large/0081Kckwgy1gli0r9g229j30zy0jkn1q.jpg)

==> diff from traditional AB-Testing where I have to run the whole test and only when it's finished, can I answer questions



### Center: Conjugate Priors

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381me8qy8j211a0lktbj.jpg" alt="image-20201209232903564" style="zoom:50%;" />

- This allows me to update the parameters of the posterior in closed form which usually just involves some + and *
- In addition, since AB-Testing on E-commerce, ==> CTR & Conversion Rate
  - Likelihood : Bernoulli
  - Conjugate prior: Beta distribution



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli0wnlj9fj311y0j6n2u.jpg" alt="image-20201209233108686" style="zoom:50%;" />



### Categorical Distribution

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli13cq0rkj30t80jg444.jpg" alt="image-20201209233735492" style="zoom:50%;" />

##### Application

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli2ckekjgj30yc03cjs3.jpg" alt="image-20201210002102375" style="zoom:50%;" />

##### Model



### Gaussian Likelihood

 **★★ If the reward is Gaussian distributed then the likelihood will be Gaussian**



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli2f5uwv0j31280h2tf9.jpg" alt="image-20201210002333117" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli2g0o2z4j312a0ik11z.jpg" alt="image-20201210002422044" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381mhgpp4j20zy0m8mys.jpg" alt="image-20201210002508566" style="zoom:50%;" />





## Exercise: Die Roll

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli303xteqj30t60dcjy6.jpg" alt="image-20201210004339948" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli33h92nej30wg0kq0wv.jpg" alt="image-20201210004654703" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli33s7268j30wc0kk451.jpg" alt="image-20201210004712934" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli344r6mbj30xw0kotcy.jpg" alt="image-20201210004732610" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli34co7zjj30xe0kwacv.jpg" alt="image-20201210004745104" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli34knbtcj30x80lawk3.jpg" alt="image-20201210004758298" style="zoom:50%;" />





## Obtaining an infinite amount of practices

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli35rq3kxj310u0a00vk.jpg" alt="image-20201210004906432" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli363h9o1j30y606yjsz.jpg" alt="image-20201210004925792" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli36mn5xpj310o0kodm0.jpg" alt="image-20201210004956807" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli378m52sj311k0dcdju.jpg" alt="image-20201210005031840" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gli38g9rd7j30wy0l27au.jpg" alt="image-20201210005141584" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkzj586qvcj30zq0isdmj.jpg" alt="image-20201123233626957" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkzj5wjes0j310u0lg43y.jpg" alt="image-20201123233705071" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkzj6ecnppj310o0ecjui.jpg" alt="image-20201123233734442" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkzjd3t9x1j30yg0j846i.jpg" alt="image-20201123234400729" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381mm36kjj20za06ojs8.jpg" alt="image-20201123234542014" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkzjf9bkckj312a0li7h3.jpg" alt="image-20201123234604809" style="zoom:50%;" />

