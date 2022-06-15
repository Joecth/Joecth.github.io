---
layout: post
categories: Spark
date: 2020-12-30
Author: Jo
---



## RDD

#### GetRDD

```java
        JavaRDD<Row> rdd = spark
                .read()
                .format("csv")
                .option("inferSchema", true)
                .option("header", true)
                .csv(path)
                .toJavaRDD();
```



#### Foreach

```java
rdd.foreach(row -> {
  	System.out.println("elem: " + row.get(1));
});
```



### kv Operation

d[key] += 1 <==      dgetOrDefault()

print(d['CA'])

#### Filter

```java
JavaRDD<Row> calPark = rdd.filter(row ->  row.getString(1).equals("California"));

long count = calPark.count()
```



#### Wide

```java
JavaPairRDD<String, Integer> parksPerState = rdd.mapToPair(row -> {
  	String state = row.getString(1);
  	return new Tuple2<>(state, 1);		// (Utah, 1), (Oregon, 1), (Utah, 1)..
});

/***↑map 　↓reduce***/
JavaPairRDD<String, Integer> stateParks = rdd.reduceByKey((x, y) -> x + y);

        stateParks.collect().forEach(tuple -> {
            System.out.println(String.format("%s: %s national parks", tuple._1, tuple._2));
        });

        stateParks.foreach((VoidFunction<Tuple2<String, Integer>>) tuple ->
                System.out.println(String.format("%s: %s national parks", tuple._1, tuple._2)));
```



## DF & Dataset

#### GetDF,  RDD VS DF

```java
        Dataset<Row> df = spark
                .read()
                .format("csv")
                .option("inferSchema", true)
                .option("header", true)
                .csv(path)
                .toDF();
df.printSchema();
/*
root
 |-- Number: integer (nullable = true)
 |-- Name: string (nullable = true)
 |-- Type1: string (nullable = true)
 |-- Type2: string (nullable = true)
 |-- Total: integer (nullable = true)
 |-- HP: integer (nullable = true)
 |-- Attack: integer (nullable = true)
 |-- Defense: integer (nullable = true)
 |-- SpecialAtk: integer (nullable = true)
 |-- SpecialDef: integer (nullable = true)
 |-- Speed: integer (nullable = true)
 |-- Generation: integer (nullable = true)
 |-- Legendary: boolean (nullable = true)
*/

/* VS rdd */
rdd.foreach(row -> {
  	System.out.println("name: " + row.get(1));
});

// In Contrast, for DF
        //由于dataframe的column信息已经有了，所以可以直接去访问column的值
        dataFrame.select("name").foreach(name_value -> {
            System.out.println("pokemon name: " + name_value.toString());
        });
        dataFrame.foreach(row -> {
            System.out.println("df name 2: " + row.getString(1));
        });

// Wut's more, 
        //dataframe可以通过spark sql访问，但rdd不可以
        dataFrame.createOrReplaceTempView("pokemon");
        Dataset<Row> poisonPokemon = spark.sql("select * from pokemon where Type1='Poison' limit 10");
        poisonPokemon.show();
/*
+------+---------+------+------+-----+---+------+-------+----------+----------+-----+----------+---------+
|Number|     Name| Type1| Type2|Total| HP|Attack|Defense|SpecialAtk|SpecialDef|Speed|Generation|Legendary|
+------+---------+------+------+-----+---+------+-------+----------+----------+-----+----------+---------+
|    23|    Ekans|Poison|    NA|  288| 35|    60|     44|        40|        54|   55|         1|    false|
|    24|    Arbok|Poison|    NA|  438| 60|    85|     69|        65|        79|   80|         1|    false|
|    29|Nidoran ♀|Poison|    NA|  275| 55|    47|     52|        40|        40|   41|         1|    false|
|    30| Nidorina|Poison|    NA|  365| 70|    62|     67|        55|        55|   56|         1|    false|
|    31|Nidoqueen|Poison|Ground|  505| 90|    92|     87|        75|        85|   76|         1|    false|
|    32|Nidoran ♂|Poison|    NA|  273| 46|    57|     40|        40|        40|   50|         1|    false|
|    33| Nidorino|Poison|    NA|  365| 61|    72|     57|        55|        55|   65|         1|    false|
|    34| Nidoking|Poison|Ground|  505| 81|   102|     77|        85|        75|   85|         1|    false|
|    41|    Zubat|Poison|Flying|  245| 40|    45|     35|        30|        40|   55|         1|    false|
|    42|   Golbat|Poison|Flying|  455| 75|    80|     70|        65|        75|   90|         1|    false|
+------+---------+------+------+-----+---+------+-------+----------+----------+-----+----------+---------+
*/
```

