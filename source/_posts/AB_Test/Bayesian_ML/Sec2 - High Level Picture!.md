---
layout: post
categories: BayesianML
tag: []
date: 2018-04-05
---



# Real-World E.G. of A/B Testing



### Drug

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3811fwlk6j210i0jo76y.jpg" alt="image-20201022162224014" style="zoom:50%;" />

### Website

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjus6uqvswj31200najy9.jpg" alt="image-20201019174113739" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjus7uutbwj311y0lkqa2.jpg" alt="image-20201019174212598" style="zoom: 50%;" />



### Biz Flyers

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjus96lyu7j311u0lk4c4.jpg" alt="image-20201019174328806" style="zoom:50%;" />



### Cloud Storage

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjusa2wqt8j30zw0lkala.jpg" alt="image-20201019174420871" style="zoom:50%;" />





## Pattern

![image-20201019174701794](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjuscv6owxj30z60k0wke.jpg)





# Bayesian ML

- Bayes rule is not BML per se, but rather just basic probability!

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381752ai0j21140hutcg.jpg" alt="image-20201019175324972" style="zoom:50%;" />



### In this Lecture!

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjuslglrm0j30yg0cedlg.jpg" alt="image-20201019175517025" style="zoom:50%;" />



### Problem

***Want to model the height of all the students of our class using a Gaussian***

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjusms8i5kj30z60jejv9.jpg" alt="image-20201019175633724" style="zoom:50%;" />



#### Way 1 -- Freq.

- collect data

- using maximum likelihood estimation, we would inf the mean and variance to do this!

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3817lz9nfj20w80jm0u8.jpg" alt="image-20201019180138139" style="zoom: 50%;" />





#### Way 2 -- Baysian

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjustzb9pjj30rs0bon06.jpg" alt="image-20201019180328594" style="zoom:50%;" />

### Sampling

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3817oadz7j210g0jyad5.jpg" alt="image-20201019180541609" style="zoom:50%;" />



### ML Models

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjusxw6u5tj30z20iin1m.jpg" alt="image-20201019180714117" style="zoom:33%;" />

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjusyvky65j30xc0jen11.jpg" alt="image-20201019180810406" style="zoom:50%;" />

- Not Interested in findingthe vector W! **BUT the distribution** of the random vector !
  - we want to find P of W given X & Y where X & Y is our training data



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjut0z2lrej30xi0kw7aj.jpg" alt="image-20201019181011767" style="zoom:50%;" />





### Bayesian Networks

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjut2ojxvaj30zi0iwdkq.jpg" alt="image-20201019181150246" style="zoom:50%;" />





#### Non-Bayesian Example

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjut3kuk5xj30zs0j8jxv.jpg" alt="image-20201019181241715" style="zoom:50%;" />



#### Bayesian Network

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3817xhrx9j20x80l2jsh.jpg" alt="image-20201019181342225" style="zoom:50%;" />

- So! with base that, we can model these dependencies explicitly!
  - Whereas, when using NN, we usually use the same model and let the NN figure out what the weights should be.

#### LDA

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjut7itzkuj30xq0kggr0.jpg" alt="image-20201019181628953" style="zoom:50%;" />



# Sum

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjutcgncymj30yo0mu10z.jpg" alt="image-20201019182113813" style="zoom: 50%;" />

