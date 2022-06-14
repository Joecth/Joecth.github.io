---
layout: post
categories: SystemDesign
tag: []
date: 2020-01-23
---



# 票務系統、Distributed Transaction

- 相似: Ticketmaster、秒殺、Robinhood
- DB、不超賣、峰值concurrency



## Req

### Non-Fn



## Resource Est.



## Q1. Sub Services?

- Search 
- Seat
- Payment
- Waitlist



## Q2. 同場同位不重複賣?

事務鎖



## Q3: 怎避免付款後發現選的已賣出?

選定座位後，預留並作ttl



## Storage





