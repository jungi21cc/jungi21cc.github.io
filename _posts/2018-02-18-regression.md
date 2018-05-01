---
layout: post
title: Regression
tags: [Math]

---

### 1. *ordinary Least Square*

![alt text](/assets/img/ols_regression.png)

#### - linear regression analysis

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;y_{i}=\beta&space;_{1}x_{i1}&plus;\beta&space;_{2}x_{i2}&plus;\cdots&space;&plus;\beta&space;_{p}x_{ip}&plus;\varepsilon&space;_{i}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;y_{i}=\beta&space;_{1}x_{i1}&plus;\beta&space;_{2}x_{i2}&plus;\cdots&space;&plus;\beta&space;_{p}x_{ip}&plus;\varepsilon&space;_{i}}" title="{\displaystyle y_{i}=\beta _{1}x_{i1}+\beta _{2}x_{i2}+\cdots +\beta _{p}x_{ip}+\varepsilon _{i}}" /></a>

#### - bias augmentation

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;X&space;=&space;\begin{bmatrix}&space;x_{11}&space;&&space;x_{12}&space;&&space;\cdots&space;&&space;x_{1D}&space;\\&space;x_{21}&space;&&space;x_{22}&space;&&space;\cdots&space;&&space;x_{2D}&space;\\&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;\\&space;x_{N1}&space;&&space;x_{N2}&space;&&space;\cdots&space;&&space;x_{ND}&space;\\&space;\end{bmatrix}&space;\rightarrow&space;X_a&space;=&space;\begin{bmatrix}&space;1&space;&&space;x_{11}&space;&&space;x_{12}&space;&&space;\cdots&space;&&space;x_{1D}&space;\\&space;1&space;&&space;x_{21}&space;&&space;x_{22}&space;&&space;\cdots&space;&&space;x_{2D}&space;\\&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;\\&space;1&space;&&space;x_{N1}&space;&&space;x_{N2}&space;&&space;\cdots&space;&&space;x_{ND}&space;\\&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;X&space;=&space;\begin{bmatrix}&space;x_{11}&space;&&space;x_{12}&space;&&space;\cdots&space;&&space;x_{1D}&space;\\&space;x_{21}&space;&&space;x_{22}&space;&&space;\cdots&space;&&space;x_{2D}&space;\\&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;\\&space;x_{N1}&space;&&space;x_{N2}&space;&&space;\cdots&space;&&space;x_{ND}&space;\\&space;\end{bmatrix}&space;\rightarrow&space;X_a&space;=&space;\begin{bmatrix}&space;1&space;&&space;x_{11}&space;&&space;x_{12}&space;&&space;\cdots&space;&&space;x_{1D}&space;\\&space;1&space;&&space;x_{21}&space;&&space;x_{22}&space;&&space;\cdots&space;&&space;x_{2D}&space;\\&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;&&space;\vdots&space;\\&space;1&space;&&space;x_{N1}&space;&&space;x_{N2}&space;&&space;\cdots&space;&&space;x_{ND}&space;\\&space;\end{bmatrix}" title="X = \begin{bmatrix} x_{11} & x_{12} & \cdots & x_{1D} \\ x_{21} & x_{22} & \cdots & x_{2D} \\ \vdots & \vdots & \vdots & \vdots \\ x_{N1} & x_{N2} & \cdots & x_{ND} \\ \end{bmatrix} \rightarrow X_a = \begin{bmatrix} 1 & x_{11} & x_{12} & \cdots & x_{1D} \\ 1 & x_{21} & x_{22} & \cdots & x_{2D} \\ \vdots & \vdots & \vdots & \vdots & \vdots \\ 1 & x_{N1} & x_{N2} & \cdots & x_{ND} \\ \end{bmatrix}" /></a>


<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;f(x)&space;=&space;w_0&space;&plus;&space;w_1&space;x_1&space;&plus;&space;w_2&space;x_2&space;&plus;&space;\cdots&space;&plus;&space;w_D&space;x_D&space;=&space;\begin{bmatrix}&space;1&space;&&space;x_1&space;&&space;x_2&space;&&space;\cdots&space;&&space;x_D&space;\end{bmatrix}&space;\begin{bmatrix}&space;w_0&space;\\&space;w_1&space;\\&space;w_2&space;\\&space;\vdots&space;\\&space;w_D&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;f(x)&space;=&space;w_0&space;&plus;&space;w_1&space;x_1&space;&plus;&space;w_2&space;x_2&space;&plus;&space;\cdots&space;&plus;&space;w_D&space;x_D&space;=&space;\begin{bmatrix}&space;1&space;&&space;x_1&space;&&space;x_2&space;&&space;\cdots&space;&&space;x_D&space;\end{bmatrix}&space;\begin{bmatrix}&space;w_0&space;\\&space;w_1&space;\\&space;w_2&space;\\&space;\vdots&space;\\&space;w_D&space;\end{bmatrix}" title="f(x) = w_0 + w_1 x_1 + w_2 x_2 + \cdots + w_D x_D = \begin{bmatrix} 1 & x_1 & x_2 & \cdots & x_D \end{bmatrix} \begin{bmatrix} w_0 \\ w_1 \\ w_2 \\ \vdots \\ w_D \end{bmatrix}" /></a>


