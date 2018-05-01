---
layout: post
title: Kaggle Ne York Taxi Trip Duration
tags: [Project]

---

### Kaggle : New York City Taxi Trip Duration

![alt text](/assets/img/taxi.png)

# 1 EDA (Exploratory Data Analysis)

# purpose of  EDA

- Suggest hypotheses about the causes of observed phenomena
- Assess assumptions on which statistical inference will be based
- Support the selection of appropriate statistical tools and techniques
- Provide a basis for further data collection through surveys or experiments

# EDA methods
- Graphical techniques used in EDA are:
    - boxplot
        - detailed feature (datetime by month, day of week, hours)
    - historgram or barplot (distribution) # bin = range of value
        - origin feature (pick lat,long, drop lat, long, duration, passenger count, flag)
        - detailed feature (datetime by month, day of week, hours)
    - scatter plot
        - duration vs distance = to check odd data
    - Parallel Coordinates vs Colormaps vs Andrews curves charts
    - odd ratio????

- Quantative methods:
    - Trimean == tukey method?

# 1.1 Understanding data


```python
from IPython.display import display
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from math import sin, cos, sqrt, atan2, radians
import seaborn as sns
import lightgbm as lgb
from sklearn.decomposition import PCA
from sklearn.cluster import DBSCAN
from sklearn.cluster import SpectralClustering
from sklearn.cluster import MeanShift
from sklearn.cluster import MiniBatchKMeans
from sklearn.model_selection import train_test_split

from scipy import stats
import statsmodels.api as sm
import statsmodels.formula.api as smf
import statsmodels.stats.api as sms
from scipy.stats import norm, skew
from statsmodels.stats.outliers_influence import variance_inflation_factor

from sklearn import metrics
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import Normalizer

%matplotlib inline

import warnings
warnings.filterwarnings("ignore")
```

    /home/jk/anaconda3/lib/python3.6/site-packages/statsmodels/compat/pandas.py:56: FutureWarning: The pandas.core.datetools module is deprecated and will be removed in a future version. Please use the pandas.tseries module instead.
      from pandas.core import datetools



```python
train = pd.read_csv("train.csv")
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




```python
test = pd.read_csv("test.csv")
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
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id3004672</td>
      <td>1</td>
      <td>2016-06-30 23:59:58</td>
      <td>1</td>
      <td>-73.988129</td>
      <td>40.732029</td>
      <td>-73.990173</td>
      <td>40.756680</td>
      <td>N</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id3505355</td>
      <td>1</td>
      <td>2016-06-30 23:59:53</td>
      <td>1</td>
      <td>-73.964203</td>
      <td>40.679993</td>
      <td>-73.959808</td>
      <td>40.655403</td>
      <td>N</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id1217141</td>
      <td>1</td>
      <td>2016-06-30 23:59:47</td>
      <td>1</td>
      <td>-73.997437</td>
      <td>40.737583</td>
      <td>-73.986160</td>
      <td>40.729523</td>
      <td>N</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id2150126</td>
      <td>2</td>
      <td>2016-06-30 23:59:41</td>
      <td>1</td>
      <td>-73.956070</td>
      <td>40.771900</td>
      <td>-73.986427</td>
      <td>40.730469</td>
      <td>N</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id1598245</td>
      <td>1</td>
      <td>2016-06-30 23:59:33</td>
      <td>1</td>
      <td>-73.970215</td>
      <td>40.761475</td>
      <td>-73.961510</td>
      <td>40.755890</td>
      <td>N</td>
    </tr>
  </tbody>
</table>
</div>




```python
sample_submission = pd.read_csv("sample_submission.csv")
sample_submission.head()
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
      <th>trip_duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id3004672</td>
      <td>959</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id3505355</td>
      <td>959</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id1217141</td>
      <td>959</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id2150126</td>
      <td>959</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id1598245</td>
      <td>959</td>
    </tr>
  </tbody>
</table>
</div>



# 1.1.a Data type and unit

# unit

### 1. latitude / longtitude = decimal degree
- 111.32mm per 0.000001° / 11.132 m per 0.0001° / 1.1132 km per 0.01° / 111.32 km per 1.0°
- 14 demical degree
- ex) 40.767937 , -73.982155

### 2. datetime = year-month-day: hour-minute-second

### 3. vendor_id = 1, 2

### 4. passenger_count = 0 - 9

### 4. store_and_fwd_flag = N, Y

### 6. duration = second
- ex) 455 sec = 7min 35sec



```python
train.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1458644 entries, 0 to 1458643
    Data columns (total 11 columns):
    id                    1458644 non-null object
    vendor_id             1458644 non-null int64
    pickup_datetime       1458644 non-null object
    dropoff_datetime      1458644 non-null object
    passenger_count       1458644 non-null int64
    pickup_longitude      1458644 non-null float64
    pickup_latitude       1458644 non-null float64
    dropoff_longitude     1458644 non-null float64
    dropoff_latitude      1458644 non-null float64
    store_and_fwd_flag    1458644 non-null object
    trip_duration         1458644 non-null int64
    dtypes: float64(4), int64(3), object(4)
    memory usage: 122.4+ MB



