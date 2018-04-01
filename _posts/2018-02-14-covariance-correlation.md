---
layout: post
title: covariance & correlation
tags: [Math]
---

**sample covariance**
>

<br/>
$$
s_{xy} = \dfrac{1}{N}\sum_{i=1}^{N} (x_i-m_x)(y_i-m_y)
$$


**sample correlation coefficient**
>

<br/>
$$
r_{xy} = \dfrac{s_{xy}}{\sqrt{s^2_{x} \cdot s^2_{y}}}
$$
<br/>

**covariance**
>

$$
\text{Cov}[X, Y] = \text{E}[(X - \text{E}[X])(Y - \text{E}[Y])]
$$

**correlation coefficient**
>

$$
\rho[X,Y] =  \dfrac{\text{Cov}[X, Y]}{\sqrt{\text{Var}[X] \cdot \text{Var}[Y]}}
$$


**sample covariance matrix**
>


S =
$$
\begin{bmatrix}
\begin{eqnarray}
s_{x_1}^2     \;\;  &  s_{x_1x_2} \;\;&  s_{x_1x_3} \;\;&  \cdots &  s_{x_1x_M} \\
s_{x_1x_2}   \;\;    &  s_{x_2}^2 \;\;&  s_{x_2x_3} \;\;&  \cdots &  s_{x_2x_M} \\
\vdots       &  \vdots &  \vdots &  \ddots &  \vdots \\
s_{x_1x_M}   \;\;    &  s_{x_2x_M} \;\;&  s_{x_3x_M} \;\;&  \cdots &  s_{x_M}^2 \\
\end{eqnarray}
\end{bmatrix}
$$


**sample covariance matrix**
>


$$\Sigma = \text{Cov}[X] = \text{E} \left[ (X - \text{E}[X])(X - \text{E}[X])^T \right]$$ =
$$\begin{bmatrix}
\begin{eqnarray}
\sigma_{x_1}^2     \;\;  &  \sigma_{x_1x_2} \;\;&  \sigma_{x_1x_3} \;\;&  \cdots &  \sigma_{x_1x_M} \\
\sigma_{x_1x_2}   \;\;    &  \sigma_{x_2}^2 \;\;&  \sigma_{x_2x_3} \;\;&  \cdots &  \sigma_{x_2x_M} \\
\vdots       &  \vdots &  \vdots &  \ddots &  \vdots \\
\sigma_{x_1x_M}   \;\;    &  \sigma_{x_2x_M} \;\;&  \sigma_{x_3x_M} \;\;&  \cdots &  \sigma_{x_M}^2 \\
\end{eqnarray}
\end{bmatrix}
$$







***
*Reference*
