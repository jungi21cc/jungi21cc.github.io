---
layout: post
title: Kaggle Airbnb New User Booking
tags: [Project]

---

### Kaggle : Airbnb New User Booking

### Abstact :

- purpose of analysis : Predict destination country

- evaluation metric : NDCG (Normalized discounted cumulative gain)

$$DCG_k=\sum_{i=1}^k\frac{2^{rel_i}-1}{\log_2{\left(i+1\right)}}$$

$$nDCG_k=\frac{DCG_k}{IDCG_k}$$


- model : Light GBM

- score :  0.88461 (top 11%)


```python
import re
import gc
from datetime import datetime

import requests
from bs4 import BeautifulSoup

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline

from IPython.core.display import Image 
from IPython.display import display

from scipy.stats import mode
import scipy.stats as stats

from sklearn.tree import export_graphviz
from sklearn import preprocessing
from sklearn.preprocessing import Imputer
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import confusion_matrix
from sklearn.metrics import classification_report
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import train_test_split
from sklearn.feature_selection import SelectFromModel


import lightgbm as lgb
from lightgbm import plot_importance

import warnings

warnings.filterwarnings('ignore')
```


```python
df_train = pd.read_csv("train_users_2.csv")
df_test = pd.read_csv("test_users.csv")
df_countries = pd.read_csv('countries.csv')
df_sessions = pd.read_csv('sessions.csv')
df_age_bkts = pd.read_csv('age_gender_bkts.csv')
```


```python
df_all = pd.concat([df_train, df_test], axis=0)
train_id = df_train.id
test_id = df_test.id
all_id = df_all.id
```


```python
df_train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 213451 entries, 0 to 213450
    Data columns (total 16 columns):
    id                         213451 non-null object
    date_account_created       213451 non-null object
    timestamp_first_active     213451 non-null int64
    date_first_booking         88908 non-null object
    gender                     213451 non-null object
    age                        125461 non-null float64
    signup_method              213451 non-null object
    signup_flow                213451 non-null int64
    language                   213451 non-null object
    affiliate_channel          213451 non-null object
    affiliate_provider         213451 non-null object
    first_affiliate_tracked    207386 non-null object
    signup_app                 213451 non-null object
    first_device_type          213451 non-null object
    first_browser              213451 non-null object
    country_destination        213451 non-null object
    dtypes: float64(1), int64(2), object(13)
    memory usage: 26.1+ MB



```python
df_train.head()
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
      <th>date_account_created</th>
      <th>timestamp_first_active</th>
      <th>date_first_booking</th>
      <th>gender</th>
      <th>age</th>
      <th>signup_method</th>
      <th>signup_flow</th>
      <th>language</th>
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>first_affiliate_tracked</th>
      <th>signup_app</th>
      <th>first_device_type</th>
      <th>first_browser</th>
      <th>country_destination</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>gxn3p5htnn</td>
      <td>2010-06-28</td>
      <td>20090319043255</td>
      <td>NaN</td>
      <td>-unknown-</td>
      <td>NaN</td>
      <td>facebook</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Chrome</td>
      <td>NDF</td>
    </tr>
    <tr>
      <th>1</th>
      <td>820tgsjxq7</td>
      <td>2011-05-25</td>
      <td>20090523174809</td>
      <td>NaN</td>
      <td>MALE</td>
      <td>38.0</td>
      <td>facebook</td>
      <td>0</td>
      <td>en</td>
      <td>seo</td>
      <td>google</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Chrome</td>
      <td>NDF</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4ft3gnwmtx</td>
      <td>2010-09-28</td>
      <td>20090609231247</td>
      <td>2010-08-02</td>
      <td>FEMALE</td>
      <td>56.0</td>
      <td>basic</td>
      <td>3</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Windows Desktop</td>
      <td>IE</td>
      <td>US</td>
    </tr>
    <tr>
      <th>3</th>
      <td>bjjt8pjhuk</td>
      <td>2011-12-05</td>
      <td>20091031060129</td>
      <td>2012-09-08</td>
      <td>FEMALE</td>
      <td>42.0</td>
      <td>facebook</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Firefox</td>
      <td>other</td>
    </tr>
    <tr>
      <th>4</th>
      <td>87mebub9p4</td>
      <td>2010-09-14</td>
      <td>20091208061105</td>
      <td>2010-02-18</td>
      <td>-unknown-</td>
      <td>41.0</td>
      <td>basic</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Chrome</td>
      <td>US</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_test.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 62096 entries, 0 to 62095
    Data columns (total 15 columns):
    id                         62096 non-null object
    date_account_created       62096 non-null object
    timestamp_first_active     62096 non-null int64
    date_first_booking         0 non-null float64
    gender                     62096 non-null object
    age                        33220 non-null float64
    signup_method              62096 non-null object
    signup_flow                62096 non-null int64
    language                   62096 non-null object
    affiliate_channel          62096 non-null object
    affiliate_provider         62096 non-null object
    first_affiliate_tracked    62076 non-null object
    signup_app                 62096 non-null object
    first_device_type          62096 non-null object
    first_browser              62096 non-null object
    dtypes: float64(2), int64(2), object(11)
    memory usage: 7.1+ MB



```python
df_test.head()
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
      <th>date_account_created</th>
      <th>timestamp_first_active</th>
      <th>date_first_booking</th>
      <th>gender</th>
      <th>age</th>
      <th>signup_method</th>
      <th>signup_flow</th>
      <th>language</th>
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>first_affiliate_tracked</th>
      <th>signup_app</th>
      <th>first_device_type</th>
      <th>first_browser</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5uwns89zht</td>
      <td>2014-07-01</td>
      <td>20140701000006</td>
      <td>NaN</td>
      <td>FEMALE</td>
      <td>35.0</td>
      <td>facebook</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Moweb</td>
      <td>iPhone</td>
      <td>Mobile Safari</td>
    </tr>
    <tr>
      <th>1</th>
      <td>jtl0dijy2j</td>
      <td>2014-07-01</td>
      <td>20140701000051</td>
      <td>NaN</td>
      <td>-unknown-</td>
      <td>NaN</td>
      <td>basic</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Moweb</td>
      <td>iPhone</td>
      <td>Mobile Safari</td>
    </tr>
    <tr>
      <th>2</th>
      <td>xx0ulgorjt</td>
      <td>2014-07-01</td>
      <td>20140701000148</td>
      <td>NaN</td>
      <td>-unknown-</td>
      <td>NaN</td>
      <td>basic</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>linked</td>
      <td>Web</td>
      <td>Windows Desktop</td>
      <td>Chrome</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6c6puo6ix0</td>
      <td>2014-07-01</td>
      <td>20140701000215</td>
      <td>NaN</td>
      <td>-unknown-</td>
      <td>NaN</td>
      <td>basic</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>linked</td>
      <td>Web</td>
      <td>Windows Desktop</td>
      <td>IE</td>
    </tr>
    <tr>
      <th>4</th>
      <td>czqhjk3yfe</td>
      <td>2014-07-01</td>
      <td>20140701000305</td>
      <td>NaN</td>
      <td>-unknown-</td>
      <td>NaN</td>
      <td>basic</td>
      <td>0</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Safari</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_sessions.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10567737 entries, 0 to 10567736
    Data columns (total 6 columns):
    user_id          object
    action           object
    action_type      object
    action_detail    object
    device_type      object
    secs_elapsed     float64
    dtypes: float64(1), object(5)
    memory usage: 483.8+ MB



```python
df_sessions.head()
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
      <th>user_id</th>
      <th>action</th>
      <th>action_type</th>
      <th>action_detail</th>
      <th>device_type</th>
      <th>secs_elapsed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>d1mm9tcy42</td>
      <td>lookup</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Windows Desktop</td>
      <td>319.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>d1mm9tcy42</td>
      <td>search_results</td>
      <td>click</td>
      <td>view_search_results</td>
      <td>Windows Desktop</td>
      <td>67753.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>d1mm9tcy42</td>
      <td>lookup</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Windows Desktop</td>
      <td>301.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>d1mm9tcy42</td>
      <td>search_results</td>
      <td>click</td>
      <td>view_search_results</td>
      <td>Windows Desktop</td>
      <td>22141.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>d1mm9tcy42</td>
      <td>lookup</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Windows Desktop</td>
      <td>435.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_countries.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10 entries, 0 to 9
    Data columns (total 7 columns):
    country_destination              10 non-null object
    lat_destination                  10 non-null float64
    lng_destination                  10 non-null float64
    distance_km                      10 non-null float64
    destination_km2                  10 non-null float64
    destination_language             10 non-null object
    language_levenshtein_distance    10 non-null float64
    dtypes: float64(5), object(2)
    memory usage: 640.0+ bytes



