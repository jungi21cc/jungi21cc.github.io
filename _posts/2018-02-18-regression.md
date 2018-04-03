---
layout: post
title: Regression
tags: [Math]
---

**ordinary Least Square**

![alt text](/assets/img/ols_regression.png)

$$
{\displaystyle y_{i}=\beta _{1}x_{i1}+\beta _{2}x_{i2}+\cdots +\beta _{p}x_{ip}+\varepsilon _{i}}
$$

*Error sum of Square*

$$
{\displaystyle S(b)=\sum _{i=1}^{n}(y_{i}-x_{i}^{\mathrm {T} }b)^{2}=(y-Xb)^{\mathrm {T} }(y-Xb)}
$$

possesses a unique global minimum at ${\displaystyle b={\hat {\beta }}} b={\hat {\beta }}$, which can be given by the explicit formula

$$
{\displaystyle {\hat {\beta }}=\operatorname {argmin} _{b\in \mathbb {R} ^{p}}S(b)=\left({\frac {1}{n}}\sum _{i=1}^{n}x_{i}x_{i}^{\mathrm {T} }\right)^{\!-1}\!\!\cdot \,{\frac {1}{n}}\sum _{i=1}^{n}x_{i}y_{i}}
$$

*coefficient of determination R Square*

$$
{\displaystyle R^{2}={\frac {\sum ({\hat {y}}_{i}-{\overline {y}})^{2}}{\sum (y_{i}-{\overline {y}})^{2}}}={\frac {y^{\mathrm {T} }P^{\mathrm {T} }LPy}{y^{\mathrm {T} }Ly}}=1-{\frac {y^{\mathrm {T} }My}{y^{\mathrm {T} }Ly}}=1-{\frac {\rm {RSS}}{\rm {TSS}}}}
$$


***
*Reference*
