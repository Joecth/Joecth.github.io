---
layout: post
categories: RL
tag: [] 
date: 2018-11-26

---





# Intro

## Scenarios

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3821hkre7j216u0mwwhm.jpg" alt="image-20201217143845241" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glquhgwd0pj31720juqqb.jpg" alt="image-20201217143902772" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3821icn90j217a0jydic.jpg" alt="image-20201217143913867" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glquhy7q5zj31700lykgu.jpg" alt="image-20201217143930653" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3821n4na1j21400o241m.jpg" alt="image-20201217143952142" style="zoom:50%;" />

- Infant needs to walk from scratch, yet AI has no such limitation, since it can just copy the waling program from an other AI who already knows how to walk.
- Follow the law of accelerating returns



## Big Pic

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3821pqhwdj20w20f6abf.jpg" alt="image-20201217144308123" style="zoom:50%;" />

#### MAB

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqumhf0o8j313k0o4tiv.jpg" alt="image-20201217144352146" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glquni0b2ej312s0h8dp7.jpg" alt="image-20201217144450643" style="zoom:50%;" />



#### MAB ==>full  RL

##### model: encodes assumptions and relationships btw diff components of a system

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqupnald2j312w0logvo.jpg" alt="image-20201217144654280" style="zoom:50%;" />



#### Approaches

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqurfxuhuj310y0m6q6y.jpg" alt="image-20201217144837964" style="zoom:50%;" />

- fr 1 to 3 become more brittle and less robust
- With great power comes great caveats

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqwkl1r0vj311g0ha0vu.jpg" alt="image-20201217155114333" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3821xs7alj212i0la0vi.jpg" alt="image-20201217155259195" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqwmp55hhj30yw0mktnn.jpg" alt="image-20201217155316874" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqwozk1fgj312q0kwafy.jpg" alt="image-20201217155528120" style="zoom:50%;" />



## Coding!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxdeqw58j30wq060whm.jpg" alt="image-20201217160011900" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h382210bsrj20ww0kmwfr.jpg" alt="image-20201217160104084" style="zoom:50%;" />

## Warm up!

### Questions

#### Joint VS. Conditional dist.

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxdrxrvij312g0l0jx3.jpg" alt="image-20201217161917862" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxe41i3bj30y40i8mzw.jpg" alt="image-20201217161937499" style="zoom:50%;" />



#### Marginalization

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxep2qe7j30z00iydjl.jpg" alt="image-20201217162011380" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxff0dlsj312m0k4dku.jpg" alt="image-20201217162052839" style="zoom:50%;" />



#### E[X] - expected value, similar to marginalization

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxoi013hj30wi0kin2f.jpg" alt="image-20201217162936412" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38224kodkj210a0j63zl.jpg" alt="image-20201217163039697" style="zoom:50%;" />





#### E(X|Y) - Conditional Expectation

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38228hwg9j20r60ecjrr.jpg" alt="image-20201217163106768" style="zoom:33%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glqxqmw0e8j312e0m043q.jpg" alt="image-20201217163123244" style="zoom:50%;" />



- Analogous to the prev expectation
- the RV is still X, but now we take the integral over x times P of X given Y
- In other words, use the conditional dist. instead of marginal distribution



#### E[g(x)] - Expected value of a function

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr89j3uvwj30x80lsq84.jpg" alt="image-20201217223548078" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8a3tdz8j31100kugri.jpg" alt="image-20201217223622091" style="zoom:50%;" />



###### For the plain Expected Value, the g(x) is just the Identity



#### E(cX + Y) = ?  for linear operator

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8cs9y2gj30ug0ek41w.jpg" alt="image-20201217223856211" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8cfs0p7j30zu0f6gpl.jpg" alt="image-20201217223835734" style="zoom:50%;" />



### 

#### Expected value of X --> mean of X

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3822brw5cj20vk0d8dge.jpg" alt="image-20201217224000073" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8e5z0c6j30ns0ckmyf.jpg" alt="image-20201217224016329" style="zoom:50%;" />

- The expected value of X refers to the true mean of X





#### Monte Carlo as estimate E(x) <-- Sample mean

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8hxlsrej30zs0lcjyb.jpg" alt="image-20201217224353200" style="zoom:50%;" />



![image-20201217224628993](https://tva1.sinaimg.cn/large/0081Kckwgy1glr8kmm1ctj30yc0la0y4.jpg)





### Bayes Rule

![image-20201217225309928](https://tva1.sinaimg.cn/large/0081Kckwgy1glr8rlt97ij30qe0i2tb5.jpg)

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8tos1l1j30zo0lowke.jpg" alt="image-20201217225510620" style="zoom:50%;" />



###### Later can b dealt with elegantly without having to resort to Monte Carlo Methods



#### MSE

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8w9n3ogj30zw0lctek.jpg" alt="image-20201217225739679" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8wl0sugj30w20gead7.jpg" alt="image-20201217225757922" style="zoom:50%;" />



#### Closed Form or Analytical Solution and GD

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8x0y5y5j30ym0gc0w6.jpg" alt="image-20201217225823562" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8y6nm3lj310y0igwjh.jpg" alt="image-20201217225930243" style="zoom:50%;" />



#### Descent VS. Ascent

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr8zljzmnj311g0j4n2a.jpg" alt="image-20201217230051996" style="zoom:50%;" />



#### Matrix Form instead of GD for "MSE w.r.t W"

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr90ahfevj30s80iemzf.jpg" alt="image-20201217230131384" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr925c17tj311e0ic0x1.jpg" alt="image-20201217230318749" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3822hv66kj20zy0ji0ux.jpg" alt="image-20201217230507397" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3822fiw54j210u0j2q4k.jpg" alt="image-20201217230659583" style="zoom:50%;" />





