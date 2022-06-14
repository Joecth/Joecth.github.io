---

layout: post
categories: RL
tag: [] 
date: 2018-11-26

---



# High Level of RL



## What's RL?

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr9q2h3ejj30xe07y0ut.jpg" alt="image-20201217232618652" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr9qxutefj31380ly10y.jpg" alt="image-20201217232708388" style="zoom:50%;" />

- RL: does things that humans can do, which can be very dynamic



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glr9szui5ej30vg0ieadx.jpg" alt="image-20201217232906830" style="zoom:50%;" />

- Cluster: some example: return a mapping to some vector or a cluster identity
- They are too similar that make sense to put them in the same lib



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrawol676j311q0kaalq.jpg" alt="image-20201218000715482" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glraxwvom2j31240lsqke.jpg" alt="image-20201218000826506" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrb5g87r8j31280lmttn.jpg" alt="image-20201218001538879" style="zoom:50%;" />

- static: has no life time



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbaesicsj312o0jm7i6.jpg" alt="image-20201218002026886" style="zoom:50%;" />

- Infeasible amount of input data for comparison

- Also, we want to allow for creativity and stochastic

- A supervised modeleven if it were feasible to train would only have one target per input so it would never be able togenerate poetry (RNN?!)



## On Unusual or Unexpected Strategies of RL

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381w25916j21260kiq59.jpg" alt="image-20201218002544266" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbhg4jyej312a0l047o.jpg" alt="image-20201218002713034" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbk62m9cj311y0ky15y.jpg" alt="image-20201218002949873" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbl0xdk9j310e0mgqay.jpg" alt="image-20201218003039627" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrblb9cumj31240iw48a.jpg" alt="image-20201218003055704" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381w5vnfmj210q0icjv1.jpg" alt="image-20201218003145761" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbmk7bh5j30ys0lctgu.jpg" alt="image-20201218003207405" style="zoom:50%;" />







## Bandits --> RL

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbo12jx8j310e0ls10b.jpg" alt="image-20201218003332356" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrborlhnuj30zu0lk44v.jpg" alt="image-20201218003415381" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl2yzm0j9dj30sc0amq79.jpg" alt="image-20201126230119135" style="zoom:50%;" />

#### Contextual Bandits!

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl2z1azi2pj30vc0iwn3k.jpg" alt="image-20201126230256945" style="zoom:50%;" />

###### Input features as States



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl2z1tluzsj31000iyagd.jpg" alt="image-20201126230327507" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gl2z33sq3rj30vs0hen22.jpg" alt="image-20201126230441694" style="zoom:50%;" />

###### Compare rewards



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381w8n7lkj212m0mc415.jpg" alt="image-20201218003921597" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrburicmaj312o0li10i.jpg" alt="image-20201218004000861" style="zoom:50%;" />





<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbvj8ntsj30yw0lwjxa.jpg" alt="image-20201218004045423" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbvtwdooj30zg0lu455.jpg" alt="image-20201218004102606" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glrbwaagcwj31100m67b4.jpg" alt="image-20201218004128410" style="zoom:50%;" />



###### GO also state-sequence dependent:

- There're some environments where it's not just the state by itself that matters but rather the sequence of the states ==> MDP



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h381wbh53qj20zs0iy0tx.jpg" alt="image-20201127145128803" style="zoom:50%;" />

- First, I looked at the MAB problem once and defined some new terms to help thnk about the problem more abstractly specifically
- MAB: CHOOSING an ACTION to oatain the best REWARD
- Next, the Contextual bandit problem, where instead of just having to choose an action, also have to pay attention to the state, which help us have more fine grained control on which action to choose!
- Finally, some foreshadowing and know sittuation where instead of just random states which are not related to one another in terms of predicting a reward, we can have states which are interdependent!