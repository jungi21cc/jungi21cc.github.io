---
layout: post
title: Bayesian Theorem
tags: [Math]
---

**prior probability**
>현재 가지고 있는 정보를 기초로하여 정한 초기 확률
<br/>
>확률 시행 전에 이미 가지고 있는 지식을 통해 부여한 확률
<br/>
**likelihood probability**
>이미 알고있는 사건이 발생했다는 조건하에 다른 사건이 발생할 확률
<br/>
>확률 분포의 모수가, 어떤 확률변수의 표집값과 일관되는 정도를 나타내는 값
<br/>
**posterior probability**
>사건발생후에 어떤 원인으로 부터 일어난 것이라고 생각되어지는 확률
<br/>
추가된 정보로 부터 사전 정보를 새롭게 수정한 확률(수정확률)
<br/>

$$
{\displaystyle P(A\mid B)\propto P(A)\cdot P(B\mid A)}
$$

(proportionality over A for given B).
<br/>

posterior = (likelihood * prior) / evidence
<br/>


$$
{\displaystyle P(A\mid B)={\frac {P(B\mid A)\,P(A)}{P(B)}},}={(likelihood * prior) / {P(B)}}
$$

>posterior is proportional to prior times likelihood

**drug testing**
>Suppose that a test for using a particular drug is 99% sensitive and 99% specific. That is, the test will produce 99% true positive results for drug users and 99% true negative results for non-drug users. Suppose that 0.5% of people are users of the drug. What is the probability that a randomly selected individual with a positive test is a drug user?

$$
{\displaystyle {\begin{aligned}P({\text{User}}\mid {\text{+}})&={\frac {P({\text{+}}\mid {\text{User}})P({\text{User}})}{P(+)}}\\&={\frac {P({\text{+}}\mid {\text{User}})P({\text{User}})}{P({\text{+}}\mid {\text{User}})P({\text{User}})+P({\text{+}}\mid {\text{Non-user}})P({\text{Non-user}})}}\\[8pt]&={\frac {0.99\times 0.005}{0.99\times 0.005+0.01\times 0.995}}\\[8pt]&\approx 33.2\%\end{aligned}}}
$$
<br/>
<br/>

![alt text](/assets/img/bayes_ex.png)


***
*Reference*

[확률과 통계이론 베이즈정리](http://j1w2k3.tistory.com/1009)
