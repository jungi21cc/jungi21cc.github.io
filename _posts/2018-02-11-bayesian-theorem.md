---
layout: post
title: Bayesian Theorem
tags: [Math]
---

### 1. *state of theorem*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(A\mid&space;B)={\frac&space;{P(B\mid&space;A)\,P(A)}{P(B)}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(A\mid&space;B)={\frac&space;{P(B\mid&space;A)\,P(A)}{P(B)}}}" title="{\displaystyle P(A\mid B)={\frac {P(B\mid A)\,P(A)}{P(B)}}}" /></a>


#### - prior probability 

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A)" title="P(A)" /></a>

the initial degree of belief in A

#### - likelihood probability 

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(B\mid&space;A)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(B\mid&space;A)}" title="{\displaystyle P(B\mid A)}" /></a>


#### - poterior probability 

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A\mid&space;B)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A\mid&space;B)" title="P(A\mid B)" /></a>

the probability for A after taking into account B for and against A


### 2. *conditaional probability*

- <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A\mid&space;B)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A\mid&space;B)" title="P(A\mid B)" /></a> 

conditional probability, the likelihood of event A occurring given that B is true

 - <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(B\mid&space;A)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(B\mid&space;A)}" title="{\displaystyle P(B\mid A)}" /></a> 

conditional probability, the likelihood of event B occurring given that A is true

 - <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A)&space;and&space;{\displaystyle&space;P(B)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A)&space;and&space;{\displaystyle&space;P(B)}" title="P(A) and {\displaystyle P(B)}" /></a> 

marginal probability

- memorable form

<a href="https://www.codecogs.com/eqnedit.php?latex={\displaystyle&space;P(A\mid&space;B)={\frac&space;{P(B\mid&space;A)\,P(A)}{P(B)}}}=\frac{(likelihood&space;*&space;prior)}{{P(B)}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\displaystyle&space;P(A\mid&space;B)={\frac&space;{P(B\mid&space;A)\,P(A)}{P(B)}}}=\frac{(likelihood&space;*&space;prior)}{{P(B)}}" title="{\displaystyle P(A\mid B)={\frac {P(B\mid A)\,P(A)}{P(B)}}}=\frac{(likelihood * prior)}{{P(B)}}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(A\mid&space;B)\propto&space;P(A)\cdot&space;P(B\mid&space;A)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(A\mid&space;B)\propto&space;P(A)\cdot&space;P(B\mid&space;A)}" title="{\displaystyle P(A\mid B)\propto P(A)\cdot P(B\mid A)}" /></a>



### 3. *drug test example*

Suppose that a test for using a particular drug is 99% sensitive and 99% specific. That is, the test will produce 99% true positive results for drug users and 99% true negative results for non-drug users. Suppose that 0.5% of people are users of the drug. What is the probability that a randomly selected individual with a positive test is a drug user?

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;$$&space;{\displaystyle&space;{\begin{aligned}P({\text{User}}\mid&space;{\text{&plus;}})&={\frac&space;{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})}{P(&plus;)}}\\&={\frac&space;{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})}{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})&plus;P({\text{&plus;}}\mid&space;{\text{Non-user}})P({\text{Non-user}})}}\\[8pt]&={\frac&space;{0.99\times&space;0.005}{0.99\times&space;0.005&plus;0.01\times&space;0.995}}\\[8pt]&\approx&space;33.2\%\end{aligned}}}&space;$$" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;$$&space;{\displaystyle&space;{\begin{aligned}P({\text{User}}\mid&space;{\text{&plus;}})&={\frac&space;{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})}{P(&plus;)}}\\&={\frac&space;{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})}{P({\text{&plus;}}\mid&space;{\text{User}})P({\text{User}})&plus;P({\text{&plus;}}\mid&space;{\text{Non-user}})P({\text{Non-user}})}}\\[8pt]&={\frac&space;{0.99\times&space;0.005}{0.99\times&space;0.005&plus;0.01\times&space;0.995}}\\[8pt]&\approx&space;33.2\%\end{aligned}}}&space;$$" title="$$ {\displaystyle {\begin{aligned}P({\text{User}}\mid {\text{+}})&={\frac {P({\text{+}}\mid {\text{User}})P({\text{User}})}{P(+)}}\\&={\frac {P({\text{+}}\mid {\text{User}})P({\text{User}})}{P({\text{+}}\mid {\text{User}})P({\text{User}})+P({\text{+}}\mid {\text{Non-user}})P({\text{Non-user}})}}\\[8pt]&={\frac {0.99\times 0.005}{0.99\times 0.005+0.01\times 0.995}}\\[8pt]&\approx 33.2\%\end{aligned}}} $$" /></a>


![alt text](/assets/img/bayes_ex.png)


***
*Reference*

[bayesian theorem - wikipedia](https://en.wikipedia.org/wiki/Bayes%27_theorem)

[posterior probability - wikipedia](https://en.wikipedia.org/wiki/Posterior_probability)