<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;f(x)&space;=&space;x_a^T&space;w_a&space;=&space;w_a^T&space;x_a" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;f(x)&space;=&space;x_a^T&space;w_a&space;=&space;w_a^T&space;x_a" title="f(x) = x_a^T w_a = w_a^T x_a" /></a>

### 2. *assumption*

- <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;$\operatorname&space;{E}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]=\beta&space;,\quad&space;\operatorname&space;{E}&space;[\,s^{2}\mid&space;X\,]=\sigma&space;^{2}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;$\operatorname&space;{E}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]=\beta&space;,\quad&space;\operatorname&space;{E}&space;[\,s^{2}\mid&space;X\,]=\sigma&space;^{2}$" title="$\operatorname {E} [\,{\hat {\beta }}\mid X\,]=\beta ,\quad \operatorname {E} [\,s^{2}\mid X\,]=\sigma ^{2}$" /></a>

- <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;$\operatorname&space;{Var}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]=\sigma&space;^{2}(X^{T}X)^{-1}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;$\operatorname&space;{Var}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]=\sigma&space;^{2}(X^{T}X)^{-1}$" title="$\operatorname {Var} [\,{\hat {\beta }}\mid X\,]=\sigma ^{2}(X^{T}X)^{-1}$" /></a>

- <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;$\operatorname&space;{Cov}&space;[\,{\hat&space;{\beta&space;}},{\hat&space;{\varepsilon&space;}}\mid&space;X\,]=0$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;$\operatorname&space;{Cov}&space;[\,{\hat&space;{\beta&space;}},{\hat&space;{\varepsilon&space;}}\mid&space;X\,]=0$" title="$\operatorname {Cov} [\,{\hat {\beta }},{\hat {\varepsilon }}\mid X\,]=0$" /></a>

- BLUE (best linear unbiased estimator)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\operatorname&space;{Var}&space;[\,{\tilde&space;{\beta&space;}}\mid&space;X\,]-\operatorname&space;{Var}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]\geq&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\operatorname&space;{Var}&space;[\,{\tilde&space;{\beta&space;}}\mid&space;X\,]-\operatorname&space;{Var}&space;[\,{\hat&space;{\beta&space;}}\mid&space;X\,]\geq&space;0" title="\operatorname {Var} [\,{\tilde {\beta }}\mid X\,]-\operatorname {Var} [\,{\hat {\beta }}\mid X\,]\geq 0" /></a>

- normalatiy

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;${\displaystyle&space;{\hat&space;{\beta&space;}}\&space;\sim&space;\&space;{\mathcal&space;{N}}{\big&space;(}\beta&space;,\&space;\sigma&space;^{2}(X^{\mathrm&space;{T}&space;}X)^{-1}{\big&space;)}}$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;${\displaystyle&space;{\hat&space;{\beta&space;}}\&space;\sim&space;\&space;{\mathcal&space;{N}}{\big&space;(}\beta&space;,\&space;\sigma&space;^{2}(X^{\mathrm&space;{T}&space;}X)^{-1}{\big&space;)}}$" title="${\displaystyle {\hat {\beta }}\ \sim \ {\mathcal {N}}{\big (}\beta ,\ \sigma ^{2}(X^{\mathrm {T} }X)^{-1}{\big )}}$" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;s^{2}\&space;\sim&space;\&space;{\frac&space;{\sigma&space;^{2}}{n-p}}\cdot&space;\chi&space;_{n-p}^{2}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;s^{2}\&space;\sim&space;\&space;{\frac&space;{\sigma&space;^{2}}{n-p}}\cdot&space;\chi&space;_{n-p}^{2}" title="s^{2}\ \sim \ {\frac {\sigma ^{2}}{n-p}}\cdot \chi _{n-p}^{2}" /></a>


### 3. *Residual sum of Square estimation*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\displaystyle&space;{\hat&space;y}=\operatorname&space;{arg&space;min}&space;f(x)=\sum&space;_{i=1}^{n}(y_{i}-x_{i}^{\mathrm&space;{T}&space;}b)^{2}=(y-Xb)^{\mathrm&space;{T}&space;}(y-Xb)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\displaystyle&space;{\hat&space;y}=\operatorname&space;{arg&space;min}&space;f(x)=\sum&space;_{i=1}^{n}(y_{i}-x_{i}^{\mathrm&space;{T}&space;}b)^{2}=(y-Xb)^{\mathrm&space;{T}&space;}(y-Xb)" title="\displaystyle {\hat y}=\operatorname {arg min} f(x)=\sum _{i=1}^{n}(y_{i}-x_{i}^{\mathrm {T} }b)^{2}=(y-Xb)^{\mathrm {T} }(y-Xb)" /></a>