```python
test.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 625134 entries, 0 to 625133
    Data columns (total 9 columns):
    id                    625134 non-null object
    vendor_id             625134 non-null int64
    pickup_datetime       625134 non-null object
    passenger_count       625134 non-null int64
    pickup_longitude      625134 non-null float64
    pickup_latitude       625134 non-null float64
    dropoff_longitude     625134 non-null float64
    dropoff_latitude      625134 non-null float64
    store_and_fwd_flag    625134 non-null object
    dtypes: float64(4), int64(2), object(3)
    memory usage: 42.9+ MB


# train data
-  1.4M data, 11 columns

# test data
-  0.6M data, 9 columns (no dropoff_datetime, trip_duration)

# 1.1.b Missing Data check


```python
#none of missing data
train.isnull().sum()
```




    id                    0
    vendor_id             0
    pickup_datetime       0
    dropoff_datetime      0
    passenger_count       0
    pickup_longitude      0
    pickup_latitude       0
    dropoff_longitude     0
    dropoff_latitude      0
    store_and_fwd_flag    0
    trip_duration         0
    dtype: int64




```python
test.isnull().sum()
```




    id                    0
    vendor_id             0
    pickup_datetime       0
    passenger_count       0
    pickup_longitude      0
    pickup_latitude       0
    dropoff_longitude     0
    dropoff_latitude      0
    store_and_fwd_flag    0
    dtype: int64



# 1.1.c Trip duration

# trip duration calculation validation


```python
train["pickup_datetime"] =  pd.to_datetime(train["pickup_datetime"])
train["dropoff_datetime"] =  pd.to_datetime(train["dropoff_datetime"])
sample_duration = train["dropoff_datetime"] - train["pickup_datetime"]
sample_duration_sec = sample_duration.dt.total_seconds().astype('int')
train['trip_sec'] =  sample_duration_sec
```


```python
train_d = train[train["trip_duration"] != train["trip_sec"]]
print(len(train_d))

if len(train_d) == 0:
    train = train.drop(['trip_sec'], axis=1)
```

    0


# drop odd data


```python
#drop 100,000 duration time data
print(len(train.loc[train.trip_duration > 100000]))
```

    4



```python
print("before drop : ", len(train))
train = train[train["trip_duration"] < 100000]
print("after  drop : ", len(train))
```

    before drop :  1458644
    after  drop :  1458640


# trip duration visualization


```python
#logigramatic trip duration data
plt.figure(figsize=(16,8))
plt.subplot(121)
sns.distplot(np.log(train["trip_duration"]))
plt.subplot(122)
stats.probplot(np.log(train["trip_duration"]), plot=plt)
plt.tight_layout()
plt.show()
```


![png](taxi_V5.0_files/taxi_V5.0_26_0.png)


# 1.2 Feature Engineering & Data Cleaning

# date time convert


```python
train = train.drop("dropoff_datetime", axis=1)
```


```python
#data type convert to datetime from object
train["pickup_datetime"] =  pd.to_datetime(train["pickup_datetime"])
test["pickup_datetime"] =  pd.to_datetime(test["pickup_datetime"])
```


```python
#day of week
#Monday=0, Sunday=6
train["pick_dayofweek"] = train["pickup_datetime"].dt.dayofweek
test["pick_dayofweek"] = test["pickup_datetime"].dt.dayofweek
```


```python
train["pick_month"] = train["pickup_datetime"].apply(lambda x : x.month)
train["pick_day"] = train["pickup_datetime"].apply(lambda x : x.day)
train["pick_hour"] = train["pickup_datetime"].apply(lambda x : x.hour)
# train["pick_min"] = train["pickup_datetime"].apply(lambda x : x.minute)
# train["pick_sec"] = train["pickup_datetime"].apply(lambda x : x.second)

test["pick_month"] = test["pickup_datetime"].apply(lambda x : x.month)
test["pick_day"] = test["pickup_datetime"].apply(lambda x : x.day)
test["pick_hour"] = test["pickup_datetime"].apply(lambda x : x.hour)
# test["pick_min"] = test["pickup_datetime"].apply(lambda x : x.minute)
# test["pick_sec"] = test["pickup_datetime"].apply(lambda x : x.second)
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
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
      <th>trip_duration</th>
      <th>pick_dayofweek</th>
      <th>pick_month</th>
      <th>pick_day</th>
      <th>pick_hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id2875421</td>
      <td>2</td>
      <td>2016-03-14 17:24:55</td>
      <td>1</td>
      <td>-73.982155</td>
      <td>40.767937</td>
      <td>-73.964630</td>
      <td>40.765602</td>
      <td>N</td>
      <td>455</td>
      <td>0</td>
      <td>3</td>
      <td>14</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id2377394</td>
      <td>1</td>
      <td>2016-06-12 00:43:35</td>
      <td>1</td>
      <td>-73.980415</td>
      <td>40.738564</td>
      <td>-73.999481</td>
      <td>40.731152</td>
      <td>N</td>
      <td>663</td>
      <td>6</td>
      <td>6</td>
      <td>12</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id3858529</td>
      <td>2</td>
      <td>2016-01-19 11:35:24</td>
      <td>1</td>
      <td>-73.979027</td>
      <td>40.763939</td>
      <td>-74.005333</td>
      <td>40.710087</td>
      <td>N</td>
      <td>2124</td>
      <td>1</td>
      <td>1</td>
      <td>19</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id3504673</td>
      <td>2</td>
      <td>2016-04-06 19:32:31</td>
      <td>1</td>
      <td>-74.010040</td>
      <td>40.719971</td>
      <td>-74.012268</td>
      <td>40.706718</td>
      <td>N</td>
      <td>429</td>
      <td>2</td>
      <td>4</td>
      <td>6</td>
      <td>19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id2181028</td>
      <td>2</td>
      <td>2016-03-26 13:30:55</td>
      <td>1</td>
      <td>-73.973053</td>
      <td>40.793209</td>
      <td>-73.972923</td>
      <td>40.782520</td>
      <td>N</td>
      <td>435</td>
      <td>5</td>
      <td>3</td>
      <td>26</td>
      <td>13</td>
    </tr>
  </tbody>