```python
df_countries.head()
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
      <th>country_destination</th>
      <th>lat_destination</th>
      <th>lng_destination</th>
      <th>distance_km</th>
      <th>destination_km2</th>
      <th>destination_language</th>
      <th>language_levenshtein_distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AU</td>
      <td>-26.853388</td>
      <td>133.275160</td>
      <td>15297.7440</td>
      <td>7741220.0</td>
      <td>eng</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>CA</td>
      <td>62.393303</td>
      <td>-96.818146</td>
      <td>2828.1333</td>
      <td>9984670.0</td>
      <td>eng</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>DE</td>
      <td>51.165707</td>
      <td>10.452764</td>
      <td>7879.5680</td>
      <td>357022.0</td>
      <td>deu</td>
      <td>72.61</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ES</td>
      <td>39.896027</td>
      <td>-2.487694</td>
      <td>7730.7240</td>
      <td>505370.0</td>
      <td>spa</td>
      <td>92.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>FR</td>
      <td>46.232193</td>
      <td>2.209667</td>
      <td>7682.9450</td>
      <td>643801.0</td>
      <td>fra</td>
      <td>92.06</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_age_bkts.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 420 entries, 0 to 419
    Data columns (total 5 columns):
    age_bucket                 420 non-null object
    country_destination        420 non-null object
    gender                     420 non-null object
    population_in_thousands    420 non-null float64
    year                       420 non-null float64
    dtypes: float64(2), object(3)
    memory usage: 16.5+ KB



```python
df_age_bkts.head()
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
      <th>age_bucket</th>
      <th>country_destination</th>
      <th>gender</th>
      <th>population_in_thousands</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100+</td>
      <td>AU</td>
      <td>male</td>
      <td>1.0</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>95-99</td>
      <td>AU</td>
      <td>male</td>
      <td>9.0</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90-94</td>
      <td>AU</td>
      <td>male</td>
      <td>47.0</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>85-89</td>
      <td>AU</td>
      <td>male</td>
      <td>118.0</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80-84</td>
      <td>AU</td>
      <td>male</td>
      <td>199.0</td>
      <td>2015.0</td>
    </tr>
  </tbody>
</table>
</div>



### country destination


```python
df_train.country_destination.value_counts()
```




    NDF      124543
    US        62376
    other     10094
    FR         5023
    IT         2835
    GB         2324
    ES         2249
    CA         1428
    DE         1061
    NL          762
    AU          539
    PT          217
    Name: country_destination, dtype: int64




```python
ax = sns.countplot(x=df_train.country_destination , data=df_train)
plt.show()
```

```python
round(df_train["country_destination"].value_counts() / len(df_train) * 100, 2)
```




    NDF      58.35
    US       29.22
    other     4.73
    FR        2.35
    IT        1.33
    GB        1.09
    ES        1.05
    CA        0.67
    DE        0.50
    NL        0.36
    AU        0.25
    PT        0.10
    Name: country_destination, dtype: float64



### feature engineering

### date time convert


```python
#date account create
df_all["date_account_created"] = pd.to_datetime(df_all["date_account_created"], format = "%Y-%m-%d")

#timestamp first active
df_all["timestamp_first_active"] = pd.to_datetime(df_all["timestamp_first_active"], format="%Y%m%d%H%M%S")
```


```python
df_all['create_year'] = df_all["date_account_created"].apply(lambda x : x.year)
df_all['create_month'] = df_all["date_account_created"].apply(lambda x : x.month)
df_all['create_day'] = df_all["date_account_created"].apply(lambda x : x.day)

df_all['active_year'] = df_all["timestamp_first_active"].apply(lambda x : x.year)
df_all['active_month'] = df_all["timestamp_first_active"].apply(lambda x : x.month)
df_all['active_day'] = df_all["timestamp_first_active"].apply(lambda x : x.day)
```


```python
df_all.shape
```




    (275547, 22)




```python
df_all.head()
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
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>age</th>
      <th>country_destination</th>
      <th>date_account_created</th>
      <th>date_first_booking</th>
      <th>first_affiliate_tracked</th>
      <th>first_browser</th>
      <th>first_device_type</th>
      <th>gender</th>
      <th>...</th>
      <th>signup_app</th>
      <th>signup_flow</th>
      <th>signup_method</th>
      <th>timestamp_first_active</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>direct</td>
      <td>direct</td>
      <td>NaN</td>
      <td>NDF</td>
      <td>2010-06-28</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>Web</td>
      <td>0</td>
      <td>facebook</td>
      <td>2009-03-19 04:32:55</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>seo</td>
      <td>google</td>
      <td>38.0</td>
      <td>NDF</td>
      <td>2011-05-25</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>MALE</td>
      <td>...</td>
      <td>Web</td>
      <td>0</td>
      <td>facebook</td>
      <td>2009-05-23 17:48:09</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>direct</td>
      <td>direct</td>
      <td>56.0</td>
      <td>US</td>
      <td>2010-09-28</td>
      <td>2010-08-02</td>
      <td>untracked</td>
      <td>IE</td>
      <td>Windows Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>Web</td>
      <td>3</td>
      <td>basic</td>
      <td>2009-06-09 23:12:47</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>direct</td>
      <td>direct</td>
      <td>42.0</td>
      <td>other</td>
      <td>2011-12-05</td>
      <td>2012-09-08</td>
      <td>untracked</td>
      <td>Firefox</td>
      <td>Mac Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>Web</td>
      <td>0</td>
      <td>facebook</td>
      <td>2009-10-31 06:01:29</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
    </tr>
    <tr>
      <th>4</th>
      <td>direct</td>
      <td>direct</td>
      <td>41.0</td>
      <td>US</td>
      <td>2010-09-14</td>
      <td>2010-02-18</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>Web</td>
      <td>0</td>
      <td>basic</td>
      <td>2009-12-08 06:11:05</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



### lagging time


```python
lagging = df_all["timestamp_first_active"] - df_all["date_account_created"]
```


```python
#lagging time days, log seconds
df_all["lag_days"] = lagging.apply(lambda x : -1 * x.days)
df_all["lag_seconds"] = np.log(lagging.apply(lambda x : x.seconds))
```


```python
df_all.head()
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
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>age</th>
      <th>country_destination</th>
      <th>date_account_created</th>
      <th>date_first_booking</th>
      <th>first_affiliate_tracked</th>
      <th>first_browser</th>
      <th>first_device_type</th>
      <th>gender</th>
      <th>...</th>
      <th>signup_method</th>
      <th>timestamp_first_active</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>direct</td>
      <td>direct</td>
      <td>NaN</td>
      <td>NDF</td>
      <td>2010-06-28</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>facebook</td>
      <td>2009-03-19 04:32:55</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>9.703511</td>
    </tr>
    <tr>
      <th>1</th>
      <td>seo</td>
      <td>google</td>
      <td>38.0</td>
      <td>NDF</td>
      <td>2011-05-25</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>MALE</td>
      <td>...</td>
      <td>facebook</td>
      <td>2009-05-23 17:48:09</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>11.068028</td>
    </tr>
    <tr>
      <th>2</th>
      <td>direct</td>
      <td>direct</td>
      <td>56.0</td>
      <td>US</td>
      <td>2010-09-28</td>
      <td>2010-08-02</td>
      <td>untracked</td>
      <td>IE</td>
      <td>Windows Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>basic</td>
      <td>2009-06-09 23:12:47</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>11.333404</td>
    </tr>
    <tr>
      <th>3</th>
      <td>direct</td>
      <td>direct</td>
      <td>42.0</td>
      <td>other</td>
      <td>2011-12-05</td>
      <td>2012-09-08</td>
      <td>untracked</td>
      <td>Firefox</td>
      <td>Mac Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>facebook</td>
      <td>2009-10-31 06:01:29</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>9.984560</td>
    </tr>
    <tr>
      <th>4</th>
      <td>direct</td>
      <td>direct</td>
      <td>41.0</td>
      <td>US</td>
      <td>2010-09-14</td>
      <td>2010-02-18</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>basic</td>
      <td>2009-12-08 06:11:05</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>10.010771</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>



