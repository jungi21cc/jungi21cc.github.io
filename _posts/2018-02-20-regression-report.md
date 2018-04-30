---
layout: post
title: Regression Result Report
tags: [Math]

---

### Regression Result Report

### 1. *OLS regression report*

```python
import pandas as pd
import numpy as np
from sklearn.datasets import load_boston
import statsmodels.api as sm
import statsmodels.formula.api as smf

boston = load_boston()
dfX = pd.DataFrame(boston.data, columns=boston.feature_names)
dfy = pd.DataFrame(boston.target, columns=["MEDV"])
df = pd.concat([dfX, dfy], axis=1)
```


```python
model = sm.OLS.from_formula("np.log(MEDV) ~ "
                             "scale(CRIM) + scale(ZN) + scale(INDUS) + "
                             "scale(NOX) + scale(RM) + scale(AGE) + "
                             "scale(np.log(DIS)) + scale(RAD) + scale(TAX) + "
                             "scale(np.log(PTRATIO)) + scale(B) + scale(np.log(LSTAT)) + CHAS", 
                             data=df)
result = model.fit()
print(result.summary())
```


                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:           np.log(MEDV)   R-squared:                       0.806
    Model:                            OLS   Adj. R-squared:                  0.801
    Method:                 Least Squares   F-statistic:                     157.4
    Date:                Mon, 30 Apr 2018   Prob (F-statistic):          8.61e-166
    Time:                        10:02:35   Log-Likelihood:                 150.27
    No. Observations:                 506   AIC:                            -272.5
    Df Residuals:                     492   BIC:                            -213.4
    Df Model:                          13                                         
    Covariance Type:            nonrobust                                         
    ==========================================================================================
                                 coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------------------
    Intercept                  3.0284      0.008    359.502      0.000       3.012       3.045
    scale(CRIM)               -0.1089      0.011     -9.972      0.000      -0.130      -0.087
    scale(ZN)                 -0.0088      0.012     -0.753      0.452      -0.032       0.014
    scale(INDUS)               0.0061      0.016      0.376      0.707      -0.026       0.038
    scale(NOX)                -0.1056      0.018     -5.940      0.000      -0.141      -0.071
    scale(RM)                  0.0458      0.012      3.884      0.000       0.023       0.069
    scale(AGE)                 0.0099      0.015      0.669      0.504      -0.019       0.039
    scale(np.log(DIS))        -0.1082      0.018     -5.896      0.000      -0.144      -0.072
    scale(RAD)                 0.1270      0.022      5.716      0.000       0.083       0.171
    scale(TAX)                -0.1054      0.024     -4.333      0.000      -0.153      -0.058
    scale(np.log(PTRATIO))    -0.0777      0.011     -7.089      0.000      -0.099      -0.056
    scale(B)                   0.0358      0.009      3.809      0.000       0.017       0.054
    scale(np.log(LSTAT))      -0.2307      0.015    -15.570      0.000      -0.260      -0.202
    CHAS                       0.0882      0.033      2.662      0.008       0.023       0.153
    ==============================================================================
    Omnibus:                       39.523   Durbin-Watson:                   1.132
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              168.952
    Skew:                          -0.110   Prob(JB):                     2.05e-37
    Kurtosis:                       5.822   Cond. No.                         10.7
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


***
*Reference*