</table>
</div>



# national holiday


```python
from bs4 import BeautifulSoup
import requests
import urllib.request
```


```python
#wrapper > div:nth-child(3) > div.twelve.columns > table.list-table > tbody > tr:nth-child(2) > td:nth-child(2)
```


```python
df = pd.DataFrame(columns=["rank","keyword"])
response = requests.get("https://www.officeholidays.com/countries/usa/2016.php")
bs = BeautifulSoup(response.content, "html.parser")
trs = bs.select("table td")
trs1 = trs[1::5]
```


```python
li = []
holi = pd.DataFrame()
count = 0

for i in trs1[0:14]:
    li.append((i.text).strip())
    li[count] = '2016 ' + li[count]
    li[count] = li[count].split(" ")
    li[count] = li[count][0] + "-" + li[count][1] + '-' + li[count][2]
    count += 1

holi['date'] = li
holi['date'] = pd.to_datetime(holi['date'])
holi
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
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-02-15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-04-15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-05-08</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-05-30</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2016-06-19</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016-07-04</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-09-05</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2016-10-10</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2016-11-11</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2016-11-24</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2016-11-25</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2016-12-26</td>
    </tr>
  </tbody>
</table>
</div>




```python
select_date = list(holi["date"].astype("str"))
holiday = train.pickup_datetime.apply(lambda x : str(x.date())).isin(select_date)
train["holiday"] = holiday
```


```python
select_date = list(holi["date"].astype("str"))
holiday = test.pickup_datetime.apply(lambda x : str(x.date())).isin(select_date)
test["holiday"] = holiday
```


```python
train['holiday'] = 1 * (train.holiday == True)
test['holiday'] = 1 * (test.holiday == True)
```

# New York City Weather Event


```python
from selenium import webdriver
```


```python
driver =  webdriver.PhantomJS()    
driver.get("https://www.weather.gov/okx/stormevents")
```


```python
date = driver.find_elements_by_css_selector('#pagebody > div:nth-child(3) > div > table > tbody > tr > td ul:nth-child(6)')
lis = date[0].find_elements_by_css_selector('li')
```


```python
li_wea = []
count = 0

rows=[]
for i in lis:
    li_wea.append(i.text)
    li_wea[count] = '2016 ' + li_wea[count]
    li_wea[count] = li_wea[count].split(" ")

    li_wea[count] = li_wea[count][0] + "-" + li_wea[count][1] + "-" + li_wea[count][2] + "-" + li_wea[count][3]
#     rows.append([li_wea[count][0], li_wea[count][1], li_wea[count][2] ,li_wea[count][3]])
    count += 1


#rows
```


```python
new1 = pd.DataFrame(li_wea, columns=['old'])
new1['date'] = new1['old'].str.extract('(\d\d\d\d-...-\d\d)', expand=True)
```


```python
new1['date'][4] = '2016-Feb-05'
new1['date'][5] = '2016-Feb-08'
new1['date'][11] = '2016-Apr-03'
new1['date'][12] = '2016-Apr-04'
new1['date'][14] = '2016-June-28'
new1['date'][15] = '2016-July-18'
new1['date'][16] = '2016-July-29'
new1['date'][17] = '2016-July-31'
new1['date'][25] = '2016-Oct-08'
new1['date'][35] = '2016-Dec-05'
new1 = new1.drop('old', axis=1)
```


```python
new2 = pd.DataFrame(['2016-August-01', '2016-Dec-01'], columns=['date'])
new1 = new1.append(new2, ignore_index=True).dropna()
new1['date'] = pd.to_datetime(new1['date'])
new1
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
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2016-01-23</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2016-02-05</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2016-02-08</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2016-02-15</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2016-02-24</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2016-03-14</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2016-03-21</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2016-03-28</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2016-04-03</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2016-04-04</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2016-05-30</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2016-06-28</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2016-07-18</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2016-07-29</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2016-07-31</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2016-08-10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2016-08-11</td>
    </tr>
    <tr>
      <th>20</th>
      <td>2016-08-12</td>
    </tr>
    <tr>
      <th>21</th>
      <td>2016-08-13</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2016-08-20</td>
    </tr>
    <tr>
      <th>24</th>
      <td>2016-09-19</td>
    </tr>
    <tr>
      <th>25</th>
      <td>2016-10-08</td>
    </tr>
    <tr>
      <th>26</th>
      <td>2016-10-22</td>
    </tr>
    <tr>
      <th>27</th>
      <td>2016-10-22</td>
    </tr>
    <tr>
      <th>28</th>
      <td>2016-10-27</td>
    </tr>
    <tr>
      <th>29</th>
      <td>2016-10-30</td>
    </tr>
    <tr>
      <th>30</th>
      <td>2016-11-11</td>
    </tr>
    <tr>
      <th>31</th>
      <td>2016-11-14</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2016-11-20</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2016-11-29</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2016-11-30</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2016-12-05</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2016-12-15</td>
    </tr>
    <tr>
      <th>37</th>
      <td>2016-12-17</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2016-12-18</td>
    </tr>
    <tr>
      <th>39</th>
      <td>2016-08-01</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2016-12-01</td>
    </tr>
  </tbody>