### holiday


```python
def get_holidays(year):
    response = requests.get("https://www.timeanddate.com/calendar/custom.html?year="+str(year)+"                                &country=1&cols=3&df=1&hol=25")
    dom = BeautifulSoup(response.content, "html.parser")

    trs = dom.select("table.cht.lpad tr")

    df = pd.DataFrame(columns=["date", "holiday"])
    for tr in trs:
        datestr = tr.select_one("td:nth-of-type(1)").text
        date = datetime.strptime("{} {}".format(year, datestr), '%Y %b %d')
        holiday = tr.select_one("td:nth-of-type(2)").text
        df.loc[len(df)] = {"date" : date, "holiday": 1}
    return df

holiday_ls = []
for year in range(2009, 2015):
    df = get_holidays(year)
    holiday_ls.append(df)
    holiday_df = pd.concat(holiday_ls)
```


```python
select_date = list(holiday_df["date"].astype("str"))
holiday = df_all.timestamp_first_active.apply(lambda x : str(x.date())).isin(select_date)

df_all["holiday"] = holiday
df_all['holiday'] = 1 * (df_all.holiday == True)
```


```python
df_all.head()
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
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>age</th>
      <th>country_destination</th>
      <th>date_account_created</th>
      <th>date_first_booking</th>
      <th>first_affiliate_tracked</th>
      <th>first_browser</th>
      <th>first_device_type</th>
      <th>gender</th>
      <th>...</th>
      <th>timestamp_first_active</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
      <th>holiday</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>direct</td>
      <td>direct</td>
      <td>NaN</td>
      <td>NDF</td>
      <td>2010-06-28</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>2009-03-19 04:32:55</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>9.703511</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>seo</td>
      <td>google</td>
      <td>38.0</td>
      <td>NDF</td>
      <td>2011-05-25</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>MALE</td>
      <td>...</td>
      <td>2009-05-23 17:48:09</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>11.068028</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>direct</td>
      <td>direct</td>
      <td>56.0</td>
      <td>US</td>
      <td>2010-09-28</td>
      <td>2010-08-02</td>
      <td>untracked</td>
      <td>IE</td>
      <td>Windows Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>2009-06-09 23:12:47</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>11.333404</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>direct</td>
      <td>direct</td>
      <td>42.0</td>
      <td>other</td>
      <td>2011-12-05</td>
      <td>2012-09-08</td>
      <td>untracked</td>
      <td>Firefox</td>
      <td>Mac Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>2009-10-31 06:01:29</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>9.984560</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>direct</td>
      <td>direct</td>
      <td>41.0</td>
      <td>US</td>
      <td>2010-09-14</td>
      <td>2010-02-18</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>2009-12-08 06:11:05</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>10.010771</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



### weekend


```python
weekday = df_all.filter(items=['id','timestamp_first_active'])
weekday = pd.to_datetime(weekday["timestamp_first_active"], format="%Y-%m-%d")
weekday = weekday.dt.dayofweek

df_all["weekend"] = weekday.apply(lambda x : 1 if x>=5 else 0)
```


```python
df_all.head()
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
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>age</th>
      <th>country_destination</th>
      <th>date_account_created</th>
      <th>date_first_booking</th>
      <th>first_affiliate_tracked</th>
      <th>first_browser</th>
      <th>first_device_type</th>
      <th>gender</th>
      <th>...</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
      <th>holiday</th>
      <th>weekend</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>direct</td>
      <td>direct</td>
      <td>NaN</td>
      <td>NDF</td>
      <td>2010-06-28</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>9.703511</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>seo</td>
      <td>google</td>
      <td>38.0</td>
      <td>NDF</td>
      <td>2011-05-25</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>MALE</td>
      <td>...</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>11.068028</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>direct</td>
      <td>direct</td>
      <td>56.0</td>
      <td>US</td>
      <td>2010-09-28</td>
      <td>2010-08-02</td>
      <td>untracked</td>
      <td>IE</td>
      <td>Windows Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>11.333404</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>direct</td>
      <td>direct</td>
      <td>42.0</td>
      <td>other</td>
      <td>2011-12-05</td>
      <td>2012-09-08</td>
      <td>untracked</td>
      <td>Firefox</td>
      <td>Mac Desktop</td>
      <td>FEMALE</td>
      <td>...</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>9.984560</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>direct</td>
      <td>direct</td>
      <td>41.0</td>
      <td>US</td>
      <td>2010-09-14</td>
      <td>2010-02-18</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>...</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>10.010771</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>




```python
df_all = df_all.drop("date_account_created" , axis=1)
df_all = df_all.drop("timestamp_first_active" , axis=1)
```

### faithless sign-in


```python
checklist = (df_all['age'] < 120) & (df_all['gender'] != '-unknown-')

df_all['faithless_sign'] = checklist.apply(lambda x : 0 if x == True else 1)
```