```java
        Dataset<Row> df = spark
                .read()
                .format("csv")
                .option("inferSchema", true)
                .option("header", true)
                .csv(path)
                .toDF()
                .select("userId", "movieId", "rating"); // Say, Discard "Timestamp"
```





#### GetDataset, DF VS Dataset

###### foreach

###### select

```java
        Encoder<Pokemon> pokemon_encoder = Encoders.bean(Pokemon.class);
        Dataset<Pokemon> dataset = spark
                .read()
                .format("csv")
                .option("inferSchema", true)
                .option("header", true)
                .csv(path)
                .as(pokemon_encoder);


        //dataframe 每一行是row，而dataset每一行是一个指定的class
        df.foreach(row -> {
            String name_tmp = row.getString(1);
            System.out.println("name from DF getString: " + name_tmp);
        });
        df.select("name").foreach(name_val -> {
            System.out.println("name from DF select: " + name_val.toString());
        });
        /*** ↑ ↑ ↑
        name from DF getString: Zenaora
        name from DF select: [Bulbasaur]
        ***/
        
        dataset.foreach(row -> {
            String name = row.getName();
//            System.out.println("name: " + name);
        });

        //dataframe的解析错误只能在runtime时候报错，而dataset解析错误compile时候就会报错
        df.foreach(row -> {
//            System.out.println("getAs: " + row.getAs("type").toString()); // 壓根沒有這個字段，只有type1 & type2
        });
        dataset.foreach((row -> {
            row.getType1(); // HOW to jump to there?
//            row.getType2();
        }));
```





## rdd, df, dataset 共同點

- lazy init
- 都會根據ram自動cache運算
- 都有partition



## Operations

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h38abmpmi6j20qh094abb.jpg" alt="image-20210128230633294" style="zoom:50%;" />



### DF APIs

```java
        //找到water 和 fire pokemon
        Dataset<Row> waterPokemon = pokemonType.filter(
                pokemonType.col("Type1").equalTo("Water")
                        .or(pokemonType.col("Type2").equalTo("Water"))
        );
        pokemonType.createOrReplaceTempView("pokemon_type");
        Dataset<Row> firePokemon = spark
                .sql("select * from pokemon_type where Type1='Fire' or Type2='Fire'");

        //找到water and fire Pokemon
        Dataset<Row> versertilePokemon = waterPokemon.select(waterPokemon.col("Name"))
                .intersect(firePokemon.select(firePokemon.col("Name")));
        System.out.println("VERSERTILE: ");
        versertilePokemon.show();
				/***
        VERSERTILE: 
        +---------+
        |     Name|
        +---------+
        |Volcanion|
        +---------+
        ***/


```



#### Join -- Left

```java
        //attack max in water pokemon
        Dataset<Row> waterPokemonWithForce = waterPokemon.join(
                pokemonForce,
                waterPokemon.col("Number").equalTo(pokemonForce.col("Number")),
                "left"
        );
        waterPokemonWithForce.sort(waterPokemonWithForce.col("Attack").desc());
        Row[] waterPokemonWithMaxForce = (Row[])waterPokemonWithForce.take(1);


        //type1 有哪些类型的pokemon type， -> count
        Dataset<Row> pokemonCounts = spark.sql("select Type1, count(*) as total_count from pokemon_type group by Type1");
        pokemonCounts.show();
        pokemonCounts.count();
				/***
        +--------+-----------+
        |   Type1|total_count|
        +--------+-----------+
        |   Water|        122|
        |  Poison|         34|
        |   Steel|         29|
        |    Rock|         51|
        |     Ice|         24|
        |   Ghost|         36|
        |   Fairy|         18|
        | Psychic|         67|
        |  Dragon|         35|
        |  Flying|          4|
        |     Bug|         78|
        |Electric|         49|
        |    Fire|         58|
        |  Ground|         34|
        |    Dark|         32|
        |Fighting|         31|
        |   Grass|         82|
        |  Normal|        110|
        +--------+-----------+
        ***/
```



#### Join -- self-join --> filter --> map --> groupie --> agg