</table>
</div>




```python
select_date = list(new1["date"].astype("str"))
weather = train.pickup_datetime.apply(lambda x : str(x.date())).isin(select_date)
train["weather"] = weather
```


```python
select_date = list(new1["date"].astype("str"))
weather = test.pickup_datetime.apply(lambda x : str(x.date())).isin(select_date)
test["weather"] = weather
```


```python
train['weather'] = 1 * (train.weather == True)
test['weather'] = 1 * (test.weather == True)
```


```python
driver.close()
```

# 1.2.b Distance between pickup and dropoff location

# uclidean Distance


```python
def uclidean(lat1, lng1, lat2, lng2):
    lat1, lng1, lat2, lng2 = map(np.radians, (lat1, lng1, lat2, lng2))
    R = 6371.0
    lat = lat2 - lat1
    lng = lng2 - lng1
    d = np.sin(lat * 0.5) ** 2 + np.cos(lat1) * np.cos(lat2) * np.sin(lng * 0.5) ** 2
    h = 2 * R * np.arcsin(np.sqrt(d))
    return h
```


```python
train['uclidean'] = uclidean(train.pickup_latitude, train.pickup_longitude, train.dropoff_latitude, train.dropoff_longitude)
test['uclidean'] = uclidean(test.pickup_latitude, test.pickup_longitude, test.dropoff_latitude, test.dropoff_longitude)
```

# manhatan distance


```python
train['manhatan'] = (abs(train.dropoff_longitude - train.pickup_longitude) + abs(train.dropoff_latitude - train.pickup_latitude)) * 113.2
test['manhatan'] = (abs(test.dropoff_longitude - test.pickup_longitude) + abs(test.dropoff_latitude - test.pickup_latitude)) * 113.2
```

## direction


```python
def direction(pickup_lat, pickup_long, dropoff_lat, dropoff_long):

    pickup_lat_rads = np.radians(pickup_lat)
    pickup_long_rads = np.radians(pickup_long)
    dropoff_lat_rads = np.radians(dropoff_lat)
    dropoff_long_rads = np.radians(dropoff_long)
    long_delta_rads = np.radians(dropoff_long_rads - pickup_long_rads)

    y = np.sin(long_delta_rads) * np.cos(dropoff_lat_rads)
    x = (np.cos(pickup_lat_rads) * np.sin(dropoff_lat_rads) - np.sin(pickup_lat_rads) * np.cos(dropoff_lat_rads) * np.cos(long_delta_rads))

    return np.degrees(np.arctan2(y, x))
```


```python
train['direction'] = direction(train.pickup_latitude, train.pickup_longitude, train.dropoff_latitude, train.dropoff_longitude)
test['direction'] = direction(test.pickup_latitude, test.pickup_longitude, test.dropoff_latitude, test.dropoff_longitude)
```

# 1.2.d.2 Spatial Data Analysis

### Types of spatial analysis
- FA(factor analysis)
    - Euclidean metric = > PCA(principal component analysis)
    - Chi-Square distance => Correspondence Analysis (similar to PCA, but better for categrorical data)
    - Generalized Mahalanobis distance => Discriminant Analysis

### stack-up coordinates data


```python
coord_pick_lat = pd.concat([train['pickup_latitude'], test['pickup_latitude']], axis=0)
coord_pick_lon = pd.concat([train['pickup_longitude'], test['pickup_longitude']], axis=0)
coord_drop_lat = pd.concat([train['dropoff_latitude'], test['dropoff_latitude']], axis=0)
coord_drop_lon = pd.concat([train['dropoff_longitude'], test['dropoff_longitude']], axis=0)

coord_pick = pd.concat([coord_pick_lat, coord_pick_lon], axis=1)
coord_drop = pd.concat([coord_drop_lat, coord_drop_lon], axis=1)

coord_lat = pd.concat([train['pickup_latitude'], train['dropoff_latitude'], test['pickup_latitude'], test['dropoff_latitude']], axis=0)
coord_lon = pd.concat([train['pickup_longitude'], train['dropoff_longitude'], test['pickup_longitude'], test['dropoff_longitude']], axis=0)
coord_all = pd.concat([coord_lat, coord_lon], axis=1)
coord_all.columns = ['lat', 'lon']
```

