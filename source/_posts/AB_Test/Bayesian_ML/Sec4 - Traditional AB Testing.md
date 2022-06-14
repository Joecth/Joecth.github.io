---
layout: post
categories: BayesianML
tag: [] 
date: 2018-10-13

---





## Confidence Interval

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx390jkcxj30tu03owfq.jpg" alt="image-20201021173502064" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd0d4rpjnj30wc0dw41j.jpg" alt="image-20201104120414000" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd0fgroxej30n80dmjtm.jpg" alt="image-20201104120629244" style="zoom:50%;" />

### Sum of RV is also RV, CLT

★ 特別是針對 expectation的！

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2euyzgej30x40fsjw0.jpg" alt="image-20201103162931919" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2gn14mcj30tc0h644k.jpg" alt="image-20201103163114888" style="zoom:67%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3818ltxvwj20sc0cc0t8.jpg" alt="image-20201104145718737" style="zoom:50%;" />



### CI - meaning

- I want to know the range of values that are likely to contain the true value of param I'm lookng for.

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd5lrs4e5j30tm0geq9h.jpg" alt="image-20201104150531747" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2hccb2aj30r60hgwjm.jpg" alt="image-20201103163155114" style="zoom:50%;" />



#### CI Limits

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2hr9x68j30x00gqn2j.jpg" alt="image-20201103163219208" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd5ic3630j30wg0g2q8l.jpg" alt="image-20201104150214583" style="zoom:50%;" />



### CDF

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd5io9f1rj30sw0d077q.jpg" alt="image-20201104150233686" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd5jckgs7j30s20dytc3.jpg" alt="image-20201104150312389" style="zoom:50%;" />





### PPF -- inverse of CDF

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2j50lfzj30v40h8aea.jpg" alt="image-20201103163338579" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd5kj32cjj30tw0880v3.jpg" alt="image-20201104150420994" style="zoom:50%;" />



### Final CI

![image-20201103163512531](https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2krh4xcj30y20i0q8a.jpg)

#### !! We don't know actual σ

- Solution: use **sqrt of the maximum likelihood estimate of the  valirance** 

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2ju3va1j30xq0n8q7s.jpg" alt="image-20201103163419169" style="zoom:50%;" />



### Non-approx. version VS. Our approximation

1.  use t! but too far away
2. Gaussian also OK, as it good in Bernoulli! --> in this case!
3. for Bernoulli, we can use the exactly same approximation!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2lsbonnj30wa0g8dlz.jpg" alt="image-20201103163611643" style="zoom:50%;" />

### Bernoulli CI

- use Bernoulli's param to substitute into!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2m5fi5uj30xo0fe0y0.jpg" alt="image-20201103163632710" style="zoom:50%;" />



### Sum

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2mioo9wj30x00gcq9t.jpg" alt="image-20201103163653542" style="zoom:50%;" />

#### ↑ Bayesian is more elegant, but later!



## AB Testing

### Setup fr Conversion Rate -- to know WHETHER DIFF ?!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc2ontp7sj30x40fik2d.jpg" alt="image-20201103163857378" style="zoom:50%;" />



##### Quntify w/ Statistical significance testing!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkczzul4b2j30v80bmaft.jpg" alt="image-20201104115127660" style="zoom:50%;" />



##### Statistically Significant! @ "α"

###### Drawback: have to choose α ourselves

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwyvowthyj30tw0f443o.jpg" alt="image-20201021150350153" style="zoom:50%;" />



##### Hypotheses -- 有３種

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381954g32j20vu0g8wg1.jpg" alt="image-20201021150437924" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwz0gasqoj30qw0cggqm.jpg" alt="image-20201021150824783" style="zoom:50%;" />



##### ★ Focus 2-sided test!



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwz16tu87j30ry0400ur.jpg" alt="image-20201021150907155" style="zoom:50%;" />





### Recipe - "Alg." to detect 2 groups different

#### test statistic

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwz2p6pwbj30ss0800yg.jpg" alt="image-20201021151034270" style="zoom:50%;" />



##### Unbiased Estimates w/ N-1

##### ![image-20201021151651242](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwz98g5m6j30w80gg441.jpg)

##### if Un-equal length

- should use a different **Pool Std** and test statistics



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3819aouqwj20tg06s75e.jpg" alt="image-20201021151729667" style="zoom:50%;" />





