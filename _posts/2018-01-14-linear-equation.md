---
layout: post
title: Linear Equation
tags: [Math, Linear Algebra]
---
**System of linear equations**


*general form*
$$
{\displaystyle{\begin{aligned}a_{11}x_{1}&+a_{12}x_{2}+\cdots+a_{1n}x_{n}=b_{1}\\a_{21}x_{1}&+a_{22}x_{2}+\cdots+a_{2n}x_{n=b_{2}\\\vdots &\\a_{m1}x_{1}&+a_{m2}x_{2}+\cdots +a_{mn}x_{n}=b_{m},\end{aligned}}}
$$

*Vector Equation*

$$x_{1}{\begin{bmatrix}a_{11}\\a_{21}\\\vdots\\a_{m1}\end{bmatrix}}+x_{2}{\begin{bmatrix}a_{12}\\a_{22}\\\vdots \\a_{m2}\end{bmatrix}}+\cdots +x_{n}{\begin{bmatrix}a_{1n}\\a_{2n}\\\vdots\\a_{mn}\end{bmatrix}}={\begin{bmatrix}b_{1}\\b_{2}\\\vdots\\b_{m}\end{bmatrix}}$$

*Matrix equation*
<br/>
general matrix equation is ~~
<br/>

$$
A{\mathbf {x}}={\mathbf {b}}
$$
and this, <br/>


$$
A={\begin{bmatrix}a_{11}&a_{12}&\cdots &a_{1n}\\a_{21}&a_{22}&\cdots &a_{2n}\\\vdots &\vdots &\ddots &\vdots \\a_{m1}&a_{m2}&\cdots &a_{mn}\end{bmatrix}},\quad {\mathbf {x}}={\begin{bmatrix}x_{1}\\x_{2}\\\vdots \\x_{n}\end{bmatrix}},\quad {\mathbf {b}}={\begin{bmatrix}b_{1}\\b_{2}\\\vdots \\b_{m}\end{bmatrix}}
$$

*Inverse Matrix*

$$
A^{-1} = \dfrac{1}{\det A} C^T = \dfrac{1}{\det A}
\begin{bmatrix}
C_{1,1} &\cdots& C_{N, 1}\\
\vdots &\ddots &\vdots\\
C_{1,N} &\cdots &C_{N,N}\\
\end{bmatrix}
$$
<br/>
*Solution set*

A linear system may behave in any one of three possible ways:

- The system has infinitely many solutions.
- The system has a single unique solution.
- The system has no solution.


*Gaussian elimination*

3 row of operations:
Type 1: Swap the positions of two rows.
Type 2: Multiply a row by a nonzero scalar.
Type 3: Add to one row a scalar multiple of another.



*matrix solution*
$${\mathbf {x}}=A^{-1}{\mathbf {b}}$$