# coordinates scatter plot


```python
# new york city coordinate = (41.145495, −73.994901)
city_lon_border = (-74.03, -73.75)
city_lat_border = (40.63, 40.85)
```


```python
sns.lmplot(x='pickup_latitude', y='pickup_longitude', data=coord_pick, fit_reg=False, scatter_kws={"s": 1}, size=10)
plt.ylim(city_lon_border)
plt.xlim(city_lat_border)
plt.title('Pick up')
plt.show()
```


![png](taxi_V5.0_files/taxi_V5.0_68_0.png)



```python
sns.lmplot(x='dropoff_latitude', y='dropoff_longitude', data=coord_drop, fit_reg=False, scatter_kws={"s": 1}, size=10)
plt.ylim(city_lon_border)
plt.xlim(city_lat_border)
plt.title('Drop off')
plt.show()
```


![png](taxi_V5.0_files/taxi_V5.0_69_0.png)


# PCA


```python
pca = PCA(random_state=0).fit(coord_all)
```


```python
#PCA
train['pick_pca0'] = pca.transform(train[['pickup_latitude', 'pickup_longitude']])[:, 0]
train['pick_pca1'] = pca.transform(train[['pickup_latitude', 'pickup_longitude']])[:, 1]
train['drop_pca0'] = pca.transform(train[['dropoff_latitude', 'dropoff_longitude']])[:, 0]
train['drop_pca1'] = pca.transform(train[['dropoff_latitude', 'dropoff_longitude']])[:, 1]
test['pick_pca0'] = pca.transform(test[['pickup_latitude', 'pickup_longitude']])[:, 0]
test['pick_pca1'] = pca.transform(test[['pickup_latitude', 'pickup_longitude']])[:, 1]
test['drop_pca0'] = pca.transform(test[['dropoff_latitude', 'dropoff_longitude']])[:, 0]
test['drop_pca1'] = pca.transform(test[['dropoff_latitude', 'dropoff_longitude']])[:, 1]
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
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
      <th>trip_duration</th>
      <th>...</th>
      <th>pick_hour</th>
      <th>holiday</th>
      <th>weather</th>
      <th>uclidean</th>
      <th>manhatan</th>
      <th>direction</th>
      <th>pick_pca0</th>
      <th>pick_pca1</th>
      <th>drop_pca0</th>
      <th>drop_pca1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id2875421</td>
      <td>2</td>
      <td>2016-03-14 17:24:55</td>
      <td>1</td>
      <td>-73.982155</td>
      <td>40.767937</td>
      <td>-73.964630</td>
      <td>40.765602</td>
      <td>N</td>
      <td>455</td>
      <td>...</td>
      <td>17</td>
      <td>0</td>
      <td>1</td>
      <td>1.498521</td>
      <td>2.248074</td>
      <td>174.333195</td>
      <td>0.007691</td>
      <td>0.017053</td>
      <td>-0.009667</td>
      <td>0.013695</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id2377394</td>
      <td>1</td>
      <td>2016-06-12 00:43:35</td>
      <td>1</td>
      <td>-73.980415</td>
      <td>40.738564</td>
      <td>-73.999481</td>
      <td>40.731152</td>
      <td>N</td>
      <td>663</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1.805507</td>
      <td>2.997289</td>
      <td>-178.051506</td>
      <td>0.007677</td>
      <td>-0.012371</td>
      <td>0.027145</td>
      <td>-0.018652</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id3858529</td>
      <td>2</td>
      <td>2016-01-19 11:35:24</td>
      <td>1</td>
      <td>-73.979027</td>
      <td>40.763939</td>
      <td>-74.005333</td>
      <td>40.710087</td>
      <td>N</td>
      <td>2124</td>
      <td>...</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>6.385098</td>
      <td>9.073912</td>
      <td>-179.629721</td>
      <td>0.004803</td>
      <td>0.012879</td>
      <td>0.034222</td>
      <td>-0.039337</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id3504673</td>
      <td>2</td>
      <td>2016-04-06 19:32:31</td>
      <td>1</td>
      <td>-74.010040</td>
      <td>40.719971</td>
      <td>-74.012268</td>
      <td>40.706718</td>
      <td>N</td>
      <td>429</td>
      <td>...</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>1.485498</td>
      <td>1.752341</td>
      <td>-179.872566</td>
      <td>0.038342</td>
      <td>-0.029194</td>
      <td>0.041343</td>
      <td>-0.042293</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id2181028</td>
      <td>2</td>
      <td>2016-03-26 13:30:55</td>
      <td>1</td>
      <td>-73.973053</td>
      <td>40.793209</td>
      <td>-73.972923</td>
      <td>40.782520</td>
      <td>N</td>
      <td>435</td>
      <td>...</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>1.188588</td>
      <td>1.224652</td>
      <td>179.990812</td>
      <td>-0.002877</td>
      <td>0.041748</td>
      <td>-0.002380</td>
      <td>0.031070</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>



# 1.2.d.3 Coordinates Clustering

# Gaussian Mixture


```python
from sklearn.mixture import GaussianMixture
```


```python
gaus_pick = GaussianMixture(n_components=20).fit(coord_pick)
gaus_drop = GaussianMixture(n_components=20).fit(coord_drop)
```


```python
train['gaus_pick'] = gaus_pick.predict(train[['pickup_latitude', 'pickup_longitude']])
test['gaus_pick'] = gaus_pick.predict(test[['pickup_latitude', 'pickup_longitude']])
train['gaus_drop'] = gaus_drop.predict(train[['dropoff_latitude', 'dropoff_longitude']])
test['gaus_drop'] = gaus_drop.predict(test[['dropoff_latitude', 'dropoff_longitude']])
```

# 1.2.d.4 Time data manipulating

# office hour


```python
labels = ["dawn", "morning", "afternoon", "evening", "night"]
cats1 = pd.cut(train['pick_hour'], 5, labels = labels)
cats2 = pd.cut(test['pick_hour'], 5, labels = labels)
train['office'] = cats1
test['office'] = cats2
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
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
      <th>trip_duration</th>
      <th>...</th>
      <th>uclidean</th>
      <th>manhatan</th>
      <th>direction</th>
      <th>pick_pca0</th>
      <th>pick_pca1</th>
      <th>drop_pca0</th>
      <th>drop_pca1</th>
      <th>gaus_pick</th>
      <th>gaus_drop</th>
      <th>office</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id2875421</td>
      <td>2</td>
      <td>2016-03-14 17:24:55</td>
      <td>1</td>
      <td>-73.982155</td>
      <td>40.767937</td>
      <td>-73.964630</td>
      <td>40.765602</td>
      <td>N</td>
      <td>455</td>
      <td>...</td>
      <td>1.498521</td>
      <td>2.248074</td>
      <td>174.333195</td>
      <td>0.007691</td>
      <td>0.017053</td>
      <td>-0.009667</td>
      <td>0.013695</td>
      <td>15</td>
      <td>1</td>
      <td>evening</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id2377394</td>
      <td>1</td>
      <td>2016-06-12 00:43:35</td>
      <td>1</td>
      <td>-73.980415</td>
      <td>40.738564</td>
      <td>-73.999481</td>
      <td>40.731152</td>
      <td>N</td>
      <td>663</td>
      <td>...</td>
      <td>1.805507</td>
      <td>2.997289</td>
      <td>-178.051506</td>
      <td>0.007677</td>
      <td>-0.012371</td>
      <td>0.027145</td>
      <td>-0.018652</td>
      <td>8</td>
      <td>13</td>
      <td>dawn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id3858529</td>
      <td>2</td>
      <td>2016-01-19 11:35:24</td>
      <td>1</td>
      <td>-73.979027</td>
      <td>40.763939</td>
      <td>-74.005333</td>
      <td>40.710087</td>
      <td>N</td>
      <td>2124</td>
      <td>...</td>
      <td>6.385098</td>
      <td>9.073912</td>
      <td>-179.629721</td>
      <td>0.004803</td>
      <td>0.012879</td>
      <td>0.034222</td>
      <td>-0.039337</td>
      <td>3</td>
      <td>0</td>
      <td>afternoon</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id3504673</td>
      <td>2</td>
      <td>2016-04-06 19:32:31</td>
      <td>1</td>
      <td>-74.010040</td>
      <td>40.719971</td>
      <td>-74.012268</td>
      <td>40.706718</td>
      <td>N</td>
      <td>429</td>
      <td>...</td>
      <td>1.485498</td>
      <td>1.752341</td>
      <td>-179.872566</td>
      <td>0.038342</td>
      <td>-0.029194</td>
      <td>0.041343</td>
      <td>-0.042293</td>
      <td>4</td>
      <td>0</td>
      <td>night</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id2181028</td>
      <td>2</td>
      <td>2016-03-26 13:30:55</td>
      <td>1</td>
      <td>-73.973053</td>
      <td>40.793209</td>
      <td>-73.972923</td>
      <td>40.782520</td>
      <td>N</td>
      <td>435</td>
      <td>...</td>
      <td>1.188588</td>
      <td>1.224652</td>
      <td>179.990812</td>
      <td>-0.002877</td>
      <td>0.041748</td>
      <td>-0.002380</td>
      <td>0.031070</td>
      <td>15</td>
      <td>15</td>
      <td>afternoon</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 26 columns</p>