```python
df_all.head()
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
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>age</th>
      <th>country_destination</th>
      <th>date_first_booking</th>
      <th>first_affiliate_tracked</th>
      <th>first_browser</th>
      <th>first_device_type</th>
      <th>gender</th>
      <th>id</th>
      <th>...</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
      <th>holiday</th>
      <th>weekend</th>
      <th>faithless_sign</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>direct</td>
      <td>direct</td>
      <td>NaN</td>
      <td>NDF</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>gxn3p5htnn</td>
      <td>...</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>9.703511</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>seo</td>
      <td>google</td>
      <td>38.0</td>
      <td>NDF</td>
      <td>NaN</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>MALE</td>
      <td>820tgsjxq7</td>
      <td>...</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>11.068028</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>direct</td>
      <td>direct</td>
      <td>56.0</td>
      <td>US</td>
      <td>2010-08-02</td>
      <td>untracked</td>
      <td>IE</td>
      <td>Windows Desktop</td>
      <td>FEMALE</td>
      <td>4ft3gnwmtx</td>
      <td>...</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>11.333404</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>direct</td>
      <td>direct</td>
      <td>42.0</td>
      <td>other</td>
      <td>2012-09-08</td>
      <td>untracked</td>
      <td>Firefox</td>
      <td>Mac Desktop</td>
      <td>FEMALE</td>
      <td>bjjt8pjhuk</td>
      <td>...</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>9.984560</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>direct</td>
      <td>direct</td>
      <td>41.0</td>
      <td>US</td>
      <td>2010-02-18</td>
      <td>untracked</td>
      <td>Chrome</td>
      <td>Mac Desktop</td>
      <td>-unknown-</td>
      <td>87mebub9p4</td>
      <td>...</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>10.010771</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



### null data


```python
df_train.isnull().sum()
```




    id                              0
    date_account_created            0
    timestamp_first_active          0
    date_first_booking         124543
    gender                          0
    age                         87990
    signup_method                   0
    signup_flow                     0
    language                        0
    affiliate_channel               0
    affiliate_provider              0
    first_affiliate_tracked      6065
    signup_app                      0
    first_device_type               0
    first_browser                   0
    country_destination             0
    dtype: int64




```python
df_test.isnull().sum()
```




    id                             0
    date_account_created           0
    timestamp_first_active         0
    date_first_booking         62096
    gender                         0
    age                        28876
    signup_method                  0
    signup_flow                    0
    language                       0
    affiliate_channel              0
    affiliate_provider             0
    first_affiliate_tracked       20
    signup_app                     0
    first_device_type              0
    first_browser                  0
    dtype: int64




```python
#null data of country_destination is due to concanate train / test data.
df_all.isnull().sum()
```




    affiliate_channel               0
    affiliate_provider              0
    age                        116866
    country_destination         62096
    date_first_booking         186639
    first_affiliate_tracked      6085
    first_browser                   0
    first_device_type               0
    gender                          0
    id                              0
    language                        0
    signup_app                      0
    signup_flow                     0
    signup_method                   0
    create_year                     0
    create_month                    0
    create_day                      0
    active_year                     0
    active_month                    0
    active_day                      0
    lag_days                        0
    lag_seconds                     0
    holiday                         0
    weekend                         0
    faithless_sign                  0
    dtype: int64




```python
print('Train data missing data ratio')
print('date_frist_booking      :',round(df_train.date_first_booking.isnull().sum() / len(df_train) * 100, 2), "%")
print('age                     :',round(df_train.age.isnull().sum() / len(df_train) * 100, 2), "%")
print('first_affiliate_tracked :',round(df_train.first_affiliate_tracked.isnull().sum() / len(df_train) * 100, 2), "%")
```

    Train data missing data ratio
    date_frist_booking      : 58.35 %
    age                     : 41.22 %
    first_affiliate_tracked : 2.84 %



```python
print('Test data missing data ratio')
print('date_frist_booking      :',round(df_test.date_first_booking.isnull().sum() / len(df_test) * 100, 2), "%")
print('age                     :',round(df_test.age.isnull().sum() / len(df_test) * 100, 2), "%")
print('first_affiliate_tracked :',round(df_test.first_affiliate_tracked.isnull().sum() / len(df_test) * 100, 2), "%")
```

    Test data missing data ratio
    date_frist_booking      : 100.0 %
    age                     : 46.5 %
    first_affiliate_tracked : 0.03 %


### drop data_first_booking data


```python
df_all = df_all.drop("date_first_booking", axis=1)
```

### fill first_affiliate_tracked data by mode


```python
sns.countplot(df_all["first_affiliate_tracked"])
plt.show()
```


![png](Airbnb_full_report_files/Airbnb_full_report_49_0.png)



```python
df_all.first_affiliate_tracked.mode()
```




    0    untracked
    dtype: object




```python
df_all["first_affiliate_tracked"] = df_all["first_affiliate_tracked"].replace(np.nan, "untracked")
```

### age data


```python
#more than 200 years old data
#keep this data for faithless-signin
#training data only
len(df_train[df_train['age']>200])
```




    779




```python
df_train[df_train['age']>200].head()
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
      <th>date_account_created</th>
      <th>timestamp_first_active</th>
      <th>date_first_booking</th>
      <th>gender</th>
      <th>age</th>
      <th>signup_method</th>
      <th>signup_flow</th>
      <th>language</th>
      <th>affiliate_channel</th>
      <th>affiliate_provider</th>
      <th>first_affiliate_tracked</th>
      <th>signup_app</th>
      <th>first_device_type</th>
      <th>first_browser</th>
      <th>country_destination</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>388</th>
      <td>v2x0ms9c62</td>
      <td>2010-04-11</td>
      <td>20100411065602</td>
      <td>2010-04-13</td>
      <td>-unknown-</td>
      <td>2014.0</td>
      <td>basic</td>
      <td>3</td>
      <td>en</td>
      <td>other</td>
      <td>craigslist</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Windows Desktop</td>
      <td>Firefox</td>
      <td>FR</td>
    </tr>
    <tr>
      <th>673</th>
      <td>umf1wdk9uc</td>
      <td>2010-05-25</td>
      <td>20100525155541</td>
      <td>NaN</td>
      <td>FEMALE</td>
      <td>2014.0</td>
      <td>basic</td>
      <td>2</td>
      <td>en</td>
      <td>other</td>
      <td>craigslist</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Safari</td>
      <td>NDF</td>
    </tr>
    <tr>
      <th>1040</th>
      <td>m82epwn7i8</td>
      <td>2010-07-14</td>
      <td>20100714230556</td>
      <td>2010-07-15</td>
      <td>MALE</td>
      <td>2014.0</td>
      <td>facebook</td>
      <td>0</td>
      <td>en</td>
      <td>other</td>
      <td>craigslist</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Chrome</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1177</th>
      <td>2th813zdx7</td>
      <td>2010-07-25</td>
      <td>20100725234419</td>
      <td>2010-07-26</td>
      <td>MALE</td>
      <td>2013.0</td>
      <td>facebook</td>
      <td>3</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Mac Desktop</td>
      <td>Chrome</td>
      <td>US</td>
    </tr>
    <tr>
      <th>1200</th>
      <td>3amf04n3o3</td>
      <td>2010-07-27</td>
      <td>20100727190447</td>
      <td>2010-07-29</td>
      <td>FEMALE</td>
      <td>2014.0</td>
      <td>basic</td>
      <td>2</td>
      <td>en</td>
      <td>direct</td>
      <td>direct</td>
      <td>untracked</td>
      <td>Web</td>
      <td>Windows Desktop</td>
      <td>IE</td>
      <td>US</td>
    </tr>
  </tbody>
</table>
</div>




```python
#on each destinations, it has various age distribution. 
#It would be better filling the null age data than dropping the age data. 
age_temp = df_train.copy()
age_temp[ age_temp['age'] > 200 ] = np.nan
sns.boxplot(x=age_temp.country_destination, y=age_temp.age, data=age_temp)
plt.show()
```


![png](Airbnb_full_report_files/Airbnb_full_report_55_0.png)


### one-hot encoding before fill age data


```python
df_age = df_all.filter(items = ['age', 'country_destination','id', 'gender'])
df_dummy = df_all.filter(items = ['affiliate_channel', 'affiliate_provider',
                                       'first_affiliate_tracked', 'first_browser', 'first_device_type',
                                       'language', 'signup_app', 'signup_flow', 'signup_method', 
                                       'create_year', 'create_month', 'create_day', 
                                       'active_year', 'active_month', 'active_day', 'lag_days', 'lag_seconds', 
                                       'holiday', 'weekend'])
    
df_dummy = pd.get_dummies(df_dummy)
df_all = pd.concat([df_age, df_dummy], axis=1)
```


```python
df_all.shape
```




    (275547, 146)




