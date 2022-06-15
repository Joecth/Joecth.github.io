---
layout: post
categories: AWS-Hhigh-Traffic
date: 2022-01-16
---





## Metrics

```bash
  994  sudo amazon-linux-extras install epel -y
  995  sudo yum install stress -y
  996  stress --cpu 1 --timeout 60
```



<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gygjoc09usj31a60k4whm.jpg" alt="image-20220117115535663" style="zoom:50%;" />

*with like 5 minutes delay after the command fired in shell, to see the update in chart*





## Customized Metrics

### Dimension

```bash
aws cloudwatch put-metric-data --namespace UploadPage002 --metric-name FileSize --value 10 --dimensions InstanceId-A01
aws cloudwatch put-metric-data --namespace UploadPage002 --metric-name FileSize --value 10 --dimensions InstanceId-A02
```

![image-20220117120712044](https://tva1.sinaimg.cn/large/008i3skNgy1gygjob9vigj31b40kcgor.jpg)





### Unit

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gygjoctv8yj313o09qdhl.jpg" alt="image-20220117120916979" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gygjocf1eij31bg0gctac.jpg" alt="image-20220117121048327" style="zoom:67%;" />







## Alarm

![image-20220117192748503](https://tva1.sinaimg.cn/large/008i3skNgy1gygw7ywvpjj31im0u0whc.jpg)

![image-20220117192804747](https://tva1.sinaimg.cn/large/008i3skNgy1gygw896lr0j31jd0u0jvv.jpg)

![image-20220117192825190](https://tva1.sinaimg.cn/large/008i3skNgy1gygw8m0p6wj31ix0u0dkl.jpg)

![image-20220117192733936](https://tva1.sinaimg.cn/large/008i3skNgy1gygw7rlembj31820sudin.jpg)

![image-20220117192847074](https://tva1.sinaimg.cn/large/008i3skNgy1gygw8z706gj313q0swwh0.jpg)

![image-20220117194107269](https://tva1.sinaimg.cn/large/008i3skNgy1gygwltb1bpj318i0c8gmy.jpg)

![image-20220117194203245](https://tva1.sinaimg.cn/large/008i3skNgy1gygwmsbln3j31ak0ksac0.jpg)

![image-20220117194209706](https://tva1.sinaimg.cn/large/008i3skNgy1gygwmvz8t0j31d60myjtq.jpg)

![image-20220117194221239](https://tva1.sinaimg.cn/large/008i3skNgy1gygwn3jd5bj31di0ron03.jpg)

![image-20220117194233395](https://tva1.sinaimg.cn/large/008i3skNgy1gygwnb3ilfj31ei0lcq7j.jpg)

![image-20220117194243823](https://tva1.sinaimg.cn/large/008i3skNgy1gygwnhkhflj314w0dq76x.jpg)

![image-20220117194252584](https://tva1.sinaimg.cn/large/008i3skNgy1gygwnmu76wj31dm0rwn0f.jpg)



#### 5 points within 1 period, so still OK

![image-20220117194357359](https://tva1.sinaimg.cn/large/008i3skNgy1gygworeouqj31ae09egnc.jpg)

![image-20220117194404710](https://tva1.sinaimg.cn/large/008i3skNgy1gygwow7arej315w0nadhq.jpg)

![image-20220117194424754](https://tva1.sinaimg.cn/large/008i3skNgy1gygwp8iis0j31b40ns0uv.jpg)





## Event Bridge

![image-20220117194526046](https://tva1.sinaimg.cn/large/008i3skNgy1gygwqbjw2ij31kq0oqgp6.jpg)

![image-20220117203918341](https://tva1.sinaimg.cn/large/008i3skNgy1gygyacwkn1j31dm0by3zy.jpg)

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gygyaiq6oej30so07qmxp.jpg" alt="image-20220117203928164" style="zoom:50%;" />

![image-20220117204030297](https://tva1.sinaimg.cn/large/008i3skNgy1gygybmasiqj31ks0s8gqx.jpg)

![image-20220117204133574](https://tva1.sinaimg.cn/large/008i3skNgy1gygycpdesjj317m0u0gnx.jpg)

![image-20220117204156500](https://tva1.sinaimg.cn/large/008i3skNgy1gygyd3jalfj31bw0ti0vj.jpg)

![image-20220117204148425](https://tva1.sinaimg.cn/large/008i3skNgy1gygycyi9kvj315g0r4dhu.jpg)

![image-20220117204223835](https://tva1.sinaimg.cn/large/008i3skNgy1gygydkbv9sj313u0mktat.jpg)

![image-20220117204237844](https://tva1.sinaimg.cn/large/008i3skNgy1gygydttc2wj31bq0qo776.jpg)

![image-20220117204254131](https://tva1.sinaimg.cn/large/008i3skNgy1gygye3pf18j31660t0n0j.jpg)

![image-20220117204349480](https://tva1.sinaimg.cn/large/008i3skNgy1gygyf1vlvtj316m0s4jv1.jpg)

![image-20220117204407635](https://tva1.sinaimg.cn/large/008i3skNgy1gygyfd8gt4j31ga0qkgp7.jpg)

![image-20220117204435502](https://tva1.sinaimg.cn/large/008i3skNgy1gygyfur28gj317q0qadih.jpg)

![image-20220117204559083](https://tva1.sinaimg.cn/large/008i3skNgy1gygyhaw0elj31ee0qsaeu.jpg)



#### Summary

![image-20220117204653285](https://tva1.sinaimg.cn/large/008i3skNgy1gygyi8oqy6j31240ro40q.jpg)

![image-20220117204701948](https://tva1.sinaimg.cn/large/008i3skNgy1gygyieiqajj31ig0te0xp.jpg)





## Monitoring remaining disk 

```bash
 sudo yum install -y amazon-cloudwatch-agent
```

![image-20220117205407673](https://tva1.sinaimg.cn/large/008i3skNgy1gygyps8gzpj314a05875n.jpg)

![image-20220117205437632](https://tva1.sinaimg.cn/large/008i3skNgy1gygyqactv5j31460ootc0.jpg)

![image-20220117205624819](https://tva1.sinaimg.cn/large/008i3skNgy1gygys5aebvj314y0r40v0.jpg)



![image-20220117205702740](https://tva1.sinaimg.cn/large/008i3skNgy1gygyst755wj31940o4wh8.jpg)

![image-20220117205723351](https://tva1.sinaimg.cn/large/008i3skNgy1gygyt5rr8ej317w0l4abe.jpg)

![image-20220117205756314](https://tva1.sinaimg.cn/large/008i3skNgy1gygytqggfxj31iq0pg0ye.jpg)![image-20220117205739751]()

![image-20220117205808298](https://tva1.sinaimg.cn/large/008i3skNgy1gygytxtw5hj30o609aq3p.jpg)

![image-20220117212821729](https://tva1.sinaimg.cn/large/008i3skNgy1gygzpea5zdj31hq0o2gp5.jpg)

![image-20220117212852914](https://tva1.sinaimg.cn/large/008i3skNgy1gygzpxr5oij31fm0mcgn8.jpg)

![image-20220117212909793](https://tva1.sinaimg.cn/large/008i3skNgy1gygzq8007rj31520fqtai.jpg)

![image-20220117212944844](https://tva1.sinaimg.cn/large/008i3skNgy1gygzqtzn5yj318i0lc0w9.jpg)





# CloudWatch Logs

![image-20220117213316992](https://tva1.sinaimg.cn/large/008i3skNgy1gygzuiu4yrj31gc0ry77s.jpg)





### Logs + EC2 w/ CloudWatchAgent

![image-20220117221923094](https://tva1.sinaimg.cn/large/008i3skNgy1gyh16hj8blj31iq0psn07.jpg)

![image-20220117221934531](https://tva1.sinaimg.cn/large/008i3skNgy1gyh16oqwv8j31em0n4juv.jpg)

![image-20220117221942065](https://tva1.sinaimg.cn/large/008i3skNgy1gyh16t1ckoj31660lwgn2.jpg)

![image-20220117222044363](https://tva1.sinaimg.cn/large/008i3skNgy1gyh17w32ezj30si0a8aav.jpg)

![image-20220117222103400](https://tva1.sinaimg.cn/large/008i3skNgy1gyh187zt2mj310c0f476c.jpg)

![image-20220117222124886](https://tva1.sinaimg.cn/large/008i3skNgy1gyh18lxku9j314m0raq5t.jpg)

![image-20220117222148362](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1909gmcj31iq0pmack.jpg)

![image-20220117222213332](https://tva1.sinaimg.cn/large/008i3skNgy1gyh19fi6epj312i0j6abe.jpg)

![image-20220117222241782](https://tva1.sinaimg.cn/large/008i3skNgy1gyh19xml9jj31jq0j2q8f.jpg)

![image-20220117222254164](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1a5bxplj319i078gme.jpg)

#### LogGroup

![image-20220117222321243](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1amlw35j31i40k2ju8.jpg)

![image-20220117222352126](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1b62616j31ji0m4whl.jpg)



#### Log Insight

![image-20220117222428721](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1bsnbxcj31f60oqdip.jpg)







### Metric Filter

![image-20220117222713310](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1en1winj31fa0pm76l.jpg)

![image-20220117222729816](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1exgqzej31gm0pkn0i.jpg)

![image-20220117222810081](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1fmozjej31hu0p677r.jpg)

![image-20220117222819427](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1fsmi9hj31im0pu42c.jpg)