</div>



# weekend


```python
train['weekend'] = 1 * ((train["pick_dayofweek"] == 5) | (train["pick_dayofweek"] == 6))
test['weekend'] = 1 * ((test["pick_dayofweek"] == 5) | (test["pick_dayofweek"] == 6))
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
      <th>id</th>
      <th>vendor_id</th>
      <th>pickup_datetime</th>
      <th>passenger_count</th>
      <th>pickup_longitude</th>
      <th>pickup_latitude</th>
      <th>dropoff_longitude</th>
      <th>dropoff_latitude</th>
      <th>store_and_fwd_flag</th>
      <th>trip_duration</th>
      <th>...</th>
      <th>manhatan</th>
      <th>direction</th>
      <th>pick_pca0</th>
      <th>pick_pca1</th>
      <th>drop_pca0</th>
      <th>drop_pca1</th>
      <th>gaus_pick</th>
      <th>gaus_drop</th>
      <th>office</th>
      <th>weekend</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>id2875421</td>
      <td>2</td>
      <td>2016-03-14 17:24:55</td>
      <td>1</td>
      <td>-73.982155</td>
      <td>40.767937</td>
      <td>-73.964630</td>
      <td>40.765602</td>
      <td>N</td>
      <td>455</td>
      <td>...</td>
      <td>2.248074</td>
      <td>174.333195</td>
      <td>0.007691</td>
      <td>0.017053</td>
      <td>-0.009667</td>
      <td>0.013695</td>
      <td>15</td>
      <td>1</td>
      <td>evening</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>id2377394</td>
      <td>1</td>
      <td>2016-06-12 00:43:35</td>
      <td>1</td>
      <td>-73.980415</td>
      <td>40.738564</td>
      <td>-73.999481</td>
      <td>40.731152</td>
      <td>N</td>
      <td>663</td>
      <td>...</td>
      <td>2.997289</td>
      <td>-178.051506</td>
      <td>0.007677</td>
      <td>-0.012371</td>
      <td>0.027145</td>
      <td>-0.018652</td>
      <td>8</td>
      <td>13</td>
      <td>dawn</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>id3858529</td>
      <td>2</td>
      <td>2016-01-19 11:35:24</td>
      <td>1</td>
      <td>-73.979027</td>
      <td>40.763939</td>
      <td>-74.005333</td>
      <td>40.710087</td>
      <td>N</td>
      <td>2124</td>
      <td>...</td>
      <td>9.073912</td>
      <td>-179.629721</td>
      <td>0.004803</td>
      <td>0.012879</td>
      <td>0.034222</td>
      <td>-0.039337</td>
      <td>3</td>
      <td>0</td>
      <td>afternoon</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>id3504673</td>
      <td>2</td>
      <td>2016-04-06 19:32:31</td>
      <td>1</td>
      <td>-74.010040</td>
      <td>40.719971</td>
      <td>-74.012268</td>
      <td>40.706718</td>
      <td>N</td>
      <td>429</td>
      <td>...</td>
      <td>1.752341</td>
      <td>-179.872566</td>
      <td>0.038342</td>
      <td>-0.029194</td>
      <td>0.041343</td>
      <td>-0.042293</td>
      <td>4</td>
      <td>0</td>
      <td>night</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>id2181028</td>
      <td>2</td>
      <td>2016-03-26 13:30:55</td>
      <td>1</td>
      <td>-73.973053</td>
      <td>40.793209</td>
      <td>-73.972923</td>
      <td>40.782520</td>
      <td>N</td>
      <td>435</td>
      <td>...</td>
      <td>1.224652</td>
      <td>179.990812</td>
      <td>-0.002877</td>
      <td>0.041748</td>
      <td>-0.002380</td>
      <td>0.031070</td>
      <td>15</td>
      <td>15</td>
      <td>afternoon</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>



# 3. Modeling

# evaluation metric

[Root Mean Squared Logarithmic Error](https://www.kaggle.com/wiki/RootMeanSquaredLogarithmicError)

$\epsilon = \sqrt{\frac{1}{n} \sum_{i=1}^n (\log(p_i + 1) - \log(a_i+1))^2 }$

Where:
- ϵ is the RMSLE value (score)

- n is the total number of observations in the (public/private) data set,

- pi is your prediction of trip duration, and
- ai is the actual trip duration for i.
- log(x) is the natural logarithm of x

### data type manipulation
- categorical data convert encoding


```python
train['store_and_fwd_flag'] = 1 * (train.store_and_fwd_flag.values == 'Y')
test['store_and_fwd_flag'] = 1 * (test.store_and_fwd_flag.values == 'Y')
```

# input data shape check


```python
train = train.drop('id', axis=1)
test = test.drop('id', axis=1)
train = train.drop(['pickup_datetime'], axis=1)
test = test.drop(['pickup_datetime'], axis=1)
print(train.shape, test.shape)
```

    (1458640, 25) (625134, 24)



```python
train = pd.get_dummies(train)
test = pd.get_dummies(test)
```


```python
X_train = train.drop(['trip_duration'], axis=1)
y_train = train['trip_duration']
y_log = np.log(y_train)
```

# lightgbm


```python
model_log = lgb.LGBMRegressor(n_estimators=12500, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1).fit(X_train, y_log)
```


```python
y_pred = model_log.predict(test)
```


```python
y_exp = np.exp(y_pred)