#### t-distribution -- Fatter tails

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3819c090uj20vs0iggn0.jpg" alt="image-20201021153100112" style="zoom:50%;" />



###### t's pdf

![image-20201021153122530](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjwzoc9lbcj30v60bq79s.jpg)

- **Greek letter: ν --> ( new ) : degrees of freedom**
  - it contols how wide or skinny the PDF is
- t-distribution can also have a **mean and scale** parameter, but again we won't be using those here!



#### Test Statistic -- t changes w/ conditions

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0gewu0rj30wy0ck452.jpg" alt="image-20201021155820879" style="zoom:50%;" />



#### t-Distribution & Statistically Significant !

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0j6fpp4j30x40ggdnl.jpg" alt="image-20201021160100750" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0kszdisj30w207078e.jpg" alt="image-20201021160234547" style="zoom:50%;" />



### P-Values

**the probability of detecting a difference** even when the 2 groups are the same

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0oikuw8j30w60aqjz7.jpg" alt="image-20201021160608180" style="zoom:50%;" />



#### Definition: 

*The prob of obtaining a result equal to or 'more extreme' than what we actually observed, when the null hypothesis is true*

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0p3y1fqj30w80fcaik.jpg" alt="image-20201021160642311" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3819jq27kj20w4092t9z.jpg" alt="image-20201021160957827" style="zoom:50%;" />

- t: our test statistic

##### Reject when p-value is small! V.S. CANNOT Reject

- CANNOT Reject != "H0 is true"

![image-20201021161208051](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx0ur3ea5j30w80b0gsl.jpg)



##### t ↑ P↓ --> Difference↑

ref: https://medium.com/@chih.sheng.huang821/%E7%B5%B1%E8%A8%88%E5%AD%B8-%E5%A4%A7%E5%AE%B6%E9%83%BD%E5%96%9C%E6%AD%A1%E5%95%8F%E7%9A%84%E7%B3%BB%E5%88%97-p%E5%80%BC%E6%98%AF%E4%BB%80%E9%BA%BC-2c03dbe8fddf

t很大，p就很小，< 0.05，就代表接受了吧，接受 H0



#### 雙尾和

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx2si9q7fj30xg0hydog.jpg" alt="image-20201021171910329" style="zoom:50%;" />



####  單尾不需要和

##### 更有力

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx2t0yycnj30vw0bkwnz.jpg" alt="image-20201021171940301" style="zoom:50%;" />

### 

### Test Characteristics, Assumptions, and Modifications

#### 兩著就是不同時的特性

##### N大時，t大, 兩者就是不同

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3819qpt4ij20w40bmq41.jpg" alt="image-20201021172151896" style="zoom:50%;" />



##### Sp大，交集多，不容易能說不同，就是怎樣都還是有像相同

###### 瘦時很好說不同

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx2yvb58lj30wq0gedk8.jpg" alt="image-20201021172516783" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx35tglhkj30wa06i0wq.jpg" alt="image-20201021173158134" style="zoom:50%;" />



#### Small N也是有力的

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3b5ccpoj30xq0nadrl.jpg" alt="image-20201021173705468" style="zoom:50%;" />



#### S-pool Issue

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3d8x0xmj30uu0gcjwm.jpg" alt="image-20201021173906854" style="zoom:50%;" />



##### 之前假設了兩個群的 sd 是一樣的。。。

###### Find t --> df --> p

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3f3vxv6j30x80gsqaj.jpg" alt="image-20201021174053806" style="zoom:50%;" />



##### 之前還假設了 Gaussian。。 -- 可用 Non-Param test

###### 假設多, Power大 (?!)

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3qxt8ymj30uo0gsds7.jpg" alt="image-20201021175215927" style="zoom:50%;" />



#### 1-sided vs. 2-sided

###### 身高時一尾easier

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3tn16ejj30wk0f6wq6.jpg" alt="image-20201021175451838" style="zoom:50%;" />



#### p↓ (extreme value) --> Reject H0



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx3vsr8gfj30vk0a8q81.jpg" alt="image-20201021175656571" style="zoom:50%;" />





### t-test in code

- var_a : np does the maximum likelihood estimate of variance
- s : pool standard deviation
- t : t statistic
- df : degree of freedom

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkc3hpmfx8j30l00dc45w.jpg" alt="image-20201103170651818" style="zoom: 50%;" />



