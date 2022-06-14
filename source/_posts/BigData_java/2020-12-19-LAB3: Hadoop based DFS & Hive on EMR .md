---
layout: post
categories: BigData_java
tag: []
date: 2020-12-19
---



# HDFS(Hadoop based DFS) & Hive on EMR

## Access Remote EMR clusters

```zsh
# joe @ MacBook-Pro-4 in ~/Desktop/mi_BigData/Codes_BD [21:38:47]
$ ssh -i onionAWS.pem hadoop@ec2-18-191-169-131.us-east-2.compute.amazonaws.com
The authenticity of host 'ec2-18-191-169-131.us-east-2.compute.amazonaws.com (18.191.169.131)' can't be established.
ECDSA key fingerprint is SHA256:UdI9VoiJMwSRoJ/SHjNpgger1vgUIxa4MgWtKU9XQaE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-18-191-169-131.us-east-2.compute.amazonaws.com,18.191.169.131' (ECDSA) to the list of known hosts.
Last login: Sat Dec 19 14:59:38 2020

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/

EEEEEEEEEEEEEEEEEEEE MMMMMMMM           MMMMMMMM RRRRRRRRRRRRRRR
E::::::::::::::::::E M:::::::M         M:::::::M R::::::::::::::R
EE:::::EEEEEEEEE:::E M::::::::M       M::::::::M R:::::RRRRRR:::::R
  E::::E       EEEEE M:::::::::M     M:::::::::M RR::::R      R::::R
  E::::E             M::::::M:::M   M:::M::::::M   R:::R      R::::R
  E:::::EEEEEEEEEE   M:::::M M:::M M:::M M:::::M   R:::RRRRRR:::::R
  E::::::::::::::E   M:::::M  M:::M:::M  M:::::M   R:::::::::::RR
  E:::::EEEEEEEEEE   M:::::M   M:::::M   M:::::M   R:::RRRRRR::::R
  E::::E             M:::::M    M:::M    M:::::M   R:::R      R::::R
  E::::E       EEEEE M:::::M     MMM     M:::::M   R:::R      R::::R
EE:::::EEEEEEEE::::E M:::::M             M:::::M   R:::R      R::::R
E::::::::::::::::::E M:::::M             M:::::M RR::::R      R::::R
EEEEEEEEEEEEEEEEEEEE MMMMMMM             MMMMMMM RRRRRRR      RRRRRR

[hadoop@ip-172-31-47-22 ~]$ ls
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs --help
--help: Unknown command
Usage: hadoop fs [generic options]
	[-appendToFile <localsrc> ... <dst>]
	[-cat [-ignoreCrc] <src> ...]
	[-checksum <src> ...]
	[-chgrp [-R] GROUP PATH...]
	[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
	[-chown [-R] [OWNER][:[GROUP]] PATH...]
	[-copyFromLocal [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-copyToLocal [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-count [-q] [-h] [-v] [-t [<storage type>]] [-u] [-x] <path> ...]
	[-cp [-f] [-p | -p[topax]] [-d] <src> ... <dst>]
	[-createSnapshot <snapshotDir> [<snapshotName>]]
	[-deleteSnapshot <snapshotDir> <snapshotName>]
	[-df [-h] [<path> ...]]
	[-du [-s] [-h] [-x] <path> ...]
	[-expunge]
	[-find <path> ... <expression> ...]
	[-get [-f] [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
	[-getfacl [-R] <path>]
	[-getfattr [-R] {-n name | -d} [-e en] <path>]
	[-getmerge [-nl] [-skip-empty-file] <src> <localdst>]
	[-help [cmd ...]]
	[-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [<path> ...]]
	[-mkdir [-p] <path> ...]
	[-moveFromLocal <localsrc> ... <dst>]
	[-moveToLocal <src> <localdst>]
	[-mv <src> ... <dst>]
	[-put [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-renameSnapshot <snapshotDir> <oldName> <newName>]
	[-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
	[-rmdir [--ignore-fail-on-non-empty] <dir> ...]
	[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
	[-setfattr {-n name [-v value] | -x name} <path>]
	[-setrep [-R] [-w] <rep> <path> ...]
	[-stat [format] <path> ...]
	[-tail [-f] <file>]
	[-test -[defsz] <path>]
	[-text [-ignoreCrc] <src> ...]
	[-touchz <path> ...]
	[-truncate [-w] <length> <path> ...]
	[-usage [cmd ...]]

Generic options supported are:
-conf <configuration file>        specify an application configuration file
-D <property=value>               define a value for a given property
-fs <file:///|hdfs://namenode:port> specify default filesystem URL to use, overrides 'fs.defaultFS' property from configurations.
-jt <local|resourcemanager:port>  specify a ResourceManager
-files <file1,...>                specify a comma-separated list of files to be copied to the map reduce cluster
-libjars <jar1,...>               specify a comma-separated list of jar files to be included in the classpath
-archives <archive1,...>          specify a comma-separated list of archives to be unarchived on the compute machines

The general command line syntax is:
command [genericOptions] [commandOptions]

[hadoop@ip-172-31-47-22 ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j2.properties Async: true
hive> show databases;
OK
default
Time taken: 0.722 seconds, Fetched: 1 row(s)
hive>
```



