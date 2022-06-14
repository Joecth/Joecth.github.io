---
layout: post
categories: RL
tag: [] 
date: 2018-11-05

---



# MAB -- Bayesian AB Testing

## Explore-Exploit Dilemma

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7iwt959j31120840w4.jpg" alt="image-20201110173053477" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7jej62ij30yc0jq7au.jpg" alt="image-20201110173124688" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7khaq7lj30ye0l8n3g.jpg" alt="image-20201110173226758" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381wxx801j210g0jodhs.jpg" alt="image-20201110173322193" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7mnki67j311i0ga0z8.jpg" alt="image-20201110173432306" style="zoom:50%;" />



#### Why Against Greey on the maximum likelihood estimate of win rate now?

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7oolfu9j30z40js7df.jpg" alt="image-20201110173629144" style="zoom:50%;" />



##### Effect size

- it's related to the difference btw the win-rate of the two slot machines

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7r8m39bj311s0juk0q.jpg" alt="image-20201110173856256" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7vhhpd9j310o0ictj2.jpg" alt="image-20201110174301320" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7wtb6w8j30y40lk46h.jpg" alt="image-20201110174418284" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk7xqhob8j30xw0jw7bt.jpg" alt="image-20201110174511335" style="zoom:50%;" />

- Algs can be used in place of a traditional AB test in software system
- Methods that overcome some of the awkard problems like effect size, wanting to stop my experiment early and the controversial P-value.
- **Adaptive**, meaning that they learn on the fly, ∴ advantageous, especialy in high throughput online business settings.



### Applications

