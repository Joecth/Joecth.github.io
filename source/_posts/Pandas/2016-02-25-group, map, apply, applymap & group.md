---
layout: post
categories: Pandas
tag: []
date: 2016-02-25 
---



# DF & Series in General

## Map, Apply & Applymap

> Case

```python
>>> df
   A  B  C
0  3  9 -4
1  9  3 -2
2  7  6  2
3  3  5  5
```



p.s. applymap 就是 map在 df 上的功能，map只能作用在 Series 上, 兩者都是作用在 elem上

In contrast, apply作用在 vector上

1. Def.

- `map`僅在Series(係列)上定義
- `applymap`僅在DataFrame上定義
- `apply`在Series和DataFrame兩者上均有定義

2. param

- `map`接受`dict`，`Series`或可調用的函數對象
- `applymap`和`apply`僅接受可調用函數對象

3. 行為

- `map`是對Series按元素操作的
- `applymap`是對DataFrames按元素操作的
- `apply`也可以逐元素運行，但適用於更複雜的操作和聚合。行為和返回值取決於函數。

4. 用例

- `map`用於將值從一個域映射到另一個域，因此針對性能進行了優化(例如`df['A'].map({1:'a', 2:'b', 3:'c'})`)
- `applymap`適用於跨多個行/列的元素轉換(例如`df[['A', 'B', 'C']].applymap(str.strip)`

### Map -- 平常肯定不該用 for loop 在搞

works on a `Series` only, doesn't work on a `df`

```python
>>> df.B.map(lambda elem: "%.3f" % elem)
0    9.000
1    3.000
2    6.000
3    5.000

>>> df.B.map({3:'A', 5:'B'})	#另一個域，因此針對性能進行了優化
0    NaN
1      A
2    NaN
3      B
```

#### Type Clarification

```python
>>> df.iloc[1].map(lambda elem: "%.3f" % elem)
A     9.000
B     3.000
C    -2.000
Name: 1, dtype: object
>>> type(df.iloc[1])
<class 'pandas.core.series.Series'>
>>> type(df.iloc[[1]])
<class 'pandas.core.frame.DataFrame'>
```





### Applymap

- works on each elem

```python
>>> df.applymap(lambda elem: 1 if elem > 0 else -1)
   A  B  C
0  1  1 -1
1  1  1 -1
2  1  1  1
3  1  1  1

>>> df.applymap(lambda elem: "%.2f" % elem)
      A     B      C
0  3.00  9.00  -4.00
1  9.00  3.00  -2.00
2  7.00  6.00   2.00
3  3.00  5.00   5.00
```





### Apply

- works on an entire row or col
- takes entire row or col as input of lambda function at a time
- 應用無法向量化的任何功能

```python
>>> df.apply(lambda row: row[1], axis=1)
0    9
1    3
2    6
3    5
dtype: int64
>>> df.apply(lambda col: col[1], axis=0)
A    9
B    3
C   -2
dtype: int64
>>> df.apply(np.log)
          A         B         C
0  1.098612  2.197225       NaN
1  2.197225  1.098612       NaN
2  1.945910  1.791759  0.693147
3  1.098612  1.609438  1.609438
>>> df.apply(np.sum)
A    22
B    23
C     1
>>> df.apply(np.sqrt)
          A         B         C
0  1.732051  3.000000       NaN
1  3.000000  1.732051       NaN
2  2.645751  2.449490  1.414214
3  1.732051  2.236068  2.236068

>>> df.apply(lambda col: col.max() - col.min())
A    6
B    6
C    9
>>> df.apply(lambda col, offset: col.max() - col.min() + offset, offset=-5)
A    1
B    1
C    4
>>> df.apply(lambda col, offset: col.max() - col.min() + offset, args=(-5,))
A    1
B    1
C    4

>>> df.apply(lambda col: col.head(2))
   A  B  C
0  3  9 -4
1  9  3 -2


df.B.apply(nltk.word_tokenize) or 用 df.B.map(nltk.word_tokenize)　一樣的意思

```

 

#### elemenwise scenario (Non-aggregation) 

```python
>>> df.apply(lambda col: col**2)
    A   B   C
0   9  81  16
1  81   9   4
2  49  36   4
3   9  25  25
>>> df.A * df.A
0     9
1    81
2    49
3     9
```



![img](https://tva1.sinaimg.cn/large/008i3skNgy1gqedbu9k0fj31b80kumzs.jpg)

ref :https://stackoverflow.com/questions/19798153/difference-between-map-applymap-and-apply-methods-in-pandas



## Summary

This part refs from other sites

- `DataFrame.apply` operates on entire rows or columns at a time.
- `DataFrame.applymap`, `Series.apply`, and `Series.map` operate on one element at time.

`Series.apply` and `Series.map` are similar and often interchangeable. Some of their slight differences are discussed in [osa's answer](https://stackoverflow.com/a/27368948/5405967) below.



**總結**

- Series中有apply及map

  兩者功能相似，都是將原本的值轉換成另一個值

  但是map可以給定function、dictionary、Series

  而apply只能給定function，但同時它有args可以設定，可以給定輸入函數的額外參數

  

- DataFrame中有apply和applymap

  apply是針對column的aggregates操作

  applymap則是element-wise

  但是如果使用apply時，給定的函數是一個ufunc，那麼它也會有element-wise的結果

ref https://home.gamer.com.tw/creationDetail.php?sn=4219422



#### More Cases

https://zhuanlan.zhihu.com/p/100064394



# Grouped

```python
df = pd.DataFrame({'A':['bob','sos','bob','sos','bob','sos','bob','bob'],
               'B':['one','one','two','three','two','two','one','three'],
               'C':[3,1,4,1,5,9,None,6],
               'D':[1,2,3,None,5,6,7,8]})
```



```python
>>> for name,group in grouped:
...     print(name)
...     print(group)
...
bob
     A      B    C    D
0  bob    one  3.0  1.0
2  bob    two  4.0  3.0
4  bob    two  5.0  5.0
6  bob    one  NaN  7.0
7  bob  three  6.0  8.0
sos
     A      B    C    D
1  sos    one  1.0  2.0
3  sos  three  1.0  NaN
5  sos    two  9.0  6.0
>>> grouped
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x108db2780>
>>> df
     A      B    C    D
0  bob    one  3.0  1.0
1  sos    one  1.0  2.0
2  bob    two  4.0  3.0
3  sos  three  1.0  NaN
4  bob    two  5.0  5.0
5  sos    two  9.0  6.0
6  bob    one  NaN  7.0
7  bob  three  6.0  8.0
>>> grouped.apply(lambda df: df.head(2))
         A      B    C    D
A
bob 0  bob    one  3.0  1.0
    2  bob    two  4.0  3.0
sos 1  sos    one  1.0  2.0
    3  sos  three  1.0  NaN
>>> grouped.apply(lambda df, n: df.head(n), n=3)	# PS, 傳參方式只能單傳，不像一般的df，有args可以包在一起傳
           B    C    D
A
bob 0    one  3.0  1.0
    2    two  4.0  3.0
    4    two  5.0  5.0
sos 1    one  1.0  2.0
    3  three  1.0  NaN
    5    two  9.0  6.0  

>>> grouped.mean()
            C    D
A
bob  4.500000  4.8
sos  3.666667  4.0
>>> df.mean()
C    4.142857
D    4.571429
dtype: float64
  
>>> df.fillna(df.mean())
     A      B         C         D
0  bob    one  3.000000  1.000000
1  sos    one  1.000000  2.000000
2  bob    two  4.000000  3.000000
3  sos  three  1.000000  4.571429
4  bob    two  5.000000  5.000000
5  sos    two  9.000000  6.000000
6  bob    one  4.142857  7.000000
7  bob  three  6.000000  8.000000
>>> grouped.apply(lambda df: df.fillna(df.mean()))
           B    C    D
A
bob 0    one  3.0  1.0
    2    two  4.0  3.0
    4    two  5.0  5.0
    6    one  4.5  7.0
    7  three  6.0  8.0
sos 1    one  1.0  2.0
    3  three  1.0  4.0
    5    two  9.0  6.0
>>> grouped.groups
{'bob': [0, 2, 4, 6, 7], 'sos': [1, 3, 5]}

  
>>> grouped.apply(lambda df: df.describe())
                  C         D
A
bob count  4.000000  5.000000
    mean   4.500000  4.800000
    std    1.290994  2.863564
    min    3.000000  1.000000
    25%    3.750000  3.000000
    50%    4.500000  5.000000
    75%    5.250000  7.000000
    max    6.000000  8.000000
sos count  3.000000  2.000000
    mean   3.666667  4.000000
    std    4.618802  2.828427
    min    1.000000  2.000000
    25%    1.000000  3.000000
    50%    1.000000  4.000000
    75%    5.000000  5.000000
    max    9.000000  6.000000  
    
    
```

##### pandas.core.groupby.GroupBy.head

- `GroupBy.``head`(*n=5*)[[source\]](https://github.com/pandas-dev/pandas/blob/v1.2.4/pandas/core/groupby/groupby.py#L2764-L2800)

  Return first n rows of each group.Similar to `.apply(lambda x: x.head(n))`, but it returns a subset of rows from the original DataFrame with original index and order preserved (`as_index` flag is ignored).Does not work for negative values of n.ReturnsSeries or DataFrame