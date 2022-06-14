---
layout: post
categories: BigData_python
tag: []
date: 2020-10-23
---



## PySpark + Clickhouse



```python
# import pandas as pd
# import numpy as np
# import json

from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql import types as T


def query_clickhouse_by_spark(spark):
    # customSchema = T.StructType([
    #     T.StructField("customerId", T.IntegerType(), True),
    #     T.StructField("titleId", T.IntegerType(), True),
    #     T.StructField("freq", T.FloatType(), True),
    #     # T.StructField("timestamp", T.LongType(), True),
    # ])
    query = """ 
            WITH
                toDate('2022-02-21') AS date,
                INTERVAL 1 WEEK AS interval_week
            SELECT
                * except(imported, campaign_id_valid) from analytics.^_____^y_002 -- (select * except(imported, campaign_id_valid) from analytics.orders_events_002 )-- analytics.orders_events_002 
            WHERE
                toDate(updated_at) >= date 
                AND toDate(updated_at) <= toStartOfDay(date + INTERVAL 0 DAY)
            """
    raw_df = spark.read \
        .format("jdbc") \
        .option("url", "jdbc:clickhouse://clickhouse-prod.internal.^___________^Y.com:8123") \
        .option("query", "SELECT version()") \
        .option("password", "Y^____________^Y") \
        .load()
        # .option("driver", "ru.yandex.clickhouse.ClickHouseDriver") \
    raw_df.printSchema()
    raw_df.show(1, True)
    # df = spark.read.csv('./resources/freqs.csv', header=True, schema=customSchema)
    return raw_df

def load_dataset_by_spark(spark):
    customSchema = T.StructType([
        T.StructField("customerId", T.IntegerType(), True),
        T.StructField("titleId", T.IntegerType(), True),
        T.StructField("freq", T.FloatType(), True),
        # T.StructField("timestamp", T.LongType(), True),
    ])

    df = spark.read.csv('./resources/freqs.csv', header=True, schema=customSchema)
    return df

def print_hi(name):
    # Use a breakpoint in the code line below to debug your script.
    print(f'Hi, {name}')  # Press ⌘F8 to toggle the breakpoint.


# Press the green button in the gutter to run the script.
if __name__ == '__main__':
    print_hi('PyCharm')
    # findspark.init()
    # myjar = "/home/ec2-user/code/train-recsys/clickhouse-jdbc-0.3.1.jar"
    # myjar = './clickhouse-native-jdbc-shaded.jar'
    myjar = "/home/ec2-user/clickhouse-jdbc-0.3.2-patch7-all.jar"
    spark = SparkSession \
        .builder \
        .master('local') \
        .appName("PySpark ALS") \
        .config("spark.jars", myjar) \
        .config("spark.executor.extraClassPath", myjar) \
        .config("spark.executor.extraLibrary", myjar) \
        .config("spark.driver.extraClassPath", myjar) \
        .getOrCreate()

    # sc = spark.sparkContext
    # df = load_dataset_by_spark(spark)
    df = query_clickhouse_by_spark(spark)
    df.show(20)

# See PyCharm help at https://www.jetbrains.com/help/pycharm/
# JAVA_HOME installation: https://bhargavamin.com/how-to-do/setting-up-java-environment-variable-on-ec2/
#
```