```python
df_all.head()
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
      <th>age</th>
      <th>country_destination</th>
      <th>id</th>
      <th>gender</th>
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>...</th>
      <th>language_tr</th>
      <th>language_zh</th>
      <th>signup_app_Android</th>
      <th>signup_app_Moweb</th>
      <th>signup_app_Web</th>
      <th>signup_app_iOS</th>
      <th>signup_method_basic</th>
      <th>signup_method_facebook</th>
      <th>signup_method_google</th>
      <th>signup_method_weibo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NDF</td>
      <td>gxn3p5htnn</td>
      <td>-unknown-</td>
      <td>0</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>38.0</td>
      <td>NDF</td>
      <td>820tgsjxq7</td>
      <td>MALE</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>56.0</td>
      <td>US</td>
      <td>4ft3gnwmtx</td>
      <td>FEMALE</td>
      <td>3</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>42.0</td>
      <td>other</td>
      <td>bjjt8pjhuk</td>
      <td>FEMALE</td>
      <td>0</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41.0</td>
      <td>US</td>
      <td>87mebub9p4</td>
      <td>-unknown-</td>
      <td>0</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 146 columns</p>
</div>



### fill age data


```python
#divide train / test by null age data
age_train = df_all[df_all["age"].notnull()].reset_index(drop=True)
age_test = df_all[df_all["age"].isnull()].reset_index(drop=True)
```


```python
#divide 5 cluster age data
bins = [0, 15, 25, 35, 60, 9999]
labels = ["underage", "tweenty", "thirty", "mid_old", "old"]
cats = pd.cut(age_train['age'], bins, labels=labels)
cats = pd.DataFrame(cats)
```


```python
age_train_id = age_train.id
age_test_id = age_test.id

age_train = age_train.drop(['id', 'age', 'country_destination', 'gender'], axis=1)
age_test = age_test.drop(['id', 'age', 'country_destination', 'gender'], axis=1)
```


```python
X = age_train
y = cats

age_train.shape, y.shape, age_test.shape
```




    ((158681, 142), (158681, 1), (116866, 142))




```python
#model recall rate is so low, but it gives better cross validation score for final prediction model
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=0)
    
model_age = lgb.LGBMClassifier(boosting_type='gbdt', n_jobs=-1, reg_alpha=0.5, reg_lambda=0.5).fit(X_train, y_train)
pred_age = model_age.predict(X_test)

print(classification_report(y_test, pred_age))
```

                 precision    recall  f1-score   support
    
        mid_old       0.48      0.38      0.42     11150
            old       0.75      0.00      0.00      2044
         thirty       0.49      0.78      0.60     14036
        tweenty       0.45      0.04      0.07      4490
       underage       0.25      0.06      0.10        17
    
    avg / total       0.50      0.48      0.42     31737
    



```python
#prediction age
pred_age = model_age.predict(age_test)
pred_age = pd.DataFrame(pred_age, columns=['age'])
pred_age = pd.concat([pred_age, age_test_id], axis=1)
pred_age["age"] = pred_age["age"].replace({'underage':15, "tweenty" : 25, "thirty" : 35, 'mid_old' : 45, 'old' : 60})

#original age
origin_age = y
origin_age = pd.DataFrame(origin_age, columns=['age'])
origin_age = pd.concat([origin_age, age_train_id], axis=1)
origin_age["age"] = origin_age["age"].replace({'underage':15, "tweenty" : 25, "thirty" : 35, 'mid_old' : 45, 'old' : 60})

#concat original age and prediction age
age = pd.concat([origin_age, pred_age], axis=0)
print('age lenght check :', len(age))
age.head()
```

    age lenght check : 275547





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
      <th>age</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45</td>
      <td>820tgsjxq7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>45</td>
      <td>4ft3gnwmtx</td>
    </tr>
    <tr>
      <th>2</th>
      <td>45</td>
      <td>bjjt8pjhuk</td>
    </tr>
    <tr>
      <th>3</th>
      <td>45</td>
      <td>87mebub9p4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>45</td>
      <td>lsw9q7uk0j</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_all = df_all.drop("age" , axis=1)

df_all = pd.merge(df_all, age, on="id", how="left")
```


```python
df_all.head()
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
      <th>country_destination</th>
      <th>id</th>
      <th>gender</th>
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>...</th>
      <th>language_zh</th>
      <th>signup_app_Android</th>
      <th>signup_app_Moweb</th>
      <th>signup_app_Web</th>
      <th>signup_app_iOS</th>
      <th>signup_method_basic</th>
      <th>signup_method_facebook</th>
      <th>signup_method_google</th>
      <th>signup_method_weibo</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NDF</td>
      <td>gxn3p5htnn</td>
      <td>-unknown-</td>
      <td>0</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NDF</td>
      <td>820tgsjxq7</td>
      <td>MALE</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>4ft3gnwmtx</td>
      <td>FEMALE</td>
      <td>3</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
    </tr>
    <tr>
      <th>3</th>
      <td>other</td>
      <td>bjjt8pjhuk</td>
      <td>FEMALE</td>
      <td>0</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>87mebub9p4</td>
      <td>-unknown-</td>
      <td>0</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 146 columns</p>
</div>




```python
df_all.isnull().sum()
```




    country_destination                       62096
    id                                            0
    gender                                        0
    signup_flow                                   0
    create_year                                   0
    create_month                                  0
    create_day                                    0
    active_year                                   0
    active_month                                  0
    active_day                                    0
    lag_days                                      0
    lag_seconds                                   0
    holiday                                       0
    weekend                                       0
    affiliate_channel_api                         0
    affiliate_channel_content                     0
    affiliate_channel_direct                      0
    affiliate_channel_other                       0
    affiliate_channel_remarketing                 0
    affiliate_channel_sem-brand                   0
    affiliate_channel_sem-non-brand               0
    affiliate_channel_seo                         0
    affiliate_provider_baidu                      0
    affiliate_provider_bing                       0
    affiliate_provider_craigslist                 0
    affiliate_provider_daum                       0
    affiliate_provider_direct                     0
    affiliate_provider_email-marketing            0
    affiliate_provider_facebook                   0
    affiliate_provider_facebook-open-graph        0
                                              ...  
    language_el                                   0
    language_en                                   0
    language_es                                   0
    language_fi                                   0
    language_fr                                   0
    language_hr                                   0
    language_hu                                   0
    language_id                                   0
    language_is                                   0
    language_it                                   0
    language_ja                                   0
    language_ko                                   0
    language_nl                                   0
    language_no                                   0
    language_pl                                   0
    language_pt                                   0
    language_ru                                   0
    language_sv                                   0
    language_th                                   0
    language_tr                                   0
    language_zh                                   0
    signup_app_Android                            0
    signup_app_Moweb                              0
    signup_app_Web                                0
    signup_app_iOS                                0
    signup_method_basic                           0
    signup_method_facebook                        0
    signup_method_google                          0
    signup_method_weibo                           0
    age                                           0
    Length: 146, dtype: int64



### filling gender data


```python
df_all.gender.value_counts()
```




    -unknown-    129480
    FEMALE        77524
    MALE          68209
    OTHER           334
    Name: gender, dtype: int64




```python
sns.countplot(df_all["gender"])
plt.show()
```


![png](Airbnb_full_report_files/Airbnb_full_report_72_0.png)



```python
df_all["gender"] = df_all["gender"].replace(['-unknown-', 'OTHER'], np.nan)

gender_train = df_all[df_all["gender"].notnull()].reset_index()
gender_test = df_all[df_all["gender"].isnull()].reset_index()
```


```python
y = gender_train.gender

gender_train_id = gender_train.id
gender_test_id = gender_test.id

gender_train = gender_train.drop(['id', 'age', 'country_destination', 'gender'], axis=1)
gender_test = gender_test.drop(['id', 'age', 'country_destination', 'gender'], axis=1)
```


```python
X = gender_train

gender_train.shape, y.shape, gender_test.shape
```




    ((145733, 143), (145733,), (129814, 143))




```python
#model recall rate is so low, but it gives better cross validation score for final prediction model
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20, random_state=0)

# X_train, X_test, y_train = gender_train, gender_test, y
    
model_age = lgb.LGBMClassifier(n_estimators=500, n_jobs=-1, reg_alpha=1).fit(X_train, y_train)
pred_age = model_age.predict(X_test)

