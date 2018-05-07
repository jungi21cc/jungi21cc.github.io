---
layout: post
title: Logistic Regression
tags: [Machine Learning]

---

### Logistic Regression

### 1. *Sigmoid Function*

#### - logistic function

<a href="https://www.codecogs.com/eqnedit.php?latex=\text{logitstic}(z)&space;=&space;\sigma(z)&space;=&space;\dfrac{1}{1&plus;\exp{(-z)}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{logitstic}(z)&space;=&space;\sigma(z)&space;=&space;\dfrac{1}{1&plus;\exp{(-z)}}" title="\text{logitstic}(z) = \sigma(z) = \dfrac{1}{1+\exp{(-z)}}" /></a>

#### - Hyperbolic tangent

<a href="https://www.codecogs.com/eqnedit.php?latex=\tanh(z)&space;=&space;\frac{\sinh&space;z}{\cosh&space;z}&space;=&space;\frac&space;{e^z&space;-&space;e^{-z}}&space;{e^z&space;&plus;&space;e^{-z}}&space;=&space;2&space;\sigma(2x)&space;-&space;1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\tanh(z)&space;=&space;\frac{\sinh&space;z}{\cosh&space;z}&space;=&space;\frac&space;{e^z&space;-&space;e^{-z}}&space;{e^z&space;&plus;&space;e^{-z}}&space;=&space;2&space;\sigma(2x)&space;-&space;1" title="\tanh(z) = \frac{\sinh z}{\cosh z} = \frac {e^z - e^{-z}} {e^z + e^{-z}} = 2 \sigma(2x) - 1" /></a>

![alt text](/assets/img/logistic.png)


### 2. odds ratio