### 4. *ANOVA*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\rm&space;{ESS}}&space;=&space;{\rm&space;{TSS}}&space;&plus;&space;{\rm&space;{RSS}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\rm&space;{ESS}}&space;=&space;{\rm&space;{TSS}}&space;&plus;&space;{\rm&space;{RSS}}" title="{\rm {ESS}} = {\rm {TSS}} + {\rm {RSS}}" /></a>

#### - TSS (total sum of square)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{TSS}&space;=&space;\sum_i&space;(y_i-\bar{y})^2&space;=&space;(y&space;-&space;\bar{y})^T(y&space;-&space;\bar{y}&space;)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{TSS}&space;=&space;\sum_i&space;(y_i-\bar{y})^2&space;=&space;(y&space;-&space;\bar{y})^T(y&space;-&space;\bar{y}&space;)" title="\text{TSS} = \sum_i (y_i-\bar{y})^2 = (y - \bar{y})^T(y - \bar{y} )" /></a>

#### - ESS (explained sum of square)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{ESS}=\sum_i&space;(\hat{y}_i&space;-\bar{\hat{y}})^2&space;=&space;(\hat{y}&space;-&space;\bar{\hat{y}})^T(\hat{y}&space;-&space;\bar{\hat{y}})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{ESS}=\sum_i&space;(\hat{y}_i&space;-\bar{\hat{y}})^2&space;=&space;(\hat{y}&space;-&space;\bar{\hat{y}})^T(\hat{y}&space;-&space;\bar{\hat{y}})" title="\text{ESS}=\sum_i (\hat{y}_i -\bar{\hat{y}})^2 = (\hat{y} - \bar{\hat{y}})^T(\hat{y} - \bar{\hat{y}})" /></a>

#### - RSS (residual sum of square)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{RSS}=\sum_i&space;(y_i&space;-&space;\hat{y}_i)^2\&space;=&space;e^Te" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{RSS}=\sum_i&space;(y_i&space;-&space;\hat{y}_i)^2\&space;=&space;e^Te" title="\text{RSS}=\sum_i (y_i - \hat{y}_i)^2\ = e^Te" /></a>

#### - R square

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;R^{2}={\frac&space;{\sum&space;({\hat&space;{y}}_{i}-{\overline&space;{y}})^{2}}{\sum&space;(y_{i}-{\overline&space;{y}})^{2}}}={\frac&space;{\rm&space;{ESS}}{\rm&space;{TSS}}}=1-{\frac&space;{\rm&space;{RSS}}{\rm&space;{TSS}}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;R^{2}={\frac&space;{\sum&space;({\hat&space;{y}}_{i}-{\overline&space;{y}})^{2}}{\sum&space;(y_{i}-{\overline&space;{y}})^{2}}}={\frac&space;{\rm&space;{ESS}}{\rm&space;{TSS}}}=1-{\frac&space;{\rm&space;{RSS}}{\rm&space;{TSS}}}}" title="{\displaystyle R^{2}={\frac {\sum ({\hat {y}}_{i}-{\overline {y}})^{2}}{\sum (y_{i}-{\overline {y}})^{2}}}={\frac {\rm {ESS}}{\rm {TSS}}}=1-{\frac {\rm {RSS}}{\rm {TSS}}}}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;0&space;\leq&space;R^2&space;\leq&space;1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;0&space;\leq&space;R^2&space;\leq&space;1" title="0 \leq R^2 \leq 1" /></a>

#### - adjust R square

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;R_{adj}^2&space;=&space;1&space;-&space;\frac{n-1}{n-K}(1-R^2)&space;=&space;\dfrac{(n-1)R^2&space;&plus;1-K}{n-K}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;R_{adj}^2&space;=&space;1&space;-&space;\frac{n-1}{n-K}(1-R^2)&space;=&space;\dfrac{(n-1)R^2&space;&plus;1-K}{n-K}" title="R_{adj}^2 = 1 - \frac{n-1}{n-K}(1-R^2) = \dfrac{(n-1)R^2 +1-K}{n-K}" /></a>


***
*Reference*

[ordinary least square - wikipedia](https://en.wikipedia.org/wiki/Ordinary_least_squares)

[gauss markov theorem - wikipedia](https://en.wikipedia.org/wiki/Gauss%E2%80%93Markov_theorem)