print(classification_report(y_test, pred_age))
```

                 precision    recall  f1-score   support
    
         FEMALE       0.57      0.68      0.62     15550
           MALE       0.54      0.43      0.48     13597
    
    avg / total       0.56      0.56      0.55     29147
    



```python
pred_gender = model_age.predict(gender_test)
pred_gender = pd.DataFrame(pred_gender)
```


```python
#prediction age
pred_gender = model_age.predict(gender_test)
pred_gender = pd.DataFrame(pred_gender, columns=['gender'])
pred_gender = pd.concat([pred_gender, gender_test_id], axis=1)

#original age
origin_gender = y
origin_gender = pd.DataFrame(origin_gender, columns=['gender'])
origin_gender = pd.concat([origin_gender, gender_train_id], axis=1)

#concat original age and prediction age
gender = pd.concat([origin_gender, pred_gender], axis=0)
print('gender lenght check :', len(gender))
gender.head()
```

    gender lenght check : 275547





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
      <th>gender</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>MALE</td>
      <td>820tgsjxq7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>FEMALE</td>
      <td>4ft3gnwmtx</td>
    </tr>
    <tr>
      <th>2</th>
      <td>FEMALE</td>
      <td>bjjt8pjhuk</td>
    </tr>
    <tr>
      <th>3</th>
      <td>FEMALE</td>
      <td>lsw9q7uk0j</td>
    </tr>
    <tr>
      <th>4</th>
      <td>FEMALE</td>
      <td>0d01nltbrs</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_all = df_all.drop("gender" , axis=1)

df_all = pd.merge(df_all, gender, on="id", how="left")
```


```python
df_all.head()
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
      <th>country_destination</th>
      <th>id</th>
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>...</th>
      <th>signup_app_Android</th>
      <th>signup_app_Moweb</th>
      <th>signup_app_Web</th>
      <th>signup_app_iOS</th>
      <th>signup_method_basic</th>
      <th>signup_method_facebook</th>
      <th>signup_method_google</th>
      <th>signup_method_weibo</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NDF</td>
      <td>gxn3p5htnn</td>
      <td>0</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>35</td>
      <td>FEMALE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NDF</td>
      <td>820tgsjxq7</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
      <td>MALE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>4ft3gnwmtx</td>
      <td>3</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
      <td>FEMALE</td>
    </tr>
    <tr>
      <th>3</th>
      <td>other</td>
      <td>bjjt8pjhuk</td>
      <td>0</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
      <td>FEMALE</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>87mebub9p4</td>
      <td>0</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>45</td>
      <td>MALE</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 146 columns</p>
</div>




```python
df_all.gender.value_counts()
```




    FEMALE    169870
    MALE      105677
    Name: gender, dtype: int64



### sessions

### action, action_type, action_detail count


```python
def make_merged_sessions():
    
    sessions = df_sessions

    #session elapsed group by sum, mean
    ses = sessions.filter(items=('user_id', 'secs_elapsed'))
     
    print(" groupby aggregation in process...")

        
    ses_groupby_sum = ses.groupby("user_id").agg(np.sum)
    ses_groupby_mean = ses.groupby("user_id").agg(np.mean)
    
    #fill null data with mode value
    sessions["action"] = sessions["action"].fillna("show")
    sessions["action_type"] = sessions["action_type"].fillna("view")
    sessions["action_detail"] = sessions["action_detail"].fillna("view_search_results")
    
    id_groupby = sessions.groupby(sessions["user_id"]).agg(mode)
    
    device_type = []
    action = []
    action_type = []
    action_detail = []
    secs_elapsed = []

    print("id groupby in process...")
    
    for i in range(len(id_groupby.index)):
        device_type.append(id_groupby['device_type'][i][0])
        action.append(id_groupby['action'][i][0])
        action_type.append(id_groupby['action_type'][i][0])
        action_detail.append(id_groupby['action_detail'][i][0])
        secs_elapsed.append(id_groupby['secs_elapsed'][i][0])
    
    id_groupby_df = pd.DataFrame({"id":id_groupby.index ,
                                  "device_type":device_type ,
                                  "action":action,
                                  "action_type":action_type,
                                  "action_detail":action_detail,
                                  "secs_elapsed":secs_elapsed
                                  })
    
     
    print("merge sessions in process...")
    
    merge_ses_groupby = pd.merge(ses_groupby_sum, ses_groupby_mean, left_index=True, right_index=True, how="left")
    merge_ses_groupby = merge_ses_groupby.rename(columns={'secs_elapsed_x': 'secs_sum', 'secs_elapsed_y': 'secs_mean'})
    
    merged_sessions = pd.merge(id_groupby_df, merge_ses_groupby, left_on="id", right_index=True, how="left")
    
    merged_sessions['secs_elapsed'] = merged_sessions['secs_elapsed'].astype(float)
    
    merged_sessions['secs_mean'] = merged_sessions['secs_mean'].fillna(0)
    
    merged_sessions.to_csv("merged_sessions.csv", index=False)
```


```python
%%time
make_merged_sessions()
```

     groupby aggregation in process...
    id groupby in process...
    merge sessions in process...
    CPU times: user 4min 32s, sys: 4.14 s, total: 4min 36s
    Wall time: 4min 35s



```python
def remove_word():
    
    merged_sessions = pd.read_csv("merged_sessions.csv")

    def remove(word):
        word = re.sub("''", "", word)
        word = re.sub("\W", "", word)
        return word

    merged_sessions["action"] = merged_sessions["action"].apply(remove)
    merged_sessions["action_detail"] = merged_sessions["action_detail"].apply(remove)
    merged_sessions["action_type"] = merged_sessions["action_type"].apply(remove)
    merged_sessions["device_type"] = merged_sessions["device_type"].apply(remove)


    merged_sessions["action_detail"] = merged_sessions["action_detail"].replace({"['-unknown-']":"unknown"})
    merged_sessions["action_type"] = merged_sessions["action_type"].replace({"['-unknown-']":"unknown"})
    merged_sessions["device_type"] = merged_sessions["device_type"].replace({"['-unknown-']":"unknown", "['Android App Unknown Phone/Tablet']": "Androd_unkown_phone"})

    merged_sessions.to_csv("merged_sessions.csv", index=False)
```


```python
%%time
remove_word()
```

    CPU times: user 2.67 s, sys: 31.9 ms, total: 2.7 s
    Wall time: 2.72 s



```python
def sessions_detail_add():

    merged_sessions = pd.read_csv("merged_sessions.csv")
    sessions = df_sessions

    print("groupby count in process...")

    
    tmp = sessions.groupby(["user_id", "action_type"])["device_type"].count().unstack().fillna(0)
    sessions_at = pd.DataFrame(tmp)
    sessions_at.rename(columns = lambda x : "type__" + x, inplace = True)

    tmp = sessions.groupby(["user_id", "action"])["device_type"].count().unstack().fillna(0)
    sessions_a = pd.DataFrame(tmp)
    sessions_a.rename(columns = lambda x : "action__" + x, inplace = True)

    tmp = sessions.groupby(["user_id", "action_detail"])["device_type"].count().unstack().fillna(0)
    sessions_ad = pd.DataFrame(tmp)
    sessions_ad.rename(columns = lambda x : "detail__" + x, inplace = True)

    df_session_info = sessions_at.merge(sessions_a, how = "outer", left_index = True, right_index = True)
    df_session_info = df_session_info.merge(sessions_ad, how = "left", left_index = True, right_index = True)

    df_session_info.drop(["type__-unknown-", "detail__-unknown-"], axis = 1, inplace = True)
    df_session_info = df_session_info.fillna(0)

    print("merge sessions in process...")
    
    last_merged_sessions = pd.merge(merged_sessions, df_session_info, left_on='id', right_index=True, how='left')

    last_merged_sessions.to_csv("merged_sessions.csv", index=False)
