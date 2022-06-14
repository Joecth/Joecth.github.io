---
layout: post
categories: RL
tag: [] 
date: 2018-11-26
---



# Markov Decision Processes (MDPs)



## Intro

- a kind of framework

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381uf1kxhj20xk0g4myj.jpg" alt="image-20201229205217435" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm50uer7jaj30sy0i2n1v.jpg" alt="image-20201229205659381" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm50whswfuj30we0iak02.jpg" alt="image-20201229205859709" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm50yjni88j30w40hc41l.jpg" alt="image-20201229210058158" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm50zz9wlpj30ua0hy0y8.jpg" alt="image-20201229210221158" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm511p9irej30w00hydrm.jpg" alt="image-20201229210400130" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm512hvnr1j30w20ia7i3.jpg" alt="image-20201229210445704" style="zoom:50%;" />



## Gridworld

- grid world is the perfrct size env to learn about RL!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm513wq4v6j30xu0jcjz4.jpg" alt="image-20201229210607559" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm51iv1hloj30yu0hsady.jpg" alt="image-20201229212029049" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm51klvqutj30xa0foq5v.jpg" alt="image-20201229212210402" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52annhthj30y20iw49k.jpg" alt="image-20201229214711945" style="zoom:50%;" />



### Terms

##### Episode

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52rqwacjj30xu0gydml.jpg" alt="image-20201229220337651" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52sr29lhj30vg0iaaie.jpg" alt="image-20201229220435948" style="zoom:50%;" />



### Environment

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52umyc0tj30ve0hsn16.jpg" alt="image-20201229220624845" style="zoom:50%;" />



### Policy

- Acts as the agents brain and tells it how to map states to actions

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52wn25hoj30ua0j2whi.jpg" alt="image-20201229220819982" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52ypmfvzj30we0hkaf3.jpg" alt="image-20201229221019687" style="zoom:50%;" />



##### Action Space

- the set of all possible actions

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm52zxmratj30xk0jon13.jpg" alt="image-20201229221129902" style="zoom:50%;" />



- 92.5% prob to the action stored in the dict; 2.5% to the rest of the actions in the action space 
  - **action space**: analogous to the state space, is the set of all possible actions



##### State space

- the set of all possible states

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381uru88gj20om0bmjrz.jpg" alt="image-20201229221826721" style="zoom:50%;" />





## Choosing Rewards

- My job: to design the reward in such a way that it results in the behavior that I want from my agent
- get an agent to solve maze

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381uv2u83j20ya0j4adg.jpg" alt="image-20201229222306498" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm53cpsjvlj30xo0jen86.jpg" alt="image-20201229222346858" style="zoom:50%;" />



- ME: to tructure my rewards in a way that is conducive to the agents solving the env



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381v1n35pj20y00iaq66.jpg" alt="image-20201229222616031" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381vkv77lj20wm0jqq4k.jpg" alt="image-20201229222643355" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm53hesbshj30xi0jk0yw.jpg" alt="image-20201229222818176" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gm53hok2b2j30x80k2q93.jpg" alt="image-20201229222833511" style="zoom:50%;" />



## The Markov Property



## Markov Decision Processes (MDPs)





## Future Rewards



## Value Functions





## The Bellman Equation



## Optimal Policy & Optimal Function



## Summary of MDP

