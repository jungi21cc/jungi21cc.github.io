---
layout: post
title: Probability
tags: [Math]
---


1. *frequentist probability*
- Objectivists assign numbers to describe some objective or physical state of affairs.

2. *Bayesian probability*
 - Subjectivists assign numbers per subjective probability.


3. *Independent Events*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A{\mbox{&space;and&space;}}B)=P(A\cap&space;B)=P(A)P(B)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A{\mbox{&space;and&space;}}B)=P(A\cap&space;B)=P(A)P(B)" title="P(A{\mbox{ and }}B)=P(A\cap B)=P(A)P(B)" /></a>


4. *Mutually exclusive events*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(A{\mbox{&space;and&space;}}B)=P(A\cap&space;B)=0}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(A{\mbox{&space;and&space;}}B)=P(A\cap&space;B)=0}" title="{\displaystyle P(A{\mbox{ and }}B)=P(A\cap B)=0}" /></a>


5. *summary of probability*

- A

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A)\in&space;[0,1]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A)\in&space;[0,1]" title="P(A)\in [0,1]" /></a>


- not A

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;P(A^{\complement&space;})=1-P(A)\,}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;P(A^{\complement&space;})=1-P(A)\,}" title="{\displaystyle P(A^{\complement })=1-P(A)\,}" /></a>

- A or B

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\begin{aligned}P(A\cup&space;B)&=P(A)&plus;P(B)-P(A\cap&space;B)\\P(A\cup&space;B)&=P(A)&plus;P(B)\qquad&space;{\mbox{if&space;A&space;and&space;B&space;are&space;mutually&space;exclusive}}\\\end{aligned}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\begin{aligned}P(A\cup&space;B)&=P(A)&plus;P(B)-P(A\cap&space;B)\\P(A\cup&space;B)&=P(A)&plus;P(B)\qquad&space;{\mbox{if&space;A&space;and&space;B&space;are&space;mutually&space;exclusive}}\\\end{aligned}}" title="{\begin{aligned}P(A\cup B)&=P(A)+P(B)-P(A\cap B)\\P(A\cup B)&=P(A)+P(B)\qquad {\mbox{if A and B are mutually exclusive}}\\\end{aligned}}" /></a>


- A and B

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\begin{aligned}P(A\cap&space;B)&=P(A|B)P(B)=P(B|A)P(A)\\P(A\cap&space;B)&=P(A)P(B)\qquad&space;{\mbox{if&space;A&space;and&space;B&space;are&space;independent}}\\\end{aligned}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\begin{aligned}P(A\cap&space;B)&=P(A|B)P(B)=P(B|A)P(A)\\P(A\cap&space;B)&=P(A)P(B)\qquad&space;{\mbox{if&space;A&space;and&space;B&space;are&space;independent}}\\\end{aligned}}" title="{\begin{aligned}P(A\cap B)&=P(A|B)P(B)=P(B|A)P(A)\\P(A\cap B)&=P(A)P(B)\qquad {\mbox{if A and B are independent}}\\\end{aligned}}" /></a>

- A given B

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A\mid&space;B)={\frac&space;{P(A\cap&space;B)}{P(B)}}={\frac&space;{P(B|A)P(A)}{P(B)}}\," target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A\mid&space;B)={\frac&space;{P(A\cap&space;B)}{P(B)}}={\frac&space;{P(B|A)P(A)}{P(B)}}\," title="P(A\mid B)={\frac {P(A\cap B)}{P(B)}}={\frac {P(B|A)P(A)}{P(B)}}\," /></a>


6. *Kolmogorov axioms*

- first axiom

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(A)\in\mathbb{R},&space;P(A)\geq&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(A)\in\mathbb{R},&space;P(A)\geq&space;0" title="P(A)\in\mathbb{R}, P(A)\geq 0" /></a>


- second axiom

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(\Omega)&space;=&space;1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;P(\Omega)&space;=&space;1" title="P(\Omega) = 1" /></a>


- third axiom

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;A&space;\cap&space;B&space;=&space;\emptyset&space;\;\;\;&space;\rightarrow&space;\;\;\;&space;P(A&space;\cup&space;B)&space;=&space;P(A)&space;&plus;&space;P(B)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;A&space;\cap&space;B&space;=&space;\emptyset&space;\;\;\;&space;\rightarrow&space;\;\;\;&space;P(A&space;\cup&space;B)&space;=&space;P(A)&space;&plus;&space;P(B)" title="A \cap B = \emptyset \;\;\; \rightarrow \;\;\; P(A \cup B) = P(A) + P(B)" /></a>


***
*Reference*


[Probability - wikipedia](https://en.wikipedia.org/wiki/Probability)

[Probability Axioms- wikipedia](https://en.wikipedia.org/wiki/Probability_axioms)

