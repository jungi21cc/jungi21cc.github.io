---
layout: post
title: Distribution
tags: [Math]
---
**continuous probability Distribution**
> probability distribution that has a cumulative distribution function that is continuous

<br/>
*normal distribution(= Gaussian normal distribution)*
<br/>

$$
\mathcal{N}(x; \mu, \sigma^2) = \dfrac{1}{\sqrt{2\pi\sigma^2}} \exp \left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)
$$

<br/>
<br/>

when $${\displaystyle \mu =0} \mu =0$$ and $${\displaystyle \sigma =1} \sigma =1$$

<br/>

*standard normal distribution*

<br/>
<br/>

$\mathcal{N}(x; \0, \1) = \dfrac{1}{\sqrt{2\pi}} \exp \left(-\dfrac{x^2}{2}\right)$

<br/>
<br/>

*central limit Theorem*

>in most situations, when independent random variables are added, their properly normalized sum tends toward a normal distribution (informally a "bell curve") even if the original variables themselves are not normally distributed

*chi-squared distribution*
>


*F-distribution*
>


<br/>

**Discrete probability Distribution**
> a probability distribution characterized by a probability mass function

<br/>

*Bernoulli Distribution*
> the probability distribution of a random variable which takes the value 1 with probability $${\displaystyle p}$$ p and the value 0 with probability $${\displaystyle q=1-p} q=1-p$$

<br/>
<br/>

- parameter

<br/>

$$
0<p<1,p\in \mathbb {R}
$$

<br/>
<br/>

- Bernoulli Distribution

<br/>

$$
{\displaystyle \Pr(X=1)=p=1-\Pr(X=0)=1-q.}
$$

<br/>

$$
f(k;p)={\begin{cases}p&{\text{if }}k=1,\\[6pt]1-p&{\text{if }}k=0.\end{cases}}
$$

<br/>
<br/>

- Bernoulli Distribution properties

<br/>
<br/>

    - expectation of Bernoulli Distribution
<br/>

$$
\text{E}[X]  = \theta
$$

<br/>
<br/>

    - variance of Bernoulli Distribution
<br/>

$$
\text{Var}[X] = \theta(1-\theta)
$$

<br>
<br/>

*Binomial Distribution*
> the discrete probability distribution of the number of successes in a sequence of n independent experiments, each asking a yes–no question, and each with its own boolean-valued outcome

<br/>

$$
X \sim \text{Bin}(x;N,\theta)
$$

<br/>
$$
\text{Bin}(x;N,\theta) = \binom N x  \theta^x(1-\theta)^{N-x}
$$
<br/>
$$
\binom N x =\dfrac{N!}{x!(N-x)!}
$$
<br/>
<br/>
<br/>
- Binomial Distribution properties
<br/>
    - expectation of Binomial Distribution
<br/>
$$
\text{E}[X] = N\theta
$$
<br/>
<br/>
    - variance of Binomial Distribution
<br/>
$$
\text{Var}[X] = N\theta(1-\theta)
$$
<br>

*Categorical distribution*
>




*Multinomial distribution*
>



**Bayes Estimator**
> an estimator or decision rule that minimizes the posterior expected value of a loss function

*Beta distribution*
>

*Dirichlet distribution*
>

*Gamma distribution*
>


***
*Reference*
