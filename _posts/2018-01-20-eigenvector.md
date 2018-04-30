---
layout: post
title: Eigenvalue Decomposition
tags: [Math]
---

### 1. *Eigenvalue and Eigenvector*

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;Av=\lambda&space;v}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;Av=\lambda&space;v}" title="{\displaystyle Av=\lambda v}" /></a>

![alt text](/assets/img/eigenvalue.png)

### 2. *Example*

#### - Matrix A

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;A={\begin{bmatrix}2&1\\1&2\end{bmatrix}&space;=&space;0}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;A={\begin{bmatrix}2&1\\1&2\end{bmatrix}&space;=&space;0}}" title="{\displaystyle A={\begin{bmatrix}2&1\\1&2\end{bmatrix} = 0}}" /></a>

#### - Determinant of A

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;{\begin{aligned}|A-\lambda&space;I|&=\left|{\begin{bmatrix}2&1\\1&2\end{bmatrix}}-\lambda&space;{\begin{bmatrix}1&0\\0&1\end{bmatrix}}\right|={\begin{vmatrix}2-\lambda&space;&1\\1&2-\lambda&space;\end{vmatrix}}=3-4\lambda&space;&plus;\lambda&space;^{2}.\end{aligned}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;{\begin{aligned}|A-\lambda&space;I|&=\left|{\begin{bmatrix}2&1\\1&2\end{bmatrix}}-\lambda&space;{\begin{bmatrix}1&0\\0&1\end{bmatrix}}\right|={\begin{vmatrix}2-\lambda&space;&1\\1&2-\lambda&space;\end{vmatrix}}=3-4\lambda&space;&plus;\lambda&space;^{2}.\end{aligned}}}" title="{\displaystyle {\begin{aligned}|A-\lambda I|&=\left|{\begin{bmatrix}2&1\\1&2\end{bmatrix}}-\lambda {\begin{bmatrix}1&0\\0&1\end{bmatrix}}\right|={\begin{vmatrix}2-\lambda &1\\1&2-\lambda \end{vmatrix}}=3-4\lambda +\lambda ^{2}.\end{aligned}}}" /></a>

#### - λ = 1 and λ = 3

![alt text](/assets/img/Eigenvectors.gif)

#### - when λ = 1

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;(A-I)v_{\lambda&space;=1}={\begin{bmatrix}1&1\\1&1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}.}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;(A-I)v_{\lambda&space;=1}={\begin{bmatrix}1&1\\1&1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}.}" title="{\displaystyle (A-I)v_{\lambda =1}={\begin{bmatrix}1&1\\1&1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}.}" /></a>

#### - purple Vector = eigenvector

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;v_{\lambda&space;=1}={\begin{bmatrix}1\\-1\end{bmatrix}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;v_{\lambda&space;=1}={\begin{bmatrix}1\\-1\end{bmatrix}}}" title="{\displaystyle v_{\lambda =1}={\begin{bmatrix}1\\-1\end{bmatrix}}}" /></a>

#### - when λ = 3

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;(A-3I)v_{\lambda&space;=3}={\begin{bmatrix}-1&1\\1&-1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;(A-3I)v_{\lambda&space;=3}={\begin{bmatrix}-1&1\\1&-1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}}" title="{\displaystyle (A-3I)v_{\lambda =3}={\begin{bmatrix}-1&1\\1&-1\end{bmatrix}}{\begin{bmatrix}v_{1}\\v_{2}\end{bmatrix}}={\begin{bmatrix}0\\0\end{bmatrix}}}" /></a>

#### - blue Vector = eigenvector

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;{\displaystyle&space;v_{\lambda&space;=3}={\begin{bmatrix}1\\1\end{bmatrix}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;{\displaystyle&space;v_{\lambda&space;=3}={\begin{bmatrix}1\\1\end{bmatrix}}}" title="{\displaystyle v_{\lambda =3}={\begin{bmatrix}1\\1\end{bmatrix}}}" /></a>


3. *Eigenvalue of geometric transformation*
![alt text](/assets/img/eigenvalue_table.png)



***
*Reference*

[Eigenvalues and eigenvectors - wikipedia](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)
