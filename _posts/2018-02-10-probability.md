---
layout: post
title: Probability
tags: [Math]
---

확률(probability)이란 사건(부분 집합)을 입력하면 숫자(확률값)가 출력되는 함수이다.
<br/>
사건(부분집합)→숫자
<br/>
<br/>
P 는 함수이고  이 함수는 다음과 같은 세가지 규칙을 지켜야 한다.
<br/>
콜모고로프의 공리(Kolmogorov's axioms)
<br/>
**First axiom** <br/>
The probability of an event is a non-negative real number:
모든 사건에 대해 확률은 실수이고 0 또는 양수이다
<br/>
$$
{\displaystyle P(E)\in \mathbb {R} ,P(E)\geq 0\qquad \forall E\in F} {\displaystyle P(E)\in \mathbb {R} ,P(E)\geq 0\qquad \forall E\in F}
$$
<br/>
$$
P(A)\in\mathbb{R}, P(A)\geq 0
$$
<br/>
<br/>
**second axiom** <br/>
This is the assumption of unit measure: that the probability that at least one of the elementary events in the entire sample space will occur is 1.
표본공간이라는 사건에 대한 확률은 1이다.
<br/>
$$
{\displaystyle P(\Omega )=1.} P(\Omega )=1
$$
<br/>
$$
P(\Omega) = 1
$$
<br/>
<br/>
**thrid axiom** <br/>
This is the assumption of σ-additivity:
공통 원소가 없는 두 사건의 합집합의 확률은 각각의 사건의 확률의 합이다.
<br/>
Any countable sequence of disjoint sets (synonymous with mutually exclusive events)
$$
{\displaystyle E_{1},E_{2},\ldots } {\displaystyle E_{1},E_{2},\ldots } satisfies
{\displaystyle P\left(\bigcup _{i=1}^{\infty }E_{i}\right)=\sum _{i=1}^{\infty }P(E_{i}).} P\left(\bigcup _{i=1}^{\infty }E_{i}\right)=\sum _{i=1}^{\infty }P(E_{i}).
$$
<br/>
$$
A \cap B = \emptyset \;\;\; \rightarrow \;\;\; P(A \cup B) = P(A) + P(B)
$$
<br/>
<br/>

*frequency probabilities*
> associated with **random physical systems** such as roulette wheels, rolling dice and radioactive atoms

*Bayesian probability*
 > assigned to any statement whatsoever, even when **no random process** is involved, as a way to represent its subjective plausibility, or the degree to which the statement is supported by the available evidence


![alt text](/assets/img/probability_table.svg)



***
*Reference*
