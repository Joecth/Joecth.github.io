---
layout: post
categories: DB
tag: []
date: 2022-01-24

---



# Clickhouse



https://github.com/ClickHouse/ClickHouse/releases?page=1



![image-20220125104621424](https://tva1.sinaimg.cn/large/008i3skNgy1gypq3x22zbj31390u0q82.jpg)





#### W/ CenOS

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-clickhouse-on-centos-7



#### on AWS w/ VPC

https://aws.amazon.com/quickstart/architecture/clickhouse-cluster/v





```shell
 237  sudo yum install yum-utils
  238  sudo rmp --import https://repo.clickhouse.com/CLICKHOUSE-KEY.CPG
  239  sudo rpm --import https://repo.clickhouse.com/CLICKHOUSE-KEY.CPG
  240  sudo rpm --import https://repo.clickhouse.tech/CLICKHOUSE-KEY.GPG
  241  sudo yum-config-manager --add-repo https://repo.clickhouse.tech/rpm/stable/x86_64
  242  sudo yum install cickhouse-server  clickhouse-client
  243  clickhouse-client
  244  clickhouse-client -h clickhouse-prod.internal.xxxxx.com --port 9000 --password yyyyy
```