sub = pd.DataFrame(columns= ['id', 'trip_duration'])
sub['id'] = sample_submission["id"]
sub['trip_duration'] = y_exp
sub.to_csv('sub_lgb_exp1.csv',index=False)

!kaggle competitions submit -c nyc-taxi-trip-duration -f sub_lgb_exp1.csv -m "Message"

#n_estimators=500, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.39475, 0.39786
#n_estimators=1000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.39072, 0.39368
#n_estimators=1500, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38848, 0.39147
#n_estimators=2000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38670, 0.38967
#n_estimators=3000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38499, 0.38761
#n_estimators=4000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38368, 0.38634
#n_estimators=5000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38295, 0.38553
#n_estimators=10000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38206, 0.38433


#top score #top 16%
#n_estimators=15000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38180, 0.38384


#n_estimators=20000, reg_alpha=0.5, reg_lambda=0.5, n_jobs=-1
#0.38195, 0.38401
```

    Warning: Your Kaggle API key is readable by other users on this system! To fix this, you can run 'chmod 600 /home/jk/.kaggle/kaggle.json'
    Successfully submitted to New York City Taxi Trip Duration


```python
lgb.plot_importance(model_log)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f4bc3ef5cf8>




![png](taxi_V5.0_files/taxi_V5.0_98_1.png)