```


```python
%%time
sessions_detail_add()
```

    groupby count in process...
    merge sessions in process...
    CPU times: user 1min 10s, sys: 5.69 s, total: 1min 16s
    Wall time: 1min 17s



```python
merged_sessions = pd.read_csv("merged_sessions.csv")
merged_sessions.head()
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
      <th>action</th>
      <th>action_detail</th>
      <th>action_type</th>
      <th>device_type</th>
      <th>id</th>
      <th>secs_elapsed</th>
      <th>secs_sum</th>
      <th>secs_mean</th>
      <th>type__booking_request</th>
      <th>type__booking_response</th>
      <th>...</th>
      <th>detail__view_resolutions</th>
      <th>detail__view_search_results</th>
      <th>detail__view_security_checks</th>
      <th>detail__view_user_real_names</th>
      <th>detail__wishlist</th>
      <th>detail__wishlist_content_update</th>
      <th>detail__wishlist_note</th>
      <th>detail__your_listings</th>
      <th>detail__your_reservations</th>
      <th>detail__your_trips</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>show</td>
      <td>view_search_results</td>
      <td>view</td>
      <td>MacDesktop</td>
      <td>00023iyk9l</td>
      <td>820.0</td>
      <td>867896.0</td>
      <td>22253.743590</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>show</td>
      <td>view_search_results</td>
      <td>view</td>
      <td>MacDesktop</td>
      <td>0010k6l0om</td>
      <td>125.0</td>
      <td>586543.0</td>
      <td>9460.370968</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>25.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>search</td>
      <td>view_search_results</td>
      <td>click</td>
      <td>AndroidAppUnknownPhoneTablet</td>
      <td>001wyh0pz8</td>
      <td>623.0</td>
      <td>282965.0</td>
      <td>3179.382022</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>71.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>show</td>
      <td>p3</td>
      <td>view</td>
      <td>unknown</td>
      <td>0028jgx1x1</td>
      <td>3.0</td>
      <td>297010.0</td>
      <td>9900.333333</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>show</td>
      <td>view_search_results</td>
      <td>view</td>
      <td>iPhone</td>
      <td>002qnbzfs5</td>
      <td>1.0</td>
      <td>6487080.0</td>
      <td>8232.335025</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>202.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 530 columns</p>
</div>




```python
merged_sessions.shape
```




    (135483, 530)




```python
gc.collect()
```




    141




```python
df_all = pd.merge(df_all, merged_sessions, on="id", how="left")
df_all.head()
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
      <th>country_destination</th>
      <th>id</th>
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>...</th>
      <th>detail__view_resolutions</th>
      <th>detail__view_search_results</th>
      <th>detail__view_security_checks</th>
      <th>detail__view_user_real_names</th>
      <th>detail__wishlist</th>
      <th>detail__wishlist_content_update</th>
      <th>detail__wishlist_note</th>
      <th>detail__your_listings</th>
      <th>detail__your_reservations</th>
      <th>detail__your_trips</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NDF</td>
      <td>gxn3p5htnn</td>
      <td>0</td>
      <td>2010</td>
      <td>6</td>
      <td>28</td>
      <td>2009</td>
      <td>3</td>
      <td>19</td>
      <td>466</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NDF</td>
      <td>820tgsjxq7</td>
      <td>0</td>
      <td>2011</td>
      <td>5</td>
      <td>25</td>
      <td>2009</td>
      <td>5</td>
      <td>23</td>
      <td>732</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>4ft3gnwmtx</td>
      <td>3</td>
      <td>2010</td>
      <td>9</td>
      <td>28</td>
      <td>2009</td>
      <td>6</td>
      <td>9</td>
      <td>476</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>other</td>
      <td>bjjt8pjhuk</td>
      <td>0</td>
      <td>2011</td>
      <td>12</td>
      <td>5</td>
      <td>2009</td>
      <td>10</td>
      <td>31</td>
      <td>765</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>87mebub9p4</td>
      <td>0</td>
      <td>2010</td>
      <td>9</td>
      <td>14</td>
      <td>2009</td>
      <td>12</td>
      <td>8</td>
      <td>280</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 675 columns</p>
</div>




```python
df_all.shape
```




    (275547, 675)




```python
gc.collect()
```




    91



### one hot encoding


```python
target = df_all.country_destination[:213451]
```


```python
df_all = df_all.drop(["country_destination", "id"], axis=1)
```


```python
df_all = pd.get_dummies(df_all)
```


```python
gc.collect()
```




    123



### fill null data with imputer


```python
%%time
## impute the missing value using median
from sklearn.preprocessing import Imputer

## impute the missing value using median
impute_list = df_all.columns.tolist()
# impute_list.remove("id")
# impute_list.remove("country_destination")

imp = Imputer(missing_values='NaN', strategy='median', axis=0)

df_all[impute_list] = imp.fit_transform(df_all[impute_list])
# test[impute_list] = imp.fit_transform(test[impute_list])

gc.collect()
```

    CPU times: user 1min 28s, sys: 22.7 s, total: 1min 51s
    Wall time: 1min 51s


### split train / test


```python
train = df_all[:213451]
test = df_all[213451:]
```


```python
train.head()
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
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
      <th>holiday</th>
      <th>...</th>
      <th>device_type_LinuxDesktop</th>
      <th>device_type_MacDesktop</th>
      <th>device_type_OperaPhone</th>
      <th>device_type_Tablet</th>
      <th>device_type_WindowsDesktop</th>
      <th>device_type_WindowsPhone</th>
      <th>device_type_iPadTablet</th>
      <th>device_type_iPhone</th>
      <th>device_type_iPodtouch</th>
      <th>device_type_unknown</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>2010.0</td>
      <td>6.0</td>
      <td>28.0</td>
      <td>2009.0</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>466.0</td>
      <td>9.703511</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>2011.0</td>
      <td>5.0</td>
      <td>25.0</td>
      <td>2009.0</td>
      <td>5.0</td>
      <td>23.0</td>
      <td>732.0</td>
      <td>11.068028</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
      <td>2010.0</td>
      <td>9.0</td>
      <td>28.0</td>
      <td>2009.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>476.0</td>
      <td>11.333404</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>2011.0</td>
      <td>12.0</td>
      <td>5.0</td>
      <td>2009.0</td>
      <td>10.0</td>
      <td>31.0</td>
      <td>765.0</td>
      <td>9.984560</td>
      <td>1.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>2010.0</td>
      <td>9.0</td>
      <td>14.0</td>
      <td>2009.0</td>
      <td>12.0</td>
      <td>8.0</td>
      <td>280.0</td>
      <td>10.010771</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 953 columns</p>
</div>




```python
train.isnull().sum()
```




    signup_flow                                 0
    create_year                                 0
    create_month                                0
    create_day                                  0
    active_year                                 0
    active_month                                0
    active_day                                  0
    lag_days                                    0
    lag_seconds                                 0
    holiday                                     0
    weekend                                     0
    affiliate_channel_api                       0
    affiliate_channel_content                   0
    affiliate_channel_direct                    0
    affiliate_channel_other                     0
    affiliate_channel_remarketing               0
    affiliate_channel_sem-brand                 0
    affiliate_channel_sem-non-brand             0
    affiliate_channel_seo                       0
    affiliate_provider_baidu                    0
    affiliate_provider_bing                     0
    affiliate_provider_craigslist               0
    affiliate_provider_daum                     0
    affiliate_provider_direct                   0
    affiliate_provider_email-marketing          0
    affiliate_provider_facebook                 0
    affiliate_provider_facebook-open-graph      0
    affiliate_provider_google                   0
    affiliate_provider_gsp                      0
    affiliate_provider_meetup                   0
                                               ..
    action_detail_view_reservations             0
    action_detail_view_search_results           0
    action_detail_wishlist                      0
    action_detail_wishlist_content_update       0
    action_detail_your_listings                 0
    action_detail_your_reservations             0
    action_detail_your_trips                    0
    action_type_booking_request                 0
    action_type_click                           0
    action_type_data                            0
    action_type_message_post                    0
    action_type_modify                          0
    action_type_partner_callback                0
    action_type_submit                          0
    action_type_unknown                         0
    action_type_view                            0
    device_type_AndroidAppUnknownPhoneTablet    0
    device_type_AndroidPhone                    0
    device_type_Blackberry                      0
    device_type_Chromebook                      0
    device_type_LinuxDesktop                    0
    device_type_MacDesktop                      0
    device_type_OperaPhone                      0
    device_type_Tablet                          0
    device_type_WindowsDesktop                  0
    device_type_WindowsPhone                    0
    device_type_iPadTablet                      0
    device_type_iPhone                          0
    device_type_iPodtouch                       0
    device_type_unknown                         0
    Length: 953, dtype: int64




