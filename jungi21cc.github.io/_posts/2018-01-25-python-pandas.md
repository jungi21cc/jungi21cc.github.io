---
layout: post
title: Python pandas
tags: [Computer Science]

---

### Python pandas

### 1. *selection*

#### - getting

```python
In [23]: df['A']
Out[23]:
2013-01-01    0.469112
2013-01-02    1.212112
2013-01-03   -0.861849
2013-01-04    0.721555
2013-01-05   -0.424972
2013-01-06   -0.673690
Freq: D, Name: A, dtype: float64
```

```python
In [24]: df[0:3]
Out[24]:
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804

```

#### - selecting by label

```python
In [26]: df.loc[dates[0]]
Out[26]:
A    0.469112
B   -0.282863
C   -1.509059
D   -1.135632
Name: 2013-01-01 00:00:00, dtype: float64
```

#### - Selecting by multi label

```python
In [27]: df.loc[:,['A','B']]
Out[27]:
                   A         B
2013-01-01  0.469112 -0.282863
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
2013-01-05 -0.424972  0.567020
2013-01-06 -0.673690  0.113648
```

```python
In [28]: df.loc['20130102':'20130104',['A','B']]
Out[28]:
                   A         B
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
```

#### - selection by position

```python
In [36]: df.iloc[:,1:3]
Out[36]:
                   B         C
2013-01-01 -0.282863 -1.509059
2013-01-02 -0.173215  0.119209
2013-01-03 -2.104569 -0.494929
2013-01-04 -0.706771 -1.039575
2013-01-05  0.567020  0.276232
2013-01-06  0.113648 -1.478427
```


### 2. *Merge, join, and concatenate*

#### - Concatenating objects

```python
In [1]: df1 = pd.DataFrame({'A': ['A0', 'A1', 'A2', 'A3'],
   ...:                     'B': ['B0', 'B1', 'B2', 'B3'],
   ...:                     'C': ['C0', 'C1', 'C2', 'C3'],
   ...:                     'D': ['D0', 'D1', 'D2', 'D3']},
   ...:                     index=[0, 1, 2, 3])
   ...:

In [2]: df2 = pd.DataFrame({'A': ['A4', 'A5', 'A6', 'A7'],
   ...:                     'B': ['B4', 'B5', 'B6', 'B7'],
   ...:                     'C': ['C4', 'C5', 'C6', 'C7'],
   ...:                     'D': ['D4', 'D5', 'D6', 'D7']},
   ...:                      index=[4, 5, 6, 7])
   ...:

In [3]: df3 = pd.DataFrame({'A': ['A8', 'A9', 'A10', 'A11'],
   ...:                     'B': ['B8', 'B9', 'B10', 'B11'],
   ...:                     'C': ['C8', 'C9', 'C10', 'C11'],
   ...:                     'D': ['D8', 'D9', 'D10', 'D11']},
   ...:                     index=[8, 9, 10, 11])
   ...:

In [4]: frames = [df1, df2, df3]

In [5]: result = pd.concat(frames)

```
![alt text](/assets/img/merging_concat_basic.png)


```python
result = pd.concat(frames, keys=['x', 'y', 'z'])

```
![alt text](/assets/img/merging_concat_keys.png)


```python
In [8]: df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
   ...:                  'D': ['D2', 'D3', 'D6', 'D7'],
   ...:                  'F': ['F2', 'F3', 'F6', 'F7']},
   ...:                 index=[2, 3, 6, 7])
   ...:

In [9]: result = pd.concat([df1, df4], axis=1)
```

![alt text](/assets/img/merging_concat_axis1.png)


```python
In [10]: result = pd.concat([df1, df4], axis=1, join='inner')
```

![alt text](/assets/img/merging_concat_axis1_inner.png)


```python
In [11]: result = pd.concat([df1, df4], axis=1, join_axes=[df1.index])
```

![alt text](/assets/img/merging_concat_axis1_join_axes.png)





```python
In [15]: result = pd.concat([df1, df4], ignore_index=True)
```

![alt text](/assets/img/merging_concat_ignore_index.png)




```python
In [17]: s1 = pd.Series(['X0', 'X1', 'X2', 'X3'], name='X')

In [18]: result = pd.concat([df1, s1], axis=1)

```
![alt text](/assets/img/merging_concat_mixed_ndim.png)


#### - concatenating with group keys


```python
In [28]: pieces = {'x': df1, 'y': df2, 'z': df3}

In [29]: result = pd.concat(pieces)
```
![alt text](/assets/img/merging_concat_dict.png)



#### - Database-style DataFrame joining/merging

#### - one-to-one

#### - many-to-one

#### - many-to-many


```python
In [38]: left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
   ....:                      'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3']})
   ....:

In [39]: right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
   ....:                       'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']})
   ....:

In [40]: result = pd.merge(left, right, on='key')
```

![alt text](/assets/img/merging_merge_on_key.png)



```python
In [41]: left = pd.DataFrame({'key1': ['K0', 'K0', 'K1', 'K2'],
   ....:                      'key2': ['K0', 'K1', 'K0', 'K1'],
   ....:                      'A': ['A0', 'A1', 'A2', 'A3'],
   ....:                      'B': ['B0', 'B1', 'B2', 'B3']})
   ....:

In [42]: right = pd.DataFrame({'key1': ['K0', 'K1', 'K1', 'K2'],
   ....:                       'key2': ['K0', 'K0', 'K0', 'K0'],
   ....:                       'C': ['C0', 'C1', 'C2', 'C3'],
   ....:                       'D': ['D0', 'D1', 'D2', 'D3']})
   ....:

In [43]: result = pd.merge(left, right, on=['key1', 'key2'])
```

![alt text](/assets/img/merging_merge_on_key_multiple.png)


| Merge method  | SQL Join Name |
| :-----------: |:-------------:|
| left  | LEFT OUTER JOIN   |
| right |	RIGHT OUTER JOIN  |
| outer | FULL OUTER JOIN   |
| outer |	INNER JOIN        |


```python
In [44]: result = pd.merge(left, right, how='left', on=['key1', 'key2'])
```

![alt text](/assets/img/merging_merge_on_key_left.png)


```python
In [45]: result = pd.merge(left, right, how='right', on=['key1', 'key2'])
```

![alt text](/assets/img/merging_merge_on_key_right.png)


```python
In [46]: result = pd.merge(left, right, how='outer', on=['key1', 'key2'])
```

![alt text](/assets/img/merging_merge_on_key_outer.png)


```python
In [47]: result = pd.merge(left, right, how='inner', on=['key1', 'key2'])
```

![alt text](/assets/img/merging_merge_on_key_inner.png)


```python
In [48]: left = pd.DataFrame({'A' : [1,2], 'B' : [2, 2]})

In [49]: right = pd.DataFrame({'A' : [4,5,6], 'B': [2,2,2]})

In [50]: result = pd.merge(left, right, on='B', how='outer')
```

![alt text](/assets/img/merging_merge_on_key_dup.png)



***
*Reference*

[10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)
[Merge, join, and concatenate - pandas documents](https://pandas.pydata.org/pandas-docs/stable/merging.html)
