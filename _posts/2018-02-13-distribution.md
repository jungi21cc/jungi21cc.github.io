---
layout: post
title: Distribution
tags: [Math]

---

### Distribution

### 1. *continuous probability Distribution*

probability distribution that has a cumulative distribution function that is continuous

#### - normal distribution(= Gaussian normal distribution)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\mathcal{N}(x;&space;\mu,&space;\sigma^2)&space;=&space;\dfrac{1}{\sqrt{2\pi\sigma^2}}&space;\exp&space;\left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\mathcal{N}(x;&space;\mu,&space;\sigma^2)&space;=&space;\dfrac{1}{\sqrt{2\pi\sigma^2}}&space;\exp&space;\left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)" title="\mathcal{N}(x; \mu, \sigma^2) = \dfrac{1}{\sqrt{2\pi\sigma^2}} \exp \left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)" /></a>

#### - standard normal distribution

when ${\displaystyle \mu =0}$ and ${\displaystyle \sigma =1}$

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\mathcal{N}(x;&space;\0,&space;\1)&space;=&space;\dfrac{1}{\sqrt{2\pi}}&space;\exp&space;\left(-\dfrac{x^2}{2}\right)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\mathcal{N}(x;&space;\0,&space;\1)&space;=&space;\dfrac{1}{\sqrt{2\pi}}&space;\exp&space;\left(-\dfrac{x^2}{2}\right)" title="\mathcal{N}(x; \0, \1) = \dfrac{1}{\sqrt{2\pi}} \exp \left(-\dfrac{x^2}{2}\right)" /></a>

![alt text](350px-Normal_Distribution_PDF.svg.png)


#### - central limit Theorem

when independent random variables are added, their properly normalized sum tends toward a normal distribution even if the original variables themselves are not normally distributed

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;S_{n}:={\frac&space;{X_{1}&plus;\cdots&space;&plus;X_{n}}{n}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;S_{n}:={\frac&space;{X_{1}&plus;\cdots&space;&plus;X_{n}}{n}}" title="S_{n}:={\frac {X_{1}+\cdots +X_{n}}{n}}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;{\sqrt&space;{n}}\left(\left({\frac&space;{1}{n}}\sum&space;_{i=1}^{n}X_{i}\right)-\mu&space;\right)\&space;{\xrightarrow&space;{d}}\&space;N\left(0,\sigma&space;^{2}\right)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;{\sqrt&space;{n}}\left(\left({\frac&space;{1}{n}}\sum&space;_{i=1}^{n}X_{i}\right)-\mu&space;\right)\&space;{\xrightarrow&space;{d}}\&space;N\left(0,\sigma&space;^{2}\right)}" title="{\displaystyle {\sqrt {n}}\left(\left({\frac {1}{n}}\sum _{i=1}^{n}X_{i}\right)-\mu \right)\ {\xrightarrow {d}}\ N\left(0,\sigma ^{2}\right)}" /></a>


![alt text](/assets/img/IllustrationCentralTheorem.png)

#### - Student t distribution

