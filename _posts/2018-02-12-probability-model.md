---
layout: post
title: Probability Model
tags: [Math]
---
**discrete random variable** <br/>
> one for which, for any value in the range that the variable is permitted to take on, there is a positive minimum distance to the nearest other permissible value.



**continuous random variable** <br/>
> one which can take on infinitely many, uncountable values


![alt text](/assets/img/pdf.png)

**probability density function formal form** <br/>
$$
\Pr[a\leq X\leq b]=\int _{a}^{b}f_{X}(x)\,dx
$$
<br/>



**expectation** <br/>

$
m = \bar{x} = \dfrac{1}{N}\sum_{i=1}^{N} x_i
$

<br/>

**expectation properties**

<br/>
<br/>

$
\operatorname{E}[c] = c
\begin{align}
$

<br/>

$
\operatorname{E}[cX]    &= c \operatorname{E}[X] \\
$

<br/>

$
\operatorname{E}[X + Y] &=   \operatorname{E}[X] + \operatorname{E}[Y]
\end{align}
$

<br/>

$
\operatorname{E}[ \bar{X} ] = \operatorname{E}[X]
$

**mean, median, mode** <br/>

![alt text](/assets/img/Visualisation_mode_median_mean.svg.png)


**sample variance** <br/>
biased sample variance:

$$
s^2 = \dfrac{1}{N}\sum_{i=1}^{N} (x_i-m)^2
$$

<br/>
unbiased sample variance:

$$
s^2_{\text{unbiased}} = \dfrac{1}{N-1}\sum_{i=1}^{N} (x_i-m)^2
$$

<br/>
<br/>

**variance** <br/>
<br/>

variance formal equation

$$
\sigma^2 = \text{Var}[X] = \text{E}[(X - \mu)^2]
$$

<br/>
probability density function variance
<br/>

$$
\sigma^2 = \text{Var}[X] = \text{E}[(X - \mu)^2] =  \sum (x - \mu)^2 P(x)
$$

<br/>

$$
\sigma^2 = \text{Var}[X] = \text{E}[(X - \mu)^2] = \int_{-\infty}^{\infty} (x - \mu)^2 f(x)dx
$$

**variance properties** <br/>

$$
\text{Var}[X] \geq 0
$$

<br/>

$$
\text{Var}[c] = 0
$$

<br/>

$$
\text{Var}[cX] = c^2 \text{Var}[X]
$$

<br/>

$$
\text{Var}[X] = \text{E}[X^2] - (\text{E}[X])^2  = \text{E}[X^2] - \mu^2
$$

<br/>
when two ramdom variables are independent,
<br/>

$$
\text{Var}\left[ X + Y \right] =  \text{Var}\left[ X \right] + \text{Var}\left[ Y \right]
$$

**sample expectation of variance** <br/>
sample expectation

$$
\text{E}[\bar{X}] = \text{E}[{X}]
$$

<br/>
sample variance

$$
\text{Var}[\bar{X}] = \dfrac{1}{N} \text{Var}[{X}]
$$

**sample variance of expectation** <br/>

$$
\text{E}[s^2] = \dfrac{N-1}{N}\sigma^2
$$

<br/>
be more specifically,
<br/>

$$
\sigma^2
= \dfrac{N}{N-1} \text{E}[s^2]
= \dfrac{N}{N-1} \text{E} \left[ \dfrac{1}{N} \sum (X_i-\bar{X})^2 \right]
= \text{E} \left[ \dfrac{1}{N-1} \sum (X_i-\bar{X})^2 \right]
= \text{E} \left[ s^2_{\text{unbiased}} \right]
$$

<br/>
<br/>
<br/>

**moment**
<br/>
<br/>

1차 모멘트 =  E[X]  : 기댓값 (Expectation) <br/>
2차 모멘트 =  E[(X−μ)2]  : 분산 (Variance) <br/>
3차 모멘트 =  E[(X−μ)3]  : 스큐니스 (Skewness) <br/>
4차 모멘트 =  E[(X−μ)4]  : 커토시스 (Kurtosis) <br/>
<br/>
<br/>



***
*Reference*