#### HDFS

- 比如5台機器在hadoop 集群裡，hdfs 或我們的分布式文件存儲系統就存在於這5台機器上，不需要去關心裡面，就是黑箱，所有合、拆、刪在裡面；我只要在hdfs平台去交互，我只要給他data，他要怎麼做是他內部自己操作

- 本質就是文件系統
- 把data存在N台機器上，但client 處理了內部的所有黑箱，所以我只有一個對手! 就如果我在本地



## HDFS commands

##### S3 -- 基於AWS的 dfs，HDFS -- 基於Hadoop的 dfs，本質是非常相似的，只不過S3是UI介面

```zsh
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls
Found 1 items
drwxr-xr-x   - hadoop hadoop          0 2020-12-19 15:10 .hiveJars
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -mkdir bigdata
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls
Found 2 items
drwxr-xr-x   - hadoop hadoop          0 2020-12-19 15:10 .hiveJars
drwxr-xr-x   - hadoop hadoop          0 2020-12-19 15:23 bigdata
[hadoop@ip-172-31-47-22 ~]$ vim file.csv
[hadoop@ip-172-31-47-22 ~]$ ls
file.csv
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -put file.csv bigdata/
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls
Found 2 items
drwxr-xr-x   - hadoop hadoop          0 2020-12-19 15:10 .hiveJars
drwxr-xr-x   - hadoop hadoop          0 2020-12-19 15:28 bigdata
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls data/
ls: `data/': No such file or directory
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -ls bigdata/
Found 1 items
-rw-r--r--   1 hadoop hadoop         19 2020-12-19 15:28 bigdata/file.csv
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -cat bigdata/file.csv
i. love. big. data
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -cat bigdata/file1.csv
cat: `bigdata/file1.csv': No such file or directory
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -rm bigdata/file.csv
Deleted bigdata/file.csv
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -rm bigdata
rm: `bigdata': Is a directory
[hadoop@ip-172-31-47-22 ~]$ hdfs dfs -rmr bigdata
rmr: DEPRECATED: Please use '-rm -r' instead.
Deleted bigdata
```



## Create Hive DB & Table

#### Hive

- 只是把data映射出來讓大家很好地作query
- 仍是存儲在HDFS上，以文件的型式存儲的
- 所以該指定的除了 Table Name、Schema, 還該指定最底層的data source的format, 可以是 ORC, Parquet, Avro，由我指定，然後data會被自動存成這個型式，hive會幫我們做這些操作! 不需要操心
  - 我需要操心的是寫進去hdfs時，想要以怎樣的一個文件型式寫入!

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a8q6u4gj20o607omxk.jpg" alt="image-20201219234856829" style="zoom: 33%;" />

- 所有的指令都和SQL一樣，底層就是antler的解析器
- hive還是要存去底層給hdfs上去的，然後要指定format，這邊指定成了 ORC



```zsh
[hadoop@ip-172-31-47-22 ~]$ hive