<a href="https://www.codecogs.com/eqnedit.php?latex=\text{odds&space;ratio}&space;=&space;\dfrac{\theta}{1-\theta}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{odds&space;ratio}&space;=&space;\dfrac{\theta}{1-\theta}" title="\text{odds ratio} = \dfrac{\theta}{1-\theta}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=z&space;=&space;\text{logit}(\text{odds&space;ratio})&space;=&space;\log&space;\left(\dfrac{\theta}{1-\theta}\right)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?z&space;=&space;\text{logit}(\text{odds&space;ratio})&space;=&space;\log&space;\left(\dfrac{\theta}{1-\theta}\right)" title="z = \text{logit}(\text{odds ratio}) = \log \left(\dfrac{\theta}{1-\theta}\right)" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\text{logitstic}(z)&space;=&space;\theta(z)&space;=&space;\dfrac{1}{1&plus;\exp{(-z)}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{logitstic}(z)&space;=&space;\theta(z)&space;=&space;\dfrac{1}{1&plus;\exp{(-z)}}" title="\text{logitstic}(z) = \theta(z) = \dfrac{1}{1+\exp{(-z)}}" /></a>


### 3. Parameter estimation (Log Likelihood)

<a href="https://www.codecogs.com/eqnedit.php?latex=\text{LL}&space;&=&&space;\log&space;\prod_{i=1}^N&space;\theta_i(x_i;w)^{y_i}&space;(1-\theta_i(x_i;w))^{1-y_i}&space;\\" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\text{LL}&space;&=&&space;\log&space;\prod_{i=1}^N&space;\theta_i(x_i;w)^{y_i}&space;(1-\theta_i(x_i;w))^{1-y_i}&space;\\" title="\text{LL} &=& \log \prod_{i=1}^N \theta_i(x_i;w)^{y_i} (1-\theta_i(x_i;w))^{1-y_i} \\" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=&=&&space;\sum_{i=1}^N&space;\left(&space;y_i&space;\log\theta_i(x_i;w)&space;&plus;&space;(1-y_i)\log(1-\theta_i(x_i;w))&space;\right)&space;\\" target="_blank"><img src="https://latex.codecogs.com/gif.latex?&=&&space;\sum_{i=1}^N&space;\left(&space;y_i&space;\log\theta_i(x_i;w)&space;&plus;&space;(1-y_i)\log(1-\theta_i(x_i;w))&space;\right)&space;\\" title="&=& \sum_{i=1}^N \left( y_i \log\theta_i(x_i;w) + (1-y_i)\log(1-\theta_i(x_i;w)) \right) \\" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex==&space;\sum_{i=1}^N(y_i&space;\log&space;(\frac{1}{1&space;&plus;&space;\exp(-w^{T}x_i)})&space;&plus;&space;(1-y_i)\log&space;(\frac{\exp&space;(-w^{T}x)}{1&space;&plus;&space;\exp&space;(-w^{T}x_i)})&space;)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?=&space;\sum_{i=1}^N(y_i&space;\log&space;(\frac{1}{1&space;&plus;&space;\exp(-w^{T}x_i)})&space;&plus;&space;(1-y_i)\log&space;(\frac{\exp&space;(-w^{T}x)}{1&space;&plus;&space;\exp&space;(-w^{T}x_i)})&space;)" title="= \sum_{i=1}^N(y_i \log (\frac{1}{1 + \exp(-w^{T}x_i)}) + (1-y_i)\log (\frac{\exp (-w^{T}x)}{1 + \exp (-w^{T}x_i)}) )" /></a>


### 4. pseudo R square (likelihood ratio R square)

<a href="https://www.codecogs.com/eqnedit.php?latex=R^2_{\text{pseudo}}&space;=&space;1&space;-&space;\dfrac{G^2}{G^2_0}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?R^2_{\text{pseudo}}&space;=&space;1&space;-&space;\dfrac{G^2}{G^2_0}" title="R^2_{\text{pseudo}} = 1 - \dfrac{G^2}{G^2_0}" /></a>

where

<a href="https://www.codecogs.com/eqnedit.php?latex=G^2&space;=&space;2\sum_{i=1}^N&space;\left(&space;y_i\log\dfrac{y_i}{\hat{y}_i}&space;&plus;&space;(1-y_i)\log\dfrac{1-y_i}{1-\hat{y}_i}&space;\right)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?G^2&space;=&space;2\sum_{i=1}^N&space;\left(&space;y_i\log\dfrac{y_i}{\hat{y}_i}&space;&plus;&space;(1-y_i)\log\dfrac{1-y_i}{1-\hat{y}_i}&space;\right)" title="G^2 = 2\sum_{i=1}^N \left( y_i\log\dfrac{y_i}{\hat{y}_i} + (1-y_i)\log\dfrac{1-y_i}{1-\hat{y}_i} \right)" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=G^2_0&space;=&space;null&space;model" target="_blank"><img src="https://latex.codecogs.com/gif.latex?G^2_0&space;=&space;null&space;model" title="G^2_0 = null model" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=H_0&space;:&space;w_0&space;=&space;w_1&space;=&space;\cdots&space;=&space;w_{K-1}&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?H_0&space;:&space;w_0&space;=&space;w_1&space;=&space;\cdots&space;=&space;w_{K-1}&space;=&space;0" title="H_0 : w_0 = w_1 = \cdots = w_{K-1} = 0" /></a>


### 5. Logistic Regression Report



```python
import pandas as pd
import numpy as np
```


```python
df = pd.read_csv("http://www.stat.tamu.edu/~sheather/book/docs/datasets/MichelinNY.csv", encoding="latin1")
```


```python
x = df.drop(['InMichelin', 'Restaurant Name'], axis=1)
y = df.InMichelin
```


```python
import statsmodels.api as sm
from scipy import stats
stats.chisqprob = lambda chisq, df: stats.chi2.sf(chisq, df)

logit_model=sm.Logit(y,x)
result=logit_model.fit()
print(result.summary())
```

    Optimization terminated successfully.
             Current function value: 0.547718
             Iterations 7
                               Logit Regression Results                           
    ==============================================================================
    Dep. Variable:             InMichelin   No. Observations:                  164
    Model:                          Logit   Df Residuals:                      160
    Method:                           MLE   Df Model:                            3
    Date:                Fri, 04 May 2018   Pseudo R-squ.:                  0.2043
    Time:                        20:42:00   Log-Likelihood:                -89.826
    converged:                       True   LL-Null:                       -112.89
                                            LLR p-value:                 5.303e-10
    ==============================================================================
                     coef    std err          z      P>|z|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Food           0.0211      0.107      0.198      0.843      -0.188       0.230
    Decor         -0.0357      0.078     -0.460      0.646      -0.188       0.117
    Service       -0.3272      0.121     -2.702      0.007      -0.565      -0.090
    Price          0.1349      0.028      4.864      0.000       0.081       0.189
    ==============================================================================




***
*Reference*

[Logistic Regression - wikipedia](https://en.wikipedia.org/wiki/Logistic_regression)

[로지스틱회귀 - ratsgo's blog](https://ratsgo.github.io/machine%20learning/2017/04/02/logistic/)
