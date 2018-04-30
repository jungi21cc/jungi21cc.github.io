---
layout: post
title: Dummy variable data analysis
tags: [Math]
---

### 1. *Incorporating a dummy variable*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;\ln&space;{\text{wage}}=\alpha&space;_{0}&plus;\delta&space;_{0}{\text{female}}&plus;\alpha&space;_{1}{\text{education}}&plus;u}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;\ln&space;{\text{wage}}=\alpha&space;_{0}&plus;\delta&space;_{0}{\text{female}}&plus;\alpha&space;_{1}{\text{education}}&plus;u}" title="{\displaystyle \ln {\text{wage}}=\alpha _{0}+\delta _{0}{\text{female}}+\alpha _{1}{\text{education}}+u}" /></a>

![alt text](/assets/img/one_dummy.jpg)


### 2. *dummy variable from ANCOVA (Analysis of Covariance) models*

Yi = α1 + α2D2i + α3D3i + α4Xi + Ui


where,

Yi = average annual salary of public school teachers in state i
Xi = State expenditure on public schools per pupil
D2i = 1, if the State i is in the North Region
D2i = 0, otherwise
D3i = 1, if the State i is in the South Region
D3i = 0, otherwise


Say the regression output for this model is

Ŷi = 13,269.11 − 1673.514D2i − 1144.157D3i + 3.2889Xi


![alt text](/assets/img/400px-Ancova_graph.jpg)


***
*Reference*

[polynomial regression - wikipedia](https://en.wikipedia.org/wiki/Dummy_variable_(statistics))