![image-20201110175052050](https://tva1.sinaimg.cn/large/0081Kckwgy1gkk83n89tgj30yw082adg.jpg)

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk84dvwwdj310u0jugu1.jpg" alt="image-20201110175134119" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk85p1cx7j31120hyq8h.jpg" alt="image-20201110175250231" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xcghimj211m0jc0up.jpg" alt="image-20201110175336380" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkk87yzs5rj311g0kcn8u.jpg" alt="image-20201110175501274" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xice2sj211m0hwju8.jpg" alt="image-20201111161819125" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklb2mo08hj312c0lwh26.jpg" alt="image-20201111161914956" style="zoom:50%;" />



##### Gaussian vs. Bernoulli

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklb3n1rduj311w0kytde.jpg" alt="image-20201111162013595" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklb4hjtr7j311006sq69.jpg" alt="image-20201111162102271" style="zoom:50%;" />





## 1 -- ε-Greedy

- to Balance exploration and exploitation
- Short-sighted
- use only immediately available info as a heuristic

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklb5gqlc4j30y40i6dlr.jpg" alt="image-20201111162158881" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xle3aej21260kcq5t.jpg" alt="image-20201111162516984" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbadmawjj311k0gs43b.jpg" alt="image-20201111162642379" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbbhdlo7j310q0ka0y2.jpg" alt="image-20201111162746138" style="zoom:50%;" />



- once we have X, we can update the estimate for the mean of the bandit we selected.

##### Additional Details

- Want to collect each data abou each bandit!
  - ∵ want estimate of win-rate to be accurate
- But at 
  - What point shouldwe stop exploring ? 
  - What happens if we let the algorithm run forever?
    - if not stop exploring, ==> total collective reward will be suboptimal...
- By definition, if one of the bandits is optimal, then the other bandits are not optimal.

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbszfncdj31200k8dmp.jpg" alt="image-20201111164434393" style="zoom:50%;" />



#### Decaying Epsilon

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbuxw8ulj310o0h6wis.jpg" alt="image-20201111164627855" style="zoom:50%;" />



### Calculating a Sample Mean

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbwp331wj30wg0i40w2.jpg" alt="image-20201111164808831" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbxwofw5j313m0eaq6h.jpg" alt="image-20201111164918981" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr9fu5673j312s0lmjvr.jpg" alt="image-20201217231627781" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr9g2xgaqj312o0lygpv.jpg" alt="image-20201217231642310" style="zoom:50%;" />

###### If the values of the RV can only be 0or 1, thenthesample mean is exactly the MLE of the Bernoulli param





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklbz4pmlmj31000lu796.jpg" alt="image-20201111165029387" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gklc06yks6j30tm0jqwjc.jpg" alt="image-20201111165130844" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkld3xquubj31220j2tdo.jpg" alt="image-20201111172942379" style="zoom:50%;" />



#### Rolling mean

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xolo91j212c0k0tae.jpg" alt="image-20201111173021174" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xrxw6lj20zq0k8763.jpg" alt="image-20201111173948929" style="zoom:50%;" />



#### Constant Time & Space for Sample Mean estimation

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkldf47f1jj30w20lujw7.jpg" alt="image-20201111174027387" style="zoom:50%;" />



### Exercise

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkldiimo4cj311q0gon2r.jpg" alt="image-20201111174342893" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkldkkdb86j31200iiwkb.jpg" alt="image-20201111174541096" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xucz2dj21lq0ry7d8.jpg" alt="image-20201112114635385" style="zoom: 67%;" />





### Designing My Bandint Program

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381xxltrqj210c0kiwg3.jpg" alt="image-20201112105916065" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkm7gvw2inj30yo0katei.jpg" alt="image-20201112110006940" style="zoom:50%;" />



==> Also pattern to be followed for My Bandit Prog.



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkm7jpesxhj312s0gywop.jpg" alt="image-20201112110249987" style="zoom:50%;" />



### My Code's Result

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381y0twbcj20x10u0q4g.jpg" alt="image-20201112154844278" style="zoom: 33%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381y3zh13j20xr0u0q4t.jpg" alt="image-20201112163522404" style="zoom:33%;" />



### Cmp Diff ε w/ Reward in Real Value as  ~ N(μ, σ**2)

- I have
  1. BanditArm class
  2. run_exp()
  3. print & plot()
- Changes:
  - pull()
    - as reward distribution has changed from Bernoulli to Gaussian
    - <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381y7pecjj20rw03gt8s.jpg" alt="image-20201117173314978" style="zoom:33%;" />
  - plt.xscale('log') 's REASON
    - Alg. will converge so fast, so it's difficult to truly see the differences btw each value of EPSILON./
    - Using a log scale allows me to zoom-in to the relevant parts of the plot.
  - Return the cumulative average reward from this functin so that in the main section, I'll can plot the cumulative averages for DIFFERENT values of EPSILON in the same plot.

- No Changes:
  - Update()
    - Since same way I calc the sample mean
  - run_experiment()

- My main()
  - 3 bandits w/ diff means: 1.5, 2.5, and 3.5
  - ε I'll try: 0.1, 0.05, 0.01
  - 100,000 trials for each experiment
  - <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gksaxt9402j30r007ewfe.jpg" alt="image-20201117173334595" style="zoom:33%;" />



### For Real Data & Online Learning

- The hypothesis tests work on static datasets (a data file).

  ​					↑  →←  ↓

- Epsilon-greedy works in real-time systems adapting to users who enter data into the system at different times. It's not a static data file.

- Bandit Summary, Real Data, and Online Learning

- Adaptive Ad service for the Streaming







## 2 -- Optimistic Initial Values



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkmi7fzl0cj312q0jmahe.jpg" alt="image-20201112171136617" style="zoom:50%;" />

#### Pseudocode

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkmi9u8tj3j30rw0gugp4.jpg" alt="image-20201112171356320" style="zoom: 33%;" />

- it's an overestimate of the true mean



### Choosing Bandit

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkmiex4oohj310c0k6n52.jpg" alt="image-20201112171848566" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkmijzpu1hj31160k2doi.jpg" alt="image-20201112172340672" style="zoom:50%;" />



- even for the optimal one,we can't expect it to have a good estimate of the true mean, because the ini value could have so **HIGH**, and the number of trials **so LOW enough** that the estimted mean is an overestimate by the end of the experiment



### Init value meaning

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381ya53ylj20z00b475k.jpg" alt="image-20201112172900427" style="zoom: 33%;" />



### Exercise

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkmjbqqmn9j30xi0k6gwd.jpg" alt="image-20201112175021736" style="zoom:50%;" />



 

### MyCode

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381ycfdlxj20rg0gujrv.jpg" alt="image-20201112175755223" style="zoom:33%;" />

- No gap btw final cumulative rewards rate and the true optimal win rate, which is 0.75

```zsh
mean estimate: 0.5454545454545454
mean estimate: 0.5958904109589042
mean estimate: 0.753300832825516
total reward earned: 7495.0
overall win rate: 0.7495
num times selected each bandit: [11.0, 146.0, 9846.0]
```

- For optimal bandit, the estimated mean will probably slowly climb down from the initial value, down to 0.75 eventually.
  - So if we start at 5, it's going to go down from 5 to 0.75
- For other suboptimal bandits, they are going to stop being selected if they go anywhere below 0.75, since we're being **greedy**, ∴ no chance to converget to true means
  - stop getting updated...
- suboptimal 沒怎麼被選到！



### Sum:

- make the mean estimation artificially high, so that ech bandit gets chosen more often until we learn the true mean is not that actually high



## 3 -- UCB1 

- Each of the ideas take the prev idea and makes it a little more complex!

##### OIV :

- make the mean estimation artificially high, so that ech bandit gets chosen more often until we learn the true mean is not that actually high

##### UCB1

- use the Prob. to infer a upper bound!

![image-20201202231448981](https://tva1.sinaimg.cn/large/0081Kckwgy1gl9x3jwv6xj31420mu10f.jpg)



![image-20201202233509282](https://tva1.sinaimg.cn/large/0081Kckwgy1gl9xootsubj31020lin2a.jpg)





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl9xw19lavj31180lon1s.jpg" alt="image-20201202234214674" style="zoom:50%;" />

![image-20201202234054598](https://tva1.sinaimg.cn/large/0081Kckwgy1gl9xuno45xj31140logq4.jpg)



###### Sum 

The prob of being larger than a large Error is smaller; 

The prob of being larger than a smaller Error is larger

##### Q: What's the prob that my Error is bigger than some T?



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381yfakphj212w0iagnc.jpg" alt="image-20201202234726793" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl9y3a2xnij30yw0iq44g.jpg" alt="image-20201202234912413" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl9y5fvmr8j31220le116.jpg" alt="image-20201202235116829" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl9y73fl1qj30zg0lmjvm.jpg" alt="image-20201202235252606" style="zoom:50%;" />



1. sample mean on bandit-j
2. **error upper bound**! Like the optimistic initial values method, except that
   instead of using an optimistic estimate for the mean, we use the actual sample mean, plus some upper bound on the error
3. JUST think of it as using the upper bound on the confidence interval as intuition
4. Still greedy wrt the upper bound!



### CI Intuition

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl9ytkrvwij31100kowlf.jpg" alt="image-20201203001428672" style="zoom:50%;" />



1. Case #1:
   - Haven't collected a lot of samples yet
   - estimate is bad; CI is large!  ==> I want to explore this bandit, so that I can collect more samples
   - Using  upper bound of the confidence interval would be useful since it would be very high, and if I take the bandit with the maximum upper bound, then I can explore this bandit
2. Case #2:
   - data Ok, so my estimate is very accurate and I no longer need to explore this bandit
   - The only way I can explore this bandit is if it's true mean is actually high and is higher than the upper bound on the other bandits
3. Upper bound of a confidence interval allows me to explore and exploit in a reasonable way!



### EE in UCB1

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381ykt36aj21240m8q4y.jpg" alt="image-20201203003218896" style="zoom:50%;" />

- 2 : merely heuristic





### Why does it work?

![image-20201203155508009](https://tva1.sinaimg.cn/large/e6c9d24egy1h3820y7h7xj210s0j840g.jpg)



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glaq3c3wf4j30u20haads.jpg" alt="image-20201203155759465" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381yswdjnj20wq0k8wgy.jpg" alt="image-20201203162128500" style="zoom:50%;" />



### Code & Result!

```python
import numpy as np
import matplotlib.pyplot as plt

NUM_TRIALS = 100000
EPS = 0.1
BANDIT_PROBABILITIES = [0.2, 0.5, 0.75]

np.random.seed(1)

class Bandit:
    def __init__(self, p):
        # p: the win rate
        self.p = p
        self.p_estimate = 0.
        self.N = 0.  # num samples collected so far

    def pull(self):
        # draw a 1 with probability p
        return np.random.random() < self.p

    def update(self, x):
        self.N += 1.
        self.p_estimate = ((self.N - 1) * self.p_estimate + x) / self.N


def ucb(mean, n, nj):  # TODO
    return mean + np.sqrt(2 * np.log(n) / nj)


def run_experiment():
    bandits = [Bandit(p) for p in BANDIT_PROBABILITIES]
    rewards = np.empty(NUM_TRIALS)
    total_plays = 0

    # initialization: play each bandit once
    for j in range(len(bandits)):
        x = bandits[j].pull()
        total_plays += 1
        bandits[j].update(x)

    for i in range(NUM_TRIALS):
        # j = # TODO
        # j = np.argmax([ucb(b.p_estimate, i, b.N) for b in bandits])
        j = np.argmax([ucb(b.p_estimate, total_plays, b.N) for b in bandits])
        x = bandits[j].pull()
        total_plays += 1
        bandits[j].update(x)

        # for the plot
        rewards[i] = x
    cumulative_average = np.cumsum(rewards) / (np.arange(NUM_TRIALS) + 1)

    # plot moving average ctr
    plt.plot(cumulative_average)
    plt.plot(np.ones(NUM_TRIALS) * np.max(BANDIT_PROBABILITIES))
    plt.xscale('log')
    plt.show()

    # plot moving average ctr linear
    plt.plot(cumulative_average)
    plt.plot(np.ones(NUM_TRIALS) * np.max(BANDIT_PROBABILITIES))
    plt.show()

    for b in bandits:
        print(b.p_estimate)

    print("total reward earned:", rewards.sum())
    print("overall win rate:", rewards.sum() / NUM_TRIALS)
    print("num times selected each bandit:", [b.N for b in bandits])

    return cumulative_average


if __name__ == '__main__':
    run_experiment()

```



## 4 -- Bayesian Bandits / Thompson Sampling Theory

digression

#### CI as tool for nice picture

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381yw6i5vj20ww0ksmz8.jpg" alt="image-20201207235724448" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glfqgbc6a8j30wa0kywkf.jpg" alt="image-20201207235819616" style="zoom:50%;" />



- 代表true mean的可能在的地方，就是分布

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glfqhtg8nuj30wa0heq82.jpg" alt="image-20201207235946520" style="zoom:50%;" />





### Bayes Rule

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381yzgy59j20zc0l4dhy.jpg" alt="image-20201208000356712" style="zoom:50%;" />



### Why Proportionality?

![image-20201208000454225](https://tva1.sinaimg.cn/large/0081Kckwgy1glfqn5d3e4j310a0kqjxt.jpg)



### Conjugate Pairs/Priors

- In general, the posterior doesn't fit nicely into one of these common distributions
  - E.g. I can't say if my likelihood is a Gaussian AND my prior as a uniform, then my posterior will be a Gaussian, that ==> WON'T WORK
- However, Conjugate Priors is SPECIAL,
  - The posterior comes from the same kind of distribution as the prior
  - If prior is a Gaussian, then. ==> posteriror will also be a Gaussian
  - This is contingent on the fact that my likelihood matches the prior such that they are conjugate!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glfqp78xjyj30z00kswlu.jpg" alt="image-20201208000652576" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glfr8dtfdsj31060kmtdl.jpg" alt="image-20201208002518384" style="zoom:50%;" />



##### Bernoulli Conjugate Prior

##### BETA <== Bernoulli * BETA(α, β)

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381z2sb57j210q0kymzs.jpg" alt="image-20201208002712077" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381z6j8ruj20vy0jgwfr.jpg" alt="image-20201208003115449" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glfrfanrgpj30ve0l0te1.jpg" alt="image-20201208003156674" style="zoom:50%;" />





### Choosing a Prior

- How to choose value of  α  &  β?

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381z9868hj20v20is75h.jpg" alt="image-20201208234648887" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgvrkf8wnj30w20fi789.jpg" alt="image-20201208234741722" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zc4y5pj20yi0kan0n.jpg" alt="image-20201209000022630" style="zoom:50%;" />





## 4 -- Bayesian Bandits (Cont.)

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgw71aeitj30yk0kmdop.jpg" alt="image-20201209000233644" style="zoom:50%;" />



- This posterior represents our belief about the distribution of theta, the mean of a bandit
  - Theta could be anywhere in this range, although obviously the areas with more probability mass are more probable.



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwg5az8lj30ye0kmwh7.jpg" alt="image-20201209001059200" style="zoom:50%;" />

-  By drawing samples from this distribution, we're saying give me a value from this distribution and let that determine which bandit I choose.
  - Instead of just using one value, which is the upper bound, I use all of its values, **according to the posterior distribution,** by drawing samples
  - As I sraw more and more samples, this distribution will become skinner and skinner as I get more confident in my belief of where the true mean lies



### Pseudocode

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwj3lp6lj30wu0kigpn.jpg" alt="image-20201209001410545" style="zoom:50%;" />



- Be greedy, but with respect to samples from the posteriors rather than some statistic estimated from the samples



### Graphs of the Posteriors!

#### Scene 1

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwq5tw3nj30za0kk79w.jpg" alt="image-20201209002056687" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwqixn3rj30zi0hegqs.jpg" alt="image-20201209002117789" style="zoom:50%;" />

- In addition, its peak moves up a bit, since getting a one increase the mean



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwtf04hqj30z80hk0vm.jpg" alt="image-20201209002404618" style="zoom:50%;" />



#### Scene 2

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwv805cej30zo0iawkt.jpg" alt="image-20201209002548692" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwvx7m9cj30z60he44l.jpg" alt="image-20201209002629349" style="zoom:50%;" />



#### Demo Plot!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwyi764lj30t00mcq7r.jpg" alt="image-20201209002857949" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgwzj1ay8j30t00mgwjg.jpg" alt="image-20201209002957232" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgx0p7qysj30t40m8dl0.jpg" alt="image-20201209003104989" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgx3vnbdnj30sa0lwaf6.jpg" alt="image-20201209003408494" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zfbzboj20ss0ksdhb.jpg" alt="image-20201209003504263" style="zoom:50%;" />





### Sum

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhvdscxofj311e0kewo0.jpg" alt="image-20201209202002097" style="zoom:50%;" />

- Intuitive idea: I'm interested in something like the CI of win-rate estimate
  - This perfectly encapsulates the EE dilemma. 
    - When CI is fat --> Explore!
    - When CI is small --> Exploite this fact, by choosing the bandit with the highest mean, which we are now confident about.
  - BUT! CI is based on the CLT, and don't portray the true distribution of the bandit win-rates
  -  Instead, the Bayesian treats the win-rate as a RV! which is to give it it's own distribution!
    - In general, computing a posterior from Bayes rule is not easy, because they usually involve intractable sums or unsolvable integrals
    - ★!!!★ SO, I use Conjugate Prior's to show that, instead of calculating the posterior by hand, we can instead use proportionality to prove that the shape of the posterior fits some particular distribution, and from there, the normalizing constant can be found since the integral of any distribution must equal to 1.
    - NEXT, introduce the Thompson Sampling algorithm: 
      - To choose a bandit, just rank each bandit, based on samples drawn from their posteriors. 
      - Over time, this does exactly what we want!
        - When distributions are FAT --> explore more
        - When distributions are Skinny --> explore less
      - Importantly, when the optimal distribution becomes skinny, we can leave the suboptimal distributions FAT, since in this scenario, the optimal bandit is still usually has the highest sample.
        - This leads to us choosing the optimal bandit more, which is how we exploit.



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhw2nvxdpj31120lik0q.jpg" alt="image-20201209204355453" style="zoom:50%;" />





### Code!

```python
  def sample(self):
    return np.random.beta(self.a, self.b)

	for i in range(NUM_TRIALS):
    # Thompson sampling
    j = np.argmax([b.sample() for b in bandits])
    # Says, give me the bandit that yields the LARGEST sample from its current beta posterior

```



- Thompson Sampling is nice because the sub-optimal bandits only get pushed down far enough so that their posteriors hav very little probability mass beyond the peak of the optimal bandit!
  - They remain FAT posterior distributions
  - This is a good thing, because bein gfat means that they haven't been explored that much!
  - And this tells us that we explored just enough to be very confident that their means are not better than the optimal bandits mean





## Thompson Sampling With Gaussian Reward Theory

* No more study this topic, leave in the future... Terrible MATH...

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhx504b5ij30wu0ly44r.jpg" alt="image-20201209212045609" style="zoom:50%;" />





#### .....



#### Solving for posterior

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhxou8fvjj31120gmdl3.jpg" alt="image-20201209213949750" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhxq665ifj310u0h2jwm.jpg" alt="image-20201209214108183" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhxqpfdyij311o0ikn40.jpg" alt="image-20201209214138178" style="zoom:50%;" />



![image-20201209214544500](https://tva1.sinaimg.cn/large/0081Kckwgy1glhxuzfw39j30y60jg446.jpg)





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhxwjnkpxj31320m0dm4.jpg" alt="image-20201209214715123" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhxyy669xj30ya0m6q83.jpg" alt="image-20201209214933115" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhy1dt8y1j311q0lqaiy.jpg" alt="image-20201209215153654" style="zoom:50%;" />



![image-20201209215247334](https://tva1.sinaimg.cn/large/0081Kckwgy1glhy2b8tbij311w0e079a.jpg)



#### Code!





## Prev

- In particular, does the P-value give us a definitive answer to our question?
- We are interested in what the drawbacks of using this type of statistical test might be?
  - important when Bayesian & AB



## Why no use a lib?

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381ziq5fdj21240kiq5s.jpg" alt="image-20201209215920654" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zmelv5j212g0iit9x.jpg" alt="image-20201209215958503" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhybf81emj312g0kuwna.jpg" alt="image-20201209220128303" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zoqzsmj211o0lgdi2.jpg" alt="image-20201209220214547" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhydxbz5cj311a0m6gsm.jpg" alt="image-20201209220354590" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhyfkvxk4j30zk0l2gsn.jpg" alt="image-20201209220530442" style="zoom:50%;" />



## Nonstationary Bandits

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zr3zbfj20tw0fu0tr.jpg" alt="image-20201122215546694" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhylfvs0jj31360lmwm1.jpg" alt="image-20201209221108933" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glhynbe4ebj313m0kg12w.jpg" alt="image-20201209221255979" style="zoom:50%;" />







<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkyao03qm1j30yy0iwjyb.jpg" alt="image-20201122215732149" style="zoom:50%;" />







<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zuz8i7j210a0lkq5o.jpg" alt="image-20201209222952053" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381zxoiylj20wy0juac4.jpg" alt="image-20201122220315820" style="zoom:50%;" />



![image-20201122220554471](https://tva1.sinaimg.cn/large/0081Kckwgy1gkyawpt8dij30y20jmwjl.jpg)





## Bandit Summary, Real Data, & Online Learning



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38200wxy8j20pq0ciab0.jpg" alt="image-20201122223631239" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkybuimi0yj30pe0jctd2.jpg" alt="image-20201122223824159" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38206jwerj20zc0jidhl.jpg" alt="image-20201122224114107" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gkybyso8bkj310c0jkgtj.jpg" alt="image-20201122224230826" style="zoom:50%;" />

![image-20201122224351405](https://tva1.sinaimg.cn/large/0081Kckwgy1gkyc06zjs4j31020gwk19.jpg)



## Alternative Bandit Designs

### ![image-20201122230100244](https://tva1.sinaimg.cn/large/e6c9d24egy1h3820uweugj210s0ieq5k.jpg)









```mermaid
classDiagram
	ReservationRequest
	class BanditArm{		# for ε-Greedy
		- p # mean of Ground Truth
		- p_estimate	# Maximum Likelihood
		- N	# samples seen so far
	  pull()
	  update(xi)
	}	
```





# My Youtube Surveys

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3820c2z0kj20n60ewt9g.jpg" alt="image-20201208183843862" style="zoom:50%;" />

https://www.youtube.com/watch?v=aWKeSvnTalE&feature=youtu.be

#### ε-Greedy's Issue 

spends equal amount of time exploring arms it knows are not optimal!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glgmvys36lj30ni068jug.jpg" alt="image-20201208184032472" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3820fg9bcj20t20iign6.jpg" alt="image-20201208204846943" style="zoom:50%;" />





#### https://www.youtube.com/watch?v=wcCSAbcj5Q0

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3820ibqy3j218k0oa775.jpg" alt="image-20201214165604645" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glnhve9k1kj318k0n6qh2.jpg" alt="image-20201214170556913" style="zoom:50%;" />





#### https://www.youtube.com/watch?v=o6HBIGzQfJs

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glnil2lmqzj315g0nqh1w.jpg" alt="image-20201214173037639" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glniqb7xn9j31620nq12q.jpg" alt="image-20201214173539974" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glnj5gjuoxj314w0n2dpm.jpg" alt="image-20201214175013432" style="zoom:50%;" />