```java
        Dataset<Row> joinedRatings = userRating.as("a").join(
                userRating.as("b"),
                "userId")
                .selectExpr("a.userId as userId", "a.movieId as movie1",
                        "a.rating as rating1", "b.movieId as movie2", "b.rating as rating2");
				
				/* 在 self-join 後，過濾掉重覆的組合 */
        Dataset<Row> uniqueJoinedRatings = joinedRatings.filter(joinedRatings.col("movie1")
                .$less(joinedRatings.col("movie2")));

				/* 做成 <movie1:movie2> : <rating1，rating2>的pair */
        Dataset<MoviePair> pairs = uniqueRatings
                .map((MapFunction<Row, MoviePair>) row -> {
                    return new MoviePair(
                                    row.getAs("movie1"),
                                    row.getAs("movie2"),
                                    row.getAs("rating1"),
                                    row.getAs("rating2"));
                }, Encoders.bean(MoviePair.class));

				/* group by (movie1,movie2),每一行都是所有的（rating1， rating2）的组合 */
        Dataset<Row> movieRatingPairs = uniqueRatings
                .groupBy("movie_pair")
                .agg(functions.collect_list("rating_pair").as("rating_pairs"));
				
				/* dump to file */
        movieSimilarities.coalesce(10).write().csv("movie_relationship");
```



#### Map : for similarity

```java
    public Dataset<MovieSimilarity> computeSimilarity(Dataset<Row> movieRatingPairs) {
        Dataset<MovieSimilarity> movieSimilarities = movieRatingPairs
                .map((MapFunction<Row, MovieSimilarity>) row-> {
                    WrappedArray<Integer> movie_pair = row.getAs("movie_pair");
                    WrappedArray<WrappedArray<Double>> rating_pairs = row.getAs("rating_pairs");
                    int num_pairs = rating_pairs.size();
                    double sum_xx = 0;
                    double sum_yy =0;
                    double sum_xy=0;
                    Iterator<WrappedArray<Double>> iterator = rating_pairs.iterator();
                    while (iterator.hasNext()) {
                        WrappedArray rating = iterator.next();
                        double rating1 = (double) rating.head();
                        double rating2 = (double) rating.last();
                        sum_xx += rating1*rating1;
                        sum_yy += rating2*rating2;
                        sum_xy += rating1*rating2;
                    }

                    double denominator = Math.sqrt(sum_xx) * Math.sqrt(sum_yy);
                    double score = 0;
                    if (denominator != 0) {
                        score = sum_xy/denominator;
                    }

            return new MovieSimilarity(movie_pair.head(), movie_pair.last(), score, num_pairs);
        }, Encoders.bean(MovieSimilarity.class));

        return movieSimilarities;
```

#### Using Similarity

```java
        RecommendMovie main = new RecommendMovie();
        Dataset<Row> movieSimilarity = main.getMovieSimilarity(spark);
//        int targetMovie = Integer.parseInt(args[0]);
        int targetMovie = 6;
        double scoreThreshold = Double.parseDouble(args[1]);
        int pairThreshold = Integer.parseInt(args[2]);
        System.out.println("my args: " + targetMovie);
        main.findMostSimilarMovies(spark, movieSimilarity, targetMovie, scoreThreshold, pairThreshold);
/***
my args: 6
+--------------------+
|      original_title|
+--------------------+
|Sissi - Die junge...|
|         The Patriot|
|                null|
|         Poltergeist|
|Till det som är v...|
+--------------------+
***/
```







### Action

- Collect(), take(), count(), first(), foreach()



### Shuffle

- groupByKey





Work --> survey, algorithm, 

​                Table



big_data











大腿们能否请教个Query~

表A
| notification_id    | type                                                 |
| uuid                      | AorBorC 反正是categorial的       |

表B
|id.            |       notification_id      | visitor_token         |
|uuid         |         就是外键指向A    |   uuid                     |

我只是想知道
表A里不同的 type各自会造成多少的 subscribe_rate  (字段名字已换掉，但关系大致是这样)

表B里的就是已经订阅了的意思，

用SELECT id AS _id, type FROM tbl_A GROUP BY tbl_A.type
是会有各个type 的东西，如
A  300
B. 300
C. 400

但为什么我
SELECT tbl_a.type, count(*)
FROM  tbl_a
INNER JOIN tbl_b
ON tbl_a.notification_id = tbl_b.notification_id
GROUP BY tbl_a.type
ORDER BY count(*) DESC;
出来的结果，A, B, C各个数字都还比较大。。　我觉得应该要比较小呀。。这样我才可以把出来的除以对A表单做的GroupBy的Query. . 
我试着拉个1000 row到本地做实验，但是
最后出来的都是空的。。。　＞＜　真的不行了，所以想请教下。。





abcdefghijklmnopqrstuvwxyz