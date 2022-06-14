---
layout: post
categories: airflow
tag: [] 
date: 2022-03-12
---



```shell
$ <turn on db from docker>
$ airflow webserver -p 8080
$ airflow scheduler -D

```



```bash
DAGs
Security
Browse
Admin
Docs
18:13 UTC 
JH
 
 DAG: Download_Stock_Price Download stock price and save to local cscv files

nature Tree
account_tree Graph
event Calendar
hourglass_bottom Task Duration
repeat Task Tries
flight_land Landing Times
vertical_distribute Gantt
details Details
code Code

Task Instance: merge_stock_price at 2022-03-14, 18:13:25
details Task Instance Details
code Rendered Template
reorder Log
sync_alt XCom

Log by attempts
1
 
*** Reading local file: /Users/joe/airflow/logs/Download_Stock_Price/merge_stock_price/2022-03-14T18:13:25.005546+00:00/1.log
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1037} INFO - Dependencies all met for <TaskInstance: Download_Stock_Price.merge_stock_price manual__2022-03-14T18:13:25.005546+00:00 [queued]>
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1037} INFO - Dependencies all met for <TaskInstance: Download_Stock_Price.merge_stock_price manual__2022-03-14T18:13:25.005546+00:00 [queued]>
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1243} INFO - 
--------------------------------------------------------------------------------
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1244} INFO - Starting attempt 1 of 2
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1245} INFO - 
--------------------------------------------------------------------------------
[2022-03-15, 02:13:36 UTC] {taskinstance.py:1264} INFO - Executing <Task(MySqlOperator): merge_stock_price> on 2022-03-14 18:13:25.005546+00:00
[2022-03-15, 02:13:36 UTC] {standard_task_runner.py:52} INFO - Started process 21927 to run task
[2022-03-15, 02:13:36 UTC] {standard_task_runner.py:76} INFO - Running: ['***', 'tasks', 'run', 'Download_Stock_Price', 'merge_stock_price', 'manual__2022-03-14T18:13:25.005546+00:00', '--job-id', '28', '--raw', '--subdir', 'DAGS_FOLDER/download_stock_price.py', '--cfg-path', '/var/folders/7v/3y625wsd3475wpjfymhpt5fh0000gn/T/tmpsvh2385r', '--error-file', '/var/folders/7v/3y625wsd3475wpjfymhpt5fh0000gn/T/tmpel4e5vxm']
[2022-03-15, 02:13:36 UTC] {standard_task_runner.py:77} INFO - Job 28: Subtask merge_stock_price
[2022-03-15, 02:13:36 UTC] {logging_mixin.py:109} INFO - Running <TaskInstance: Download_Stock_Price.merge_stock_price manual__2022-03-14T18:13:25.005546+00:00 [running]> on host 1.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.0.ip6.arpa
[2022-03-15, 02:13:37 UTC] {taskinstance.py:1431} INFO - Exporting the following env vars:
AIRFLOW_CTX_DAG_EMAIL=joeonion0303@gmail.com
AIRFLOW_CTX_DAG_OWNER=JoH
AIRFLOW_CTX_DAG_ID=Download_Stock_Price
AIRFLOW_CTX_TASK_ID=merge_stock_price
AIRFLOW_CTX_EXECUTION_DATE=2022-03-14T18:13:25.005546+00:00
AIRFLOW_CTX_DAG_RUN_ID=manual__2022-03-14T18:13:25.005546+00:00
[2022-03-15, 02:13:37 UTC] {mysql.py:82} INFO - Executing: -- update the existing rows
update stock_prices p, stock_prices_stage s
set p.open_price = s.open_price,
	p.high_price = s.high_price,
	p.low_price = s.low_price,
	p.close_price = s.close_price,
	updated_at = now()
where p.ticker = s.ticker
	and p.as_of_date = s.as_of_date;
	
-- inserting new rows
insert into stock_prices
(ticker, as_of_date, open_price, high_price, low_price, close_price)
select ticker, as_of_date, open_price, high_price, low_price, close_price
from stock_prices_stage s
	where not exists
	(select 1 from stock_prices p
		where p.ticker = s.ticker
		and p.as_of_date = s.as_of_date);

-- truncate the stage table;
truncate table stock_prices_stage;
[2022-03-15, 02:13:37 UTC] {base.py:79} INFO - Using connection to: id: demodb. Host: 127.0.0.1, Port: 3306, Schema: demodb, Login: root, Password: ***, extra: {}
[2022-03-15, 02:13:37 UTC] {dbapi.py:225} INFO - Running statement: -- update the existing rows
update stock_prices p, stock_prices_stage s
set p.open_price = s.open_price,
	p.high_price = s.high_price,
	p.low_price = s.low_price,
	p.close_price = s.close_price,
	updated_at = now()
where p.ticker = s.ticker
	and p.as_of_date = s.as_of_date;
	
-- inserting new rows
insert into stock_prices
(ticker, as_of_date, open_price, high_price, low_price, close_price)
select ticker, as_of_date, open_price, high_price, low_price, close_price
from stock_prices_stage s
	where not exists
	(select 1 from stock_prices p
		where p.ticker = s.ticker
		and p.as_of_date = s.as_of_date);

-- truncate the stage table;
truncate table stock_prices_stage;, parameters: None
[2022-03-15, 02:13:37 UTC] {dbapi.py:233} INFO - Rows affected: 80
[2022-03-15, 02:13:37 UTC] {taskinstance.py:1282} INFO - Marking task as SUCCESS. dag_id=Download_Stock_Price, task_id=merge_stock_price, execution_date=20220314T181325, start_date=20220314T181336, end_date=20220314T181337
[2022-03-15, 02:13:37 UTC] {local_task_job.py:154} INFO - Task exited with return code 0
[2022-03-15, 02:13:37 UTC] {local_task_job.py:264} INFO - 0 downstream tasks scheduled from follow-on schedule check


Version: v2.2.4
Git Version: .release:2.2.4+ee9049c0566b2539a247687de05f9cffa008f871
```
