---
layout: post
title: "golden gate fields"
author: "Junki Cho"
categories: python
tags: [documentation,prediction]
image: grandstand.jpg
---

# this is ipython convert to markdown test!!!

```python
taxi = pd.read_csv("/Users/jisuim/Desktop/tp_taxi/jisu/train.csv")
```


```python
taxi_ex = taxi.copy()
```


```python
taxi_ex.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>dropoff_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
      <th>trip_duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id2875421</td>
      <td>2</td>
      <td>2016-03-14 17:24:55</td>
      <td>2016-03-14 17:32:30</td>
      <td>1</td>
      <td>-73.982155</td>
      <td>40.767937</td>
      <td>-73.964630</td>
      <td>40.765602</td>
      <td>N</td>
      <td>455</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id2377394</td>
      <td>1</td>
      <td>2016-06-12 00:43:35</td>
      <td>2016-06-12 00:54:38</td>
      <td>1</td>
      <td>-73.980415</td>
      <td>40.738564</td>
      <td>-73.999481</td>
      <td>40.731152</td>
      <td>N</td>
      <td>663</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id3858529</td>
      <td>2</td>
      <td>2016-01-19 11:35:24</td>
      <td>2016-01-19 12:10:48</td>
      <td>1</td>
      <td>-73.979027</td>
      <td>40.763939</td>
      <td>-74.005333</td>
      <td>40.710087</td>
      <td>N</td>
      <td>2124</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id3504673</td>
      <td>2</td>
      <td>2016-04-06 19:32:31</td>
      <td>2016-04-06 19:39:40</td>
      <td>1</td>
      <td>-74.010040</td>
      <td>40.719971</td>
      <td>-74.012268</td>
      <td>40.706718</td>
      <td>N</td>
      <td>429</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id2181028</td>
      <td>2</td>
      <td>2016-03-26 13:30:55</td>
      <td>2016-03-26 13:38:10</td>
      <td>1</td>
      <td>-73.973053</td>
      <td>40.793209</td>
      <td>-73.972923</td>
      <td>40.782520</td>
      <td>N</td>
      <td>435</td>
    </tr>
  </tbody>
</table>
</div>



# vendor_id -> dummy variable 변환


```python
dummy_id=pd.get_dummies(taxi_ex['vendor_id'],prefix='id')
```


```python
dummy_id.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id_1</th>
      <th>id_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
y = list(taxi_ex['trip_duration'])
```


```python
model = sm.OLS(y, dummy_id)
result = model.fit()
print(result.summary())  
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                  0.000
    Method:                 Least Squares   F-statistic:                     601.6
    Date:                Sat, 24 Feb 2018   Prob (F-statistic):          8.05e-133
    Time:                        13:36:05   Log-Likelihood:            -1.4561e+07
    No. Observations:             1458644   AIC:                         2.912e+07
    Df Residuals:                 1458642   BIC:                         2.912e+07
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    id_1         845.4382      6.358    132.977      0.000     832.977     857.899
    id_2        1058.6432      5.928    178.588      0.000    1047.025    1070.262
    ================================================================================
    Omnibus:                  8287020.863   Durbin-Watson:                     2.000
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):   2247942471552562.000
    Skew:                         343.397   Prob(JB):                           0.00
    Kurtosis:                  192321.380   Cond. No.                           1.07
    ================================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


> vendor_id가 2일 때 trip_duration이 더 길다

# pickup datetime
- 시간대별
- 월별
- 요일별

### 월별 OLS


```python
for i in range(len(taxi)):
    k = taxi_ex['pickup_datetime'][i][5:7]
    taxi_ex.at[i, "month"] = k
## 월별 column 추가
```


```python
dummy_month = pd.get_dummies(taxi_ex['month'],prefix='month')

```


```python
model = sm.OLS(y, dummy_month)
result = model.fit()
print(result.summary())  
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                  0.000
    Method:                 Least Squares   F-statistic:                     13.69
    Date:                Sat, 24 Feb 2018   Prob (F-statistic):           2.14e-13
    Time:                        13:34:03   Log-Likelihood:            -1.4561e+07
    No. Observations:             1458644   AIC:                         2.912e+07
    Df Residuals:                 1458638   BIC:                         2.912e+07
    Df Model:                           5                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    month_01     922.3733     10.928     84.408      0.000     900.956     943.791
    month_02     920.8305     10.729     85.829      0.000     899.803     941.858
    month_03     937.3654     10.347     90.590      0.000     917.085     957.646
    month_04     962.8915     10.440     92.228      0.000     942.429     983.354
    month_05     999.4506     10.506     95.127      0.000     978.858    1020.043
    month_06    1013.3672     10.820     93.661      0.000     992.161    1034.573
    ================================================================================
    Omnibus:                  8285764.005   Durbin-Watson:                     2.000
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):   2244212437297335.250
    Skew:                         343.201   Prob(JB):                           0.00
    Kurtosis:                  192161.756   Cond. No.                           1.06
    ================================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


### 시간대별 OLS


```python
for i in range(len(taxi)):
    k = taxi_ex['pickup_datetime'][i][11:13]
    taxi_ex.at[i, "time"] = k  
