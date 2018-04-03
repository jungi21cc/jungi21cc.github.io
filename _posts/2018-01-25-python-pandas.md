---
layout: post
title: Python pandas
tags: [Computer Science]
---

1. **selection**

*getting*

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

also slicing is available:

```python
In [24]: df[0:3]
Out[24]:
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804

```

*selecting by lable*

```python
In [26]: df.loc[dates[0]]
Out[26]:
A    0.469112
B   -0.282863
C   -1.509059
D   -1.135632
Name: 2013-01-01 00:00:00, dtype: float64
```

Selecting by multi label:
<br/>

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

this is ultimate slicing form:
<br/>

```python
In [28]: df.loc['20130102':'20130104',['A','B']]
Out[28]:
                   A         B
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
```

*selection by position*

it is ultimately the most frequent usage of pandas function:

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

you have to understand what is happening at *[:, 1:3]*

- *:* selects entire rows.<br/>
- *,* separates row to columns. if you do not use *,*, it will slice only rows.
- *1:3* selects 1,2 columns. you have to be careful 1, 2 means B and C
>lets assume columns [A, B, C, D, E]


2. **Merge, join, and concatenate**


```

```




***
*Reference*

[10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)
[Merge, join, and concatenate | pandas documents](https://pandas.pydata.org/pandas-docs/stable/merging.html)