# Cross Validation


```python
# from sklearn.cross_validation import cross_val_score
```


```python
# cross_lgb = cross_val_score(model_log, X_train, y_log, cv=2, n_jobs=-1)
# cross_lgb
```

# OLS


```python
OLS_model = sm.OLS(y_log, X_train).fit()
print(OLS_model.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:          trip_duration   R-squared:                       0.358
    Model:                            OLS   Adj. R-squared:                  0.358
    Method:                 Least Squares   F-statistic:                 3.540e+04
    Date:                Thu, 26 Apr 2018   Prob (F-statistic):               0.00
    Time:                        16:15:42   Log-Likelihood:            -1.4198e+06
    No. Observations:             1458640   AIC:                         2.840e+06
    Df Residuals:                 1458616   BIC:                         2.840e+06
    Df Model:                          23                                         
    Covariance Type:            nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    vendor_id              0.0199      0.001     17.901      0.000       0.018       0.022
    passenger_count        0.0079      0.000     18.698      0.000       0.007       0.009
    pickup_longitude      -0.9181      0.007   -131.463      0.000      -0.932      -0.904
    pickup_latitude       -0.4466      0.010    -46.867      0.000      -0.465      -0.428
    dropoff_longitude      0.4693      0.007     71.526      0.000       0.456       0.482
    dropoff_latitude      -0.2251      0.008    -26.933      0.000      -0.241      -0.209
    store_and_fwd_flag     0.0007      0.007      0.098      0.922      -0.013       0.015
    pick_dayofweek         0.0153      0.000     34.435      0.000       0.014       0.016
    pick_month             0.0170      0.000     53.156      0.000       0.016       0.018
    pick_day               0.0007    6.1e-05     10.770      0.000       0.001       0.001
    pick_hour              0.0068      0.000     16.933      0.000       0.006       0.008
    holiday               -0.1176      0.003    -40.438      0.000      -0.123      -0.112
    weather               -0.0499      0.002    -23.725      0.000      -0.054      -0.046
    uclidean               0.1655      0.001    177.266      0.000       0.164       0.167
    manhatan              -0.0367      0.001    -61.396      0.000      -0.038      -0.036
    direction             -0.0003   4.85e-06    -53.675      0.000      -0.000      -0.000
    pick_pca0              0.5870      0.008     76.411      0.000       0.572       0.602
    pick_pca1             -0.6161      0.011    -55.641      0.000      -0.638      -0.594
    drop_pca0             -0.8110      0.008   -102.752      0.000      -0.827      -0.796
    drop_pca1             -0.4763      0.009    -50.474      0.000      -0.495      -0.458
    gaus_pick              0.0009   8.42e-05     10.158      0.000       0.001       0.001
    gaus_drop              0.0055   8.01e-05     68.585      0.000       0.005       0.006
    weekend               -0.1547      0.002    -80.970      0.000      -0.158      -0.151
    office_dawn           -0.0790      0.004    -18.453      0.000      -0.087      -0.071
    office_morning        -0.0312      0.002    -15.946      0.000      -0.035      -0.027
    office_afternoon       0.1163      0.001    105.402      0.000       0.114       0.118
    office_evening         0.0868      0.002     41.610      0.000       0.083       0.091
    office_night          -0.0879      0.004    -22.720      0.000      -0.095      -0.080
    ==============================================================================
    Omnibus:                  2672070.271   Durbin-Watson:                   2.001
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):     253169639877.915
    Skew:                         -12.080   Prob(JB):                         0.00
    Kurtosis:                    2043.831   Cond. No.                     1.28e+16
    ==============================================================================

    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The smallest eigenvalue is 1.65e-22. This might indicate that there are
    strong multicollinearity problems or that the design matrix is singular.



```python
Y_test = OLS_model.predict(test)
Y_test_exp = np.exp(Y_test)


sub = pd.DataFrame(columns= ['id', 'trip_duration'])
sub['id'] = sample_submission["id"]
sub['trip_duration'] = Y_test_exp
sub.to_csv('submission_OLS.csv',index=False)
!kaggle competitions submit -c nyc-taxi-trip-duration -f submission_OLS.csv -m "Message"
```

    Warning: Your Kaggle API key is readable by other users on this system! To fix this, you can run 'chmod 600 /home/jk/.kaggle/kaggle.json'
    Successfully submitted to New York City Taxi Trip Duration

# Appendix

### 1. degree of decimal
- 0.000001 = 1.11mm

### 2. spatial data analysis
- PCA
- discriminant analysis

### 3. clustering
- K means
- K nearest neighbor
- Expectation Maximization

### 4. ensemble methods
- aggregation
- boosting


***

*Reference*