```


```python
dummy_time = pd.get_dummies(taxi_ex['time'],prefix='time')
```


```python
model = sm.OLS(y, dummy_time)
result = model.fit()
print(result.summary())  
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                  0.000
    Method:                 Least Squares   F-statistic:                     14.33
    Date:                Sat, 24 Feb 2018   Prob (F-statistic):           4.83e-56
    Time:                        13:39:35   Log-Likelihood:            -1.4561e+07
    No. Observations:             1458644   AIC:                         2.912e+07
    Df Residuals:                 1458620   BIC:                         2.912e+07
    Df Model:                          23                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    time_00      936.6573     22.695     41.272      0.000     892.177     981.138
    time_01      903.0871     26.665     33.868      0.000     850.825     955.350
    time_02      890.0769     31.312     28.426      0.000     828.706     951.447
    time_03      890.0709     36.229     24.568      0.000     819.064     961.078
    time_04      921.9764     41.673     22.124      0.000     840.299    1003.654
    time_05      822.2990     42.756     19.232      0.000     738.498     906.099
    time_06      797.4349     28.720     27.765      0.000     741.144     853.726
    time_07      831.7583     22.209     37.451      0.000     788.229     875.288
    time_08      924.5592     20.224     45.716      0.000     884.921     964.197
    time_09      933.5289     20.132     46.369      0.000     894.070     972.988
    time_10      933.3676     20.472     45.592      0.000     893.243     973.492
    time_11      966.4303     20.013     48.291      0.000     927.206    1005.654
    time_12      993.5331     19.534     50.862      0.000     955.247    1031.819
    time_13     1032.2464     19.589     52.696      0.000     993.854    1070.639
    time_14     1075.7893     19.213     55.992      0.000    1038.132    1113.447
    time_15     1118.8325     19.542     57.252      0.000    1080.530    1157.135
    time_16     1080.1463     20.650     52.307      0.000    1039.673    1120.620
    time_17     1030.5816     18.936     54.424      0.000     993.468    1067.696
    time_18      981.5832     17.398     56.418      0.000     947.483    1015.683
    time_19      894.3567     17.426     51.322      0.000     860.201     928.512
    time_20      879.4615     18.061     48.693      0.000     844.062     914.861
    time_21      890.5212     18.049     49.339      0.000     855.146     925.897
    time_22     1023.4559     18.458     55.446      0.000     987.278    1059.634
    time_23      925.0914     19.824     46.665      0.000     886.237     963.946
    ================================================================================
    Omnibus:                  8286240.963   Durbin-Watson:                     2.000
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):   2245469225323410.750
    Skew:                         343.276   Prob(JB):                           0.00
    Kurtosis:                  192215.554   Cond. No.                           2.46
    ================================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


### 요일별 OLS


```python
import calendar
for i in range(len(taxi)):
    m = taxi_ex['pickup_datetime'][i][5:7]
    d = taxi_ex['pickup_datetime'][i][8:10]
    k = calendar.weekday(2016, int(m), int(d))
    taxi_ex.at[i, "week"] = k
```


```python
dummy_week = pd.get_dummies(taxi_ex['week'],prefix='week')
```


```python
model = sm.OLS(y, dummy_week)
result = model.fit()
print(result.summary())  
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                  0.000
    Method:                 Least Squares   F-statistic:                     13.72
    Date:                Sat, 24 Feb 2018   Prob (F-statistic):           1.20e-15
    Time:                        13:56:17   Log-Likelihood:            -1.4561e+07
    No. Observations:             1458644   AIC:                         2.912e+07
    Df Residuals:                 1458637   BIC:                         2.912e+07
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    week_0.0     897.9478     12.098     74.225      0.000     874.237     921.659
    week_1.0     983.4631     11.631     84.553      0.000     960.666    1006.260
    week_2.0     975.4505     11.425     85.378      0.000     953.058     997.843
    week_3.0    1006.5287     11.202     89.850      0.000     984.573    1028.485
    week_4.0     990.2242     11.077     89.392      0.000     968.513    1011.935
    week_5.0     948.0512     11.144     85.073      0.000     926.209     969.893
    week_6.0     901.6394     11.849     76.094      0.000     878.416     924.863
    ================================================================================
    Omnibus:                  8285707.358   Durbin-Watson:                     2.000
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):   2244080987088621.750
    Skew:                         343.192   Prob(JB):                           0.00
    Kurtosis:                  192156.128   Cond. No.                           1.09
    ================================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


# passenger count


```python
X = sm.add_constant(taxi_ex['passenger_count'])
```


```python
model = sm.OLS(y, X)
result = model.fit()
print(result.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.000
    Model:                            OLS   Adj. R-squared:                  0.000
    Method:                 Least Squares   F-statistic:                     104.7
    Date:                Sat, 24 Feb 2018   Prob (F-statistic):           1.44e-24
    Time:                        15:17:53   Log-Likelihood:            -1.4561e+07
    No. Observations:             1458644   AIC:                         2.912e+07
    Df Residuals:                 1458642   BIC:                         2.912e+07
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ===================================================================================
                          coef    std err          t      P>|t|      [0.025      0.975]
    -----------------------------------------------------------------------------------
    const             903.3010      6.998    129.085      0.000     889.586     917.016
    passenger_count    33.7580      3.300     10.231      0.000      27.291      40.225
    ================================================================================
    Omnibus:                  8285779.904   Durbin-Watson:                     2.000
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):   2244313951622524.750
    Skew:                         343.204   Prob(JB):                           0.00
    Kurtosis:                  192166.102   Cond. No.                           3.93
    ================================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
