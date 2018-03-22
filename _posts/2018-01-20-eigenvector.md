---
layout: post
title: Eigenvalue Decomposition
tags: [Math]
---
**Eigenvalue**
<br/>
$$Av = \lambda v$$
<br/>
![alt text](/assets/img/eigenvalue.png)


===Eigenvalues of geometric transformations===
The following table presents some example transformations in the plane along with their 2×2 matrices, eigenvalues, and eigenvectors.
{| class="wikitable" style="text-align:center; margin:1em auto 1em auto;"
|
! [[Scaling (geometry)|scaling]]
! unequal scaling
! [[Rotation (geometry)|rotation]]
! [[Shear mapping|horizontal shear]]
! [[hyperbolic rotation]]
|-
! illustration
|| [[File:Homothety in two dim.svg|100px|alt=Equal scaling ([[Homothetic transformation|homothety]])]]
|| [[File:Unequal scaling.svg|100px|alt=Vertical shrink and horizontal stretch of a unit square.]]
|| [[File:Rotation.png|100px|alt=Rotation by 50 degrees]]
|| [[File:Shear.svg|100px|center|alt=Horizontal shear mapping]]
|| [[File:Squeeze r=1.5.svg|100px]]
|- style="vertical-align:top"
! matrix
| <math> \begin{bmatrix}k & 0\\0 & k\end{bmatrix}</math>
| <math> \begin{bmatrix}k_1 & 0\\0 & k_2\end{bmatrix}</math>
| <math> \begin{bmatrix}c & -s \\ s & c\end{bmatrix} </math><br /><math>c=\cos\theta</math><br /><math>s=\sin\theta</math>
| <math> \begin{bmatrix}1 & k\\ 0 & 1\end{bmatrix} </math>
| <math>\begin{bmatrix} c & s \\ s & c \end{bmatrix}</math><br /><math>c=\cosh \varphi</math><br /><math>s=\sinh \varphi</math>
|-
! characteristic<br />polynomial
| <math>\ (\lambda - k)^2</math>
| <math>(\lambda - k_1)(\lambda - k_2)</math>
| <math>\lambda^2 - 2c\lambda + 1</math>
| <math>\ (\lambda - 1)^2</math>
| <math>\lambda^2 - 2c\lambda + 1</math>
|-
! eigenvalues <math>\lambda_i</math>
|<math>\lambda_1 = \lambda_2 = k</math>
|<math>\lambda_1 = k_1</math><br /><math>\lambda_2 = k_2</math>
|<math>\lambda_1 = e^{\mathbf{i}\theta}=c+s\mathbf{i}</math><br /><math>\lambda_2 = e^{-\mathbf{i}\theta}=c-s\mathbf{i}</math>
|<math>\lambda_1 = \lambda_2 = 1</math>
|<math>\lambda_1 = e^\varphi</math><br /><math>\lambda_2 = e^{-\varphi}</math>,
|-
! algebraic multipl.<br /><math>\mu_i=\mu(\lambda_i)</math>
|<math>\mu_1 = 2</math>
|<math>\mu_1 = 1</math><br /><math>\mu_2 = 1</math>
|<math>\mu_1 = 1</math><br /><math>\mu_2 = 1</math>
|<math>\mu_1 = 2</math>
|<math>\mu_1 = 1</math><br /><math>\mu_2 = 1</math>
|-
! geometric multipl.<br /><math>\gamma_i = \gamma(\lambda_i)</math>
|<math>\gamma_1 = 2</math>
|<math>\gamma_1 = 1</math><br /><math>\gamma_2 = 1</math>
|<math>\gamma_1 = 1</math><br /><math>\gamma_2 = 1</math>
|<math>\gamma_1 = 1</math>
|<math>\gamma_1 = 1</math><br /><math>\gamma_2 = 1</math>
|-
! eigenvectors
|All non-zero vectors
|<math> \begin{align} u_1 & = \begin{bmatrix}1\\0\end{bmatrix} \\ u_2 & = \begin{bmatrix}0\\1\end{bmatrix} \end{align}</math>
|<math>\begin{align} u_1 & = \begin{bmatrix}{\ }1\\-\mathbf{i}\end{bmatrix} \\ u_2 & = \begin{bmatrix}{\ }1\\ +\mathbf{i}\end{bmatrix} \end{align}</math>
|<math>u_1 = \begin{bmatrix}1\\0\end{bmatrix}</math>
|<math> \begin{align} u_1 & = \begin{bmatrix}{\ }1\\{\ }1\end{bmatrix} \\ u_2 & = \begin{bmatrix}{\ }1\\-1\end{bmatrix}. \end{align}</math>
|}

Note that the characteristic equation for a rotation is a [[quadratic equation]] with [[discriminant]] <math>D = -4(\sin\theta)^2</math>, which is a negative number whenever {{mvar|&theta;}} is not an integer multiple of 180°. Therefore, except for these special cases, the two eigenvalues are complex numbers, <math>\cos\theta \pm \mathbf{i}\sin\theta</math>; and all eigenvectors have non-real entries. Indeed, except for those special cases, a rotation changes the direction of every nonzero vector in the plane.

A linear transformation that takes a square to a rectangle of the same area (a [[squeeze mapping]]) has reciprocal eigenvalues.

![alt text](/assets/img/eigenvalue_table.png)
