---
layout: post
title: Probability Model
tags: [Math]

---

### 1. *discrete random variable*

 When the image of X is finite or countably infinite, the random variable is called a discrete random variable

#### - Probability Mass Function

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;f_{X}(x)=\Pr(X=x)=\Pr(\{s\in&space;S:X(s)=x\})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;f_{X}(x)=\Pr(X=x)=\Pr(\{s\in&space;S:X(s)=x\})" title="f_{X}(x)=\Pr(X=x)=\Pr(\{s\in S:X(s)=x\})" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\sum&space;_{x\in&space;A}f_{X}(x)=1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\sum&space;_{x\in&space;A}f_{X}(x)=1" title="\sum _{x\in A}f_{X}(x)=1" /></a>

### 2. *continuous random variable*

When the image is uncountably infinite then X is called continuous random variable

#### - Probability Density Function

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\Pr[a\leq&space;X\leq&space;b]=\int&space;_{a}^{b}f_{X}(x)\,dx" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\Pr[a\leq&space;X\leq&space;b]=\int&space;_{a}^{b}f_{X}(x)\,dx" title="\Pr[a\leq X\leq b]=\int _{a}^{b}f_{X}(x)\,dx" /></a>

### 3. *mean, median, mode*

![alt text](/assets/img/Visualisation_mode_median_mean.svg.png)


### 4. *Moments*

#### - first moment (Expectation)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;\mu&space;\equiv&space;\operatorname&space;{E}&space;[X]}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;\mu&space;\equiv&space;\operatorname&space;{E}&space;[X]}" title="{\displaystyle \mu \equiv \operatorname {E} [X]}" /></a>

#### - sample mean

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{E}[\bar{X}]&space;=&space;\text{E}[{X}]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{E}[\bar{X}]&space;=&space;\text{E}[{X}]" title="\text{E}[\bar{X}] = \text{E}[{X}]" /></a>

#### - expectation properties

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\operatorname{E}[c]&space;=&space;c" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\operatorname{E}[c]&space;=&space;c" title="\operatorname{E}[c] = c" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\operatorname{E}[cX]&space;&=&space;c&space;\operatorname{E}[X]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\operatorname{E}[cX]&space;&=&space;c&space;\operatorname{E}[X]" title="\operatorname{E}[cX] &= c \operatorname{E}[X]" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\operatorname{E}[X&space;&plus;&space;Y]&space;&=&space;\operatorname{E}[X]&space;&plus;&space;\operatorname{E}[Y]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\operatorname{E}[X&space;&plus;&space;Y]&space;&=&space;\operatorname{E}[X]&space;&plus;&space;\operatorname{E}[Y]" title="\operatorname{E}[X + Y] &= \operatorname{E}[X] + \operatorname{E}[Y]" /></a>


#### - second moment (variance)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;\sigma^{2}&space;=&space;\left\operatorname&space;{E}&space;\left[(x-\mu&space;)^{2}\right]\right}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;\sigma^{2}&space;=&space;\left\operatorname&space;{E}&space;\left[(x-\mu&space;)^{2}\right]\right}" title="{\displaystyle \sigma^{2} = \left\operatorname {E} \left[(x-\mu )^{2}\right]\right}" /></a>

#### - sample variance

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{Var}[\bar{X}]&space;=&space;\dfrac{1}{N}&space;\text{Var}[{X}]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{Var}[\bar{X}]&space;=&space;\dfrac{1}{N}&space;\text{Var}[{X}]" title="\text{Var}[\bar{X}] = \dfrac{1}{N} \text{Var}[{X}]" /></a>

#### - biased sample variance

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;s^2&space;=&space;\dfrac{1}{N}\sum_{i=1}^{N}&space;(x_i-m)^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;s^2&space;=&space;\dfrac{1}{N}\sum_{i=1}^{N}&space;(x_i-m)^2" title="s^2 = \dfrac{1}{N}\sum_{i=1}^{N} (x_i-m)^2" /></a>

#### - unbiased sample variance

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;s^2_{\text{unbiased}}&space;=&space;\dfrac{1}{N-1}\sum_{i=1}^{N}&space;(x_i-m)^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;s^2_{\text{unbiased}}&space;=&space;\dfrac{1}{N-1}\sum_{i=1}^{N}&space;(x_i-m)^2" title="s^2_{\text{unbiased}} = \dfrac{1}{N-1}\sum_{i=1}^{N} (x_i-m)^2" /></a>

#### - variance properties

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{Var}[c]&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{Var}[c]&space;=&space;0" title="\text{Var}[c] = 0" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{Var}[cX]&space;=&space;c^2&space;\text{Var}[X]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{Var}[cX]&space;=&space;c^2&space;\text{Var}[X]" title="\text{Var}[cX] = c^2 \text{Var}[X]" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{Var}[X]&space;=&space;\text{E}[X^2]&space;-&space;(\text{E}[X])^2&space;=&space;\text{E}[X^2]&space;-&space;\mu^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{Var}[X]&space;=&space;\text{E}[X^2]&space;-&space;(\text{E}[X])^2&space;=&space;\text{E}[X^2]&space;-&space;\mu^2" title="\text{Var}[X] = \text{E}[X^2] - (\text{E}[X])^2 = \text{E}[X^2] - \mu^2" /></a>


#### - third moment (Skewness)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{E}[(X-\mu)^3]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{E}[(X-\mu)^3]" title="\text{E}[(X-\mu)^3]" /></a>

#### - fourth moment (Kurtosis)

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{E}[(X-\mu)^4]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{E}[(X-\mu)^4]" title="\text{E}[(X-\mu)^4]" /></a>

***
*Reference*

[random variable - wikipedia](https://en.wikipedia.org/wiki/Random_variable)

[moment (mathmatics) - wikipedia](https://en.wikipedia.org/wiki/Moment_(mathematics))

[expected value - wikipedia](https://en.wikipedia.org/wiki/Expected_value#Basic_properties)

[variance - wikipedia](https://en.wikipedia.org/wiki/Variance)