Logging initialized using configuration in file:/etc/hive/conf.dist/hive-log4j2.properties Async: true
hive> show database;
NoViableAltException(78@[846:1: ddlStatement : ( createDatabaseStatement | switchDatabaseStatement | dropDatabaseStatement | createTableStatement | dropTableStatement | truncateTableStatement | alterStatement | descStatement | showStatement | metastoreCheck | createViewStatement | createMaterializedViewStatement | dropViewStatement | dropMaterializedViewStatement | createFunctionStatement | createMacroStatement | createIndexStatement | dropIndexStatement | dropFunctionStatement | reloadFunctionStatement | dropMacroStatement | analyzeStatement | lockStatement | unlockStatement | lockDatabase | unlockDatabase | createRoleStatement | dropRoleStatement | ( grantPrivileges )=> grantPrivileges | ( revokePrivileges )=> revokePrivileges | showGrants | showRoleGrants | showRolePrincipals | showRoles | grantRole | revokeRole | setRole | showCurrentRole | abortTransactionStatement );])
	at org.antlr.runtime.DFA.noViableAlt(DFA.java:158)
	at org.antlr.runtime.DFA.predict(DFA.java:116)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:3757)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:2382)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1333)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:208)
	at org.apache.hadoop.hive.ql.parse.ParseUtils.parse(ParseUtils.java:77)
	at org.apache.hadoop.hive.ql.parse.ParseUtils.parse(ParseUtils.java:70)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:468)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1317)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1457)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1237)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1227)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:233)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:184)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:403)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:821)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:759)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:686)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:244)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:158)
FAILED: ParseException line 1:5 cannot recognize input near 'show' 'database' '<EOF>' in ddl statement
hive> show databases;
OK
default
Time taken: 0.263 seconds, Fetched: 1 row(s)
hive> create bigdata;
NoViableAltException(24@[846:1: ddlStatement : ( createDatabaseStatement | switchDatabaseStatement | dropDatabaseStatement | createTableStatement | dropTableStatement | truncateTableStatement | alterStatement | descStatement | showStatement | metastoreCheck | createViewStatement | createMaterializedViewStatement | dropViewStatement | dropMaterializedViewStatement | createFunctionStatement | createMacroStatement | createIndexStatement | dropIndexStatement | dropFunctionStatement | reloadFunctionStatement | dropMacroStatement | analyzeStatement | lockStatement | unlockStatement | lockDatabase | unlockDatabase | createRoleStatement | dropRoleStatement | ( grantPrivileges )=> grantPrivileges | ( revokePrivileges )=> revokePrivileges | showGrants | showRoleGrants | showRolePrincipals | showRoles | grantRole | revokeRole | setRole | showCurrentRole | abortTransactionStatement );])
	at org.antlr.runtime.DFA.noViableAlt(DFA.java:158)
	at org.antlr.runtime.DFA.predict(DFA.java:144)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:3757)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:2382)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1333)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:208)
	at org.apache.hadoop.hive.ql.parse.ParseUtils.parse(ParseUtils.java:77)
	at org.apache.hadoop.hive.ql.parse.ParseUtils.parse(ParseUtils.java:70)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:468)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1317)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1457)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1237)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1227)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:233)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:184)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:403)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:821)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:759)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:686)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:244)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:158)
FAILED: ParseException line 1:7 cannot recognize input near 'create' 'bigdata' '<EOF>' in ddl statement
hive> create database bigdata
    > ;
OK
Time taken: 0.301 seconds
hive> show databases;
OK
bigdata
default
Time taken: 0.028 seconds, Fetched: 2 row(s)
hive> use bigdata;
OK
Time taken: 0.105 seconds
hive> CREATE TABLE IF NOT EXISTS bigdata.movie_similarity
    > (
    > movie1          INT,
    > movie2 INT,
    > num_pairs INT,
    > similarity DOUBLE
    > )
    > STORED AS ORC;
OK
Time taken: 0.56 seconds
hive> describe movie_similarity;
OK
movie1              	int
movie2              	int
num_pairs           	int
similarity          	double
Time taken: 0.093 seconds, Fetched: 4 row(s)
hive> describe formatted movie_similarity;
OK
# col_name            	data_type           	comment

movie1              	int
movie2              	int
num_pairs           	int
similarity          	double