```python
test.head()
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
      <th>signup_flow</th>
      <th>create_year</th>
      <th>create_month</th>
      <th>create_day</th>
      <th>active_year</th>
      <th>active_month</th>
      <th>active_day</th>
      <th>lag_days</th>
      <th>lag_seconds</th>
      <th>holiday</th>
      <th>...</th>
      <th>device_type_LinuxDesktop</th>
      <th>device_type_MacDesktop</th>
      <th>device_type_OperaPhone</th>
      <th>device_type_Tablet</th>
      <th>device_type_WindowsDesktop</th>
      <th>device_type_WindowsPhone</th>
      <th>device_type_iPadTablet</th>
      <th>device_type_iPhone</th>
      <th>device_type_iPodtouch</th>
      <th>device_type_unknown</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>213451</th>
      <td>0.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.791759</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>213452</th>
      <td>0.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.931826</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>213453</th>
      <td>0.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>4.682131</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>213454</th>
      <td>0.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>4.905275</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>213455</th>
      <td>0.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>2014.0</td>
      <td>7.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>5.220356</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 953 columns</p>
</div>




```python
test.isnull().sum()
```




    signup_flow                                 0
    create_year                                 0
    create_month                                0
    create_day                                  0
    active_year                                 0
    active_month                                0
    active_day                                  0
    lag_days                                    0
    lag_seconds                                 0
    holiday                                     0
    weekend                                     0
    affiliate_channel_api                       0
    affiliate_channel_content                   0
    affiliate_channel_direct                    0
    affiliate_channel_other                     0
    affiliate_channel_remarketing               0
    affiliate_channel_sem-brand                 0
    affiliate_channel_sem-non-brand             0
    affiliate_channel_seo                       0
    affiliate_provider_baidu                    0
    affiliate_provider_bing                     0
    affiliate_provider_craigslist               0
    affiliate_provider_daum                     0
    affiliate_provider_direct                   0
    affiliate_provider_email-marketing          0
    affiliate_provider_facebook                 0
    affiliate_provider_facebook-open-graph      0
    affiliate_provider_google                   0
    affiliate_provider_gsp                      0
    affiliate_provider_meetup                   0
                                               ..
    action_detail_view_reservations             0
    action_detail_view_search_results           0
    action_detail_wishlist                      0
    action_detail_wishlist_content_update       0
    action_detail_your_listings                 0
    action_detail_your_reservations             0
    action_detail_your_trips                    0
    action_type_booking_request                 0
    action_type_click                           0
    action_type_data                            0
    action_type_message_post                    0
    action_type_modify                          0
    action_type_partner_callback                0
    action_type_submit                          0
    action_type_unknown                         0
    action_type_view                            0
    device_type_AndroidAppUnknownPhoneTablet    0
    device_type_AndroidPhone                    0
    device_type_Blackberry                      0
    device_type_Chromebook                      0
    device_type_LinuxDesktop                    0
    device_type_MacDesktop                      0
    device_type_OperaPhone                      0
    device_type_Tablet                          0
    device_type_WindowsDesktop                  0
    device_type_WindowsPhone                    0
    device_type_iPadTablet                      0
    device_type_iPhone                          0
    device_type_iPodtouch                       0
    device_type_unknown                         0
    Length: 953, dtype: int64




```python
train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 213451 entries, 0 to 213450
    Columns: 953 entries, signup_flow to device_type_unknown
    dtypes: float64(953)
    memory usage: 1.5 GB



```python
test.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 62096 entries, 213451 to 275546
    Columns: 953 entries, signup_flow to device_type_unknown
    dtypes: float64(953)
    memory usage: 452.0 MB



```python
train.shape, test.shape, target.shape
```




    ((213451, 953), (62096, 953), (213451,))




```python
gc.collect()
```




    14



### modeling


```python
def submit_kaggle(df_train, df_test, target, reg_alpha, reg_lambda, learning_rate, n_estimators):
    
    le = LabelEncoder()

    y_train = le.fit_transform(target)
 
    model = lgb.LGBMClassifier(boosting_type= 'gbdt',nthread=3, n_jobs=-1, reg_alpha=reg_alpha, reg_lambda=reg_lambda, max_depth=-1, learning_rate=learning_rate, n_estimators=n_estimators)

    print("cross validation started ...")
    #negative cross entropy
    print(np.mean(cross_val_score(model, df_train, y_train, n_jobs=-1, )))
    print()
    print("model fitting starting ...")
    
    model = model.fit(df_train, y_train)
       
    print("model fitting completed ...")
    print()
    
    predic_proba = model.predict_proba(df_test)
    
    df_submit = pd.DataFrame(columns=["id", "country"])
    ids = []
    cts = []
    for i in range(len(test_id)):
        idx = test_id.iloc[i]
        ids += [idx] * 5
        cts += le.inverse_transform(np.argsort(predic_proba[i])[::-1])[:5].tolist()
        
    df_submit["id"] = ids
    df_submit["country"] = cts
    df_submit.to_csv('submission.csv', index = False)
    print("kaggle submission in process ...")
    ! kaggle competitions submit -c airbnb-recruiting-new-user-bookings -f submission.csv -m "Message"
    
    gc.collect()
    
    return model, df_train, df_test, target
```


```python
%%time
model, df_train, df_test, target = submit_kaggle(train, test, target, 
                                                      reg_alpha=1, 
                                                      reg_lambda=0, 
                                                      learning_rate=0.05, 
                                                      n_estimators=400)
```

    cross validation started ...
    0.26659212543108246
    
    model fitting starting ...
    model fitting completed ...
    
    kaggle submission in process ...
    Warning: Your Kaggle API key is readable by other users on this system! To fix this, you can run 'chmod 600 /home/jk/.kaggle/kaggle.json'
    Successfully submitted to Airbnb New User BookingsCPU times: user 11min 19s, sys: 8.83 s, total: 11min 27s
    Wall time: 11min 7s


### feature importance


```python
print("feature importance mean :", round(np.mean(model.feature_importances_), 2))
print("feature importance median :", round(np.median(model.feature_importances_), 2))

lgb.plot_importance(model, figsize=(50, 50), max_num_features=50)
plt.show()
```

    feature importance mean : 148.07
    feature importance median : 0.0


### feature selection by feature importance


```python
model1 = SelectFromModel(model, prefit=True, threshold=1)
X_train = model1.transform(df_train)
X_test = model1.transform(df_test)

X_train.shape, X_test.shape, target.shape
```




    ((213451, 451), (62096, 451), (213451,))




```python
%%time
model, df_train, df_test, target = submit_kaggle(X_train, X_test, target, 
                                                      reg_alpha=1, 
                                                      reg_lambda=0, 
                                                      learning_rate=0.1, 
                                                      n_estimators=100)
```

    cross validation started ...



```python
print("feature importance mean :", round(np.mean(model.feature_importances_), 2))
print("feature importance median :", round(np.median(model.feature_importances_), 2))

lgb.plot_importance(model, figsize=(50, 50), max_num_features=50)
plt.show()
```


```python
#0.87814 #top22% #326/1462
```