any member of a family of continuous probability distributions that arises when estimating the mean of a normally distributed population in situations where the sample size is small and population standard deviation is unknown

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;t(x;\mu,&space;\sigma^2,&space;\nu)&space;=&space;\frac{\Gamma\left(\frac{\nu&plus;1}{2}\right)}&space;{\sqrt{\nu\pi}\Gamma\left(\frac{\nu}{2}\right)}&space;\left(1&plus;\frac{(x-\mu)^2}{\nu\sigma^2}&space;\right)^{-\frac{\nu&plus;1}{2}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;t(x;\mu,&space;\sigma^2,&space;\nu)&space;=&space;\frac{\Gamma\left(\frac{\nu&plus;1}{2}\right)}&space;{\sqrt{\nu\pi}\Gamma\left(\frac{\nu}{2}\right)}&space;\left(1&plus;\frac{(x-\mu)^2}{\nu\sigma^2}&space;\right)^{-\frac{\nu&plus;1}{2}}" title="t(x;\mu, \sigma^2, \nu) = \frac{\Gamma\left(\frac{\nu+1}{2}\right)} {\sqrt{\nu\pi}\Gamma\left(\frac{\nu}{2}\right)} \left(1+\frac{(x-\mu)^2}{\nu\sigma^2} \right)^{-\frac{\nu+1}{2}}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Gamma(x)&space;=&space;\int_0^\infty&space;u^{x-1}&space;e^{-u}&space;du" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Gamma(x)&space;=&space;\int_0^\infty&space;u^{x-1}&space;e^{-u}&space;du" title="\Gamma(x) = \int_0^\infty u^{x-1} e^{-u} du" /></a>


![alt text](/assets/img/student_t.png)


#### - chi-squared distribution

 the distribution of a sum of the squares of k independent standard normal random variables

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\sum_{i=1}^n&space;x_i^2&space;\sim&space;\chi^2(x;&space;n)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\sum_{i=1}^n&space;x_i^2&space;\sim&space;\chi^2(x;&space;n)" title="\sum_{i=1}^n x_i^2 \sim \chi^2(x; n)" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\chi^2(x;&space;\nu)&space;=&space;\frac{x^{(\nu/2-1)}&space;e^{-x/2}}{2^{\nu/2}&space;\Gamma\left(\frac{\nu}{2}\right)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\chi^2(x;&space;\nu)&space;=&space;\frac{x^{(\nu/2-1)}&space;e^{-x/2}}{2^{\nu/2}&space;\Gamma\left(\frac{\nu}{2}\right)}" title="\chi^2(x; \nu) = \frac{x^{(\nu/2-1)} e^{-x/2}}{2^{\nu/2} \Gamma\left(\frac{\nu}{2}\right)}" /></a>

![alt text](/assets/img/chi_square.png)

#### - F-distribution

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\dfrac{x_1&space;/&space;n_1}{x_2/&space;n_2}&space;\sim&space;F(n_1,&space;n_2)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\dfrac{x_1&space;/&space;n_1}{x_2/&space;n_2}&space;\sim&space;F(n_1,&space;n_2)" title="\dfrac{x_1 / n_1}{x_2/ n_2} \sim F(n_1, n_2)" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;f(x;&space;n_1,n_2)&space;=&space;\dfrac{\sqrt{\dfrac{(n_1\,x)^{n_1}\,\,n_2^{n_2}}&space;{(n_1\,x&plus;n_2)^{n_1&plus;n_2}}}}&space;{x\,\text{B}\!\left(\frac{n_1}{2},\frac{n_2}{2}\right)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;f(x;&space;n_1,n_2)&space;=&space;\dfrac{\sqrt{\dfrac{(n_1\,x)^{n_1}\,\,n_2^{n_2}}&space;{(n_1\,x&plus;n_2)^{n_1&plus;n_2}}}}&space;{x\,\text{B}\!\left(\frac{n_1}{2},\frac{n_2}{2}\right)}" title="f(x; n_1,n_2) = \dfrac{\sqrt{\dfrac{(n_1\,x)^{n_1}\,\,n_2^{n_2}} {(n_1\,x+n_2)^{n_1+n_2}}}} {x\,\text{B}\!\left(\frac{n_1}{2},\frac{n_2}{2}\right)}" /></a>

![alt text](/assets/img/f_distribution.png)


### 2. *Discrete probability Distribution*

probability distribution characterized by a probability mass function


#### - Bernoulli Distribution




#### - Binomial Distribution



#### - Categorical distribution



#### - Multinomial distribution




### 3. *Bayesian Estimator*

an estimator or decision rule that minimizes the posterior expected value of a loss function

#### - Beta distribution


#### - Dirichlet distribution


#### - Gamma distribution



***
*Reference*


https://en.wikipedia.org/wiki/Probability_distribution

https://en.wikipedia.org/wiki/Central_limit_theorem

https://en.wikipedia.org/wiki/Student%27s_t-distribution

https://en.wikipedia.org/wiki/Chi-squared_distribution

https://en.wikipedia.org/wiki/F-distribution