#### t-test exercise

- Does one advertisement have a better click through rate than another as determined by statistical significance?
- ![image-20201021183945851](https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx54d5ht6j30wg0k248y.jpg)



###### Should check, for "Bonferroni Correction"

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx53skq4qj30xg0h8dku.jpg" alt="image-20201021183912717" style="zoom:50%;" />



#### Does one is better than the other by stat. significance?

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjx554z8skj30xi06smz1.jpg" alt="image-20201021184031122" style="zoom:50%;" />

###### TODO: t-test w/ same or diff variance? or try both then compare?





##### Key points

- this data represents clicked for news on various advertisements but need not be th case.

  - or headlines
  - diff landing pages/ website designs/ logo
  - key points structure of data
  - compter sees are the numbers
  - doing comparisons at the same time

- how do i collect data? 

  - only i can answer
    - write a script to get my data off Hadoop; mapreduct spark ; web php&mysql
    - 3rd party software, then download as csv
  - ml side vs de side, the part we get the data will not be considered ML. That wouel just be regular programming or even maybe it's just navigating web then download csv.

- so, to understand the format is just ok.

- all data is the same

  


###### Try ex_ttest.py



### 0.01 vs 0.001

#### Why care?

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd993i0y0j30pc09igqm.jpg" alt="image-20201104171143908" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd99pu1djj30qw08w43t.jpg" alt="image-20201104171220565" style="zoom:50%;" />