# Detailed Table Information
Database:           	bigdata
Owner:              	hadoop
CreateTime:         	Sat Dec 19 15:53:27 UTC 2020
LastAccessTime:     	UNKNOWN
Retention:          	0
Location:           	hdfs://ip-172-31-47-22.us-east-2.compute.internal:8020/user/hive/warehouse/bigdata.db/movie_similarity
Table Type:         	MANAGED_TABLE
Table Parameters:
	COLUMN_STATS_ACCURATE	{\"BASIC_STATS\":\"true\"}
	numFiles            	0
	numRows             	0
	rawDataSize         	0
	totalSize           	0
	transient_lastDdlTime	1608393207

# Storage Information
SerDe Library:      	org.apache.hadoop.hive.ql.io.orc.OrcSerde
InputFormat:        	org.apache.hadoop.hive.ql.io.orc.OrcInputFormat
OutputFormat:       	org.apache.hadoop.hive.ql.io.orc.OrcOutputFormat
Compressed:         	No
Num Buckets:        	-1
Bucket Columns:     	[]
Sort Columns:       	[]
Storage Desc Params:
	serialization.format	1
Time taken: 0.125 seconds, Fetched: 33 row(s)
hive>
```



## Write data into Hive Table, fr Spark

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a8slwhcj21e406mq3w.jpg" alt="image-20201220001941568" style="zoom:50%;" />

- 得確認我所generate 出來的DataFrame的schema跟我所需要寫進去的Table裡的schema是完全一致的，不然就會報錯

- run Step 的差别在：現在在EMR上run它的JAR的話，data不是存到S3，而是會存到hive連下去的DFS



##### 當需要的是「非static data」，而是「動態被傳入的 data」，Arguments 上就要被進行一個傳入

- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gltmtsovq8j317i0owq9d.jpg" alt="image-20201220003046498" style="zoom:50%;" />



- <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a87yanmj20sm09u3zs.jpg" alt="image-20201220003235826" style="zoom:50%;" />





## Interact with data in Hive Table

Complete後
<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gltmxlpvqtj319o0l87ok.jpg" alt="image-20201220003427565" style="zoom:50%;" />

- Limit 1 後才有Trigger，就是aws上的hive還是怪怪的

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a8abfduj21620nen25.jpg" alt="image-20201220003534838" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gltmz8c6y9j30n00fstnt.jpg" alt="image-20201220003601410" style="zoom:50%;" />



<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a8giz5qj216a0fcwka.jpg" alt="image-20201220003929425" style="zoom:50%;" />



- snappy就是個常見的壓縮方式



想要找到某一個target 的movie所對應的相關聯的電影中的top 3；我們的方法呢？

那時還沒有hive，也沒有query engine，就只能建新的java的file叫 recommend_movie.java 去query output 的 ORC的data，然後把這些data load到DataFrame去，然後DataFrame再進行一系列的 JOIN的操作，然後再把結果給print出來



現在有了Hive，也許就不需要再Spark這種heavy的操作

- Spark -- 需要 
  - 寫一個spark application
  - 要把spark applicateion 上傳去AWS
  - 要去add step、去執行它
    - 再等AWS裡面的Spark job還要去 schedule, 之後還要run，run後生成的結果才能去std.out查到!

- 現在有hive，又可以view了，不如就是用hive直接交互數據



#### Case: movie target id=1 相關的前 Top3 movie

- 在 Hive裡 SQL 語句 -- query的界面

![image-20201220005825999](https://tva1.sinaimg.cn/large/0081Kckwgy1gltnmjustxj315s0gyasu.jpg)

- 這個時候未啟動 reducer













- ↓這時候在reducer裡進行了一系列的排序(?!)，是因為有了排序，所以比較慢了點

![image-20201220005905548](https://tva1.sinaimg.cn/large/0081Kckwgy1gltnn83zk0j316e0h44h3.jpg)

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38a8kox1ij20om0lctcp.jpg" alt="image-20201220005953451" style="zoom:50%;" />

- 偷看一眼用
- 這樣工作效率就高很多，就是因為有 Hive! 可以取代了 Spark code in java
- 可以簡化了基礎的流程