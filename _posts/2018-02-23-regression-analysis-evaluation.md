---
layout: post
title: Regression Analysis Regularization
tags: [Math]
---

### 1. *Ridge*

L2 regularization

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda&space;\sum&space;w_i^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda&space;\sum&space;w_i^2" title="\text{cost} &=& \sum e_i^2 + \lambda \sum w_i^2" /></a>

### 2. *LASSO (Least Absolute Shrinkage and Selection Operator)*

L1 regularization

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda&space;\sum&space;|&space;w_i&space;|" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda&space;\sum&space;|&space;w_i&space;|" title="\text{cost} &=& \sum e_i^2 + \lambda \sum | w_i |" /></a>


### 3. *ridge and lasso geographical understanding*

#### - finding RSS as constraint satisfied

![alt text](/assets/img/L1_and_L2_balls.svg.png)

#### - finding RSS when gamma(constraint) changes

![alt text](/assets/img/jne427232f9_online.jpg)

#### - linear regression vs ridge regression

![alt text](/assets/img/normal_ridge.png)



### 4. *Elastic Net*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda_1&space;\sum&space;|&space;w_i&space;|&space;&plus;&space;\lambda_2&space;\sum&space;w_i^2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\text{cost}&space;&=&&space;\sum&space;e_i^2&space;&plus;&space;\lambda_1&space;\sum&space;|&space;w_i&space;|&space;&plus;&space;\lambda_2&space;\sum&space;w_i^2" title="\text{cost} &=& \sum e_i^2 + \lambda_1 \sum | w_i | + \lambda_2 \sum w_i^2" /></a>

![alt text](/assets/img/elasticnet.png)


***
*Reference*