![image-20201104171254363](https://tva1.sinaimg.cn/large/0081Kckwgy1gkd9aatgv4j30sm07on1y.jpg)

#### IMPORTANCES: N, std, & diff



### Chi-Square Test Statistic on CTR

###### Contingency Table

#### Q: To know if the diff in CTR btw Ad-A & Ad-B is "statistically significant" ?

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd9bfvctnj30te0doq8z.jpg" alt="image-20201104171400040" style="zoom:50%;" />



##### Chi-2 test statistic

- 非負數！

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381bul4tpj20ts0f8403.jpg" alt="image-20201104171423476" style="zoom:50%;" />



###### Chi-Square is not self-explanatory

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd9cj19dvj30sq0bsn1a.jpg" alt="image-20201104171502878" style="zoom:50%;" />





##### <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkd9czuj7oj30t60fejwy.jpg" alt="image-20201104171529649" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381a9eghcj20vy0g8q4c.jpg" alt="image-20201021230553830" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381ac6jnzj20uc09egmi.jpg" alt="image-20201104172535695" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxctwr4xpj30wg0g0jy9.jpg" alt="image-20201021230630656" style="zoom:50%;" />



#### Chi2 ↑ --> P Value ↓ --> Diff ↑ (Reject H0)

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxcvo1p9kj30xa0ge7d2.jpg" alt="image-20201021230812035" style="zoom:50%;" />



#### Contingency Table in Scipy

###### 

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxcw6n0njj30re08qju5.jpg" alt="image-20201021230841374" style="zoom:50%;" />



#### Chi2 distribution

Yates Correction

###### Fisher's exact test

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxcwno3mpj30u40eaahq.jpg" alt="image-20201021230909261" style="zoom:50%;" />



### CTR AB Test in Code (w/ Data Generator!)

- How doest that P-value change as we collect each sample?

- Also, interested in "what the drawbacks of using this type of stat test might be?"

  ==> Later in Bayesian AB

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import chi2, chi2_contingency

# contingency table
#        click       no click
#------------------------------
# ad A |   a            b
# ad B |   c            d
#
# chi^2 = (ad - bc)^2 (a + b + c + d) / [ (a + b)(c + d)(a + c)(b + d)]
# degrees of freedom = (#cols - 1) x (#rows - 1) = (2 - 1)(2 - 1) = 1

# short example

# T = np.array([[36, 14], [30, 25]])
# c2 = np.linalg.det(T)**2 * T.sum() / ( T[0].sum()*T[1].sum()*T[:,0].sum()*T[:,1].sum() )
# p_value = 1 - chi2.cdf(x=c2, df=1)

# equivalent:
# (36-31.429)**2/31.429+(14-18.571)**2/18.571 + (30-34.571)**2/34.571 + (25-20.429)**2/20.429


class DataGenerator:
  def __init__(self, p1, p2):
    self.p1 = p1	# the probability of click for Group1
    self.p2 = p2	# the probability of click for Group2

  def next(self):
    click1 = 1 if (np.random.random() < self.p1) else 0
    click2 = 1 if (np.random.random() < self.p2) else 0
    return click1, click2	# 保證了模擬的 A&B一樣多；雖然 chi2's contengency table 沒要求 A&B 要一樣多


def get_p_value(T):	# Take in contengency table
  # same as scipy.stats.chi2_contingency(T, correction=False)
  det = T[0,0]*T[1,1] - T[0,1]*T[1,0]
  c2 = float(det) / T[0].sum() * det / T[1].sum() * T.sum() / T[:,0].sum() / T[:,1].sum()
  p = 1 - chi2.cdf(x=c2, df=1)
  return p


def run_experiment(p1, p2, N):
  data = DataGenerator(p1, p2)
  p_values = np.empty(N)
  T = np.zeros((2, 2)).astype(np.float32)
  for i in range(N):
    c1, c2 = data.next()
    T[0,c1] += 1
    T[1,c2] += 1
    # ignore the first 10 values	==> Why?!! ==> to prevent ValueError (divided by 0)
    if i < 10:
      p_values[i] = None
    else:
      p_values[i] = get_p_value(T)
  plt.plot(p_values)
  plt.plot(np.ones(N)*0.05)	# assume using alpha of 5%
  plt.show()

run_experiment(0.1, 0.11, 20000)

```

- P-Value should be **below** significant threshold

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381akovilj20vq0nijtc.jpg" alt="image-20201106152905452" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfhj4h25oj30pu0jkwhe.jpg" alt="image-20201106152918164" style="zoom:50%;" />



##### P-Value might be problematic!



### Chi2 Exercise

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381anw1o0j20ws0hkq3s.jpg" alt="image-20201106154058146" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfhvt0usdj30wu082whf.jpg" alt="image-20201106154129575" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfhw2uya4j30nq0eatbx.jpg" alt="image-20201106154145441" style="zoom:50%;" />







### Bonferroni Correction

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkffzqak32j30mm064dhs.jpg" alt="image-20201106143601121" style="zoom: 33%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381aso5i1j20x00eyacd.jpg" alt="image-20201106143745713" style="zoom:50%;" />

- 實驗做多了會significant, 同個實驗做兩次ＸＤ。。。

#### １

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381avt9t4j20lk08q3z1.jpg" alt="image-20201106144009651" style="zoom: 50%;" />

#### ２

![image-20201106144030549](https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg4cjhgoj30p40au42l.jpg)



#### ３Post Hoc Testing

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg4u7ctuj30rw098grw.jpg" alt="image-20201106144059138" style="zoom:50%;" />

- More elegant in Bayesian AB Testing...



### Power

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg66ggcmj30l609sae1.jpg" alt="image-20201106144216367" style="zoom:50%;" />



![image-20201106144250249](https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg6s312cj30uk0gu45q.jpg)



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg87ll4oj30uo0c4wjb.jpg" alt="image-20201106144412954" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfg9fy07pj30vk0bsahj.jpg" alt="image-20201106144524721" style="zoom:50%;" />



### AB Testing Pitfalls

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgbn89ukj30vw0ei12y.jpg" alt="image-20201106144731638" style="zoom:50%;" />



##### def of P-Values

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgdmbkylj30vw0a6wk0.jpg" alt="image-20201106144925031" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381b49j2uj20w60eyq4q.jpg" alt="image-20201106145338937" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgiytq9lj30rc0g47ai.jpg" alt="image-20201106145433831" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgjjgqcdj30wq0fmgxd.jpg" alt="image-20201106145505902" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgka3wn0j30vy09kgqc.jpg" alt="image-20201106145549425" style="zoom:50%;" />



# Summary

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkfgl9p4t7j30ui0a8jyk.jpg" alt="image-20201106145645951" style="zoom:50%;" />



#### t Summary

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxe1yiwqfj30um0e8tij.jpg" alt="image-20201021234850706" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxe3u0a7cj30u60dsgwj.jpg" alt="image-20201021235039422" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxe4h9kkaj30va0d8wob.jpg" alt="image-20201021235115920" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gjxeb4tqz0j30mw09sadg.jpg" alt="image-20201021235739720" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381bdnczgj20uw0fotb6.jpg" alt="image-20201021235849357" style="zoom:50%;" />

- FP - type I error



