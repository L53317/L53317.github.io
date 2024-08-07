---
title: 'Marginalization in optimization problems'
date: 2024-08-05
permalink: /posts/2024/08/marginalization/
tags:
  - cool posts
  - category1
  - category2
---


A general optimization problem can be described as

$$ \min_{x}{||y- f(x)||^2_W} $$

or a general linear form

$$\min_{x}{||y- Ax||^2_W} $$

where $x$ is the optimization variable to be estimated, $y$ is the observation, and $W$ is the weighted matrix. 

Consider a special case with $W=I$. One of the most popular algorithms to solve this problem is to construct a residual for each observation or constraint, and it can be described as 

$$\min_{x} \frac{1}{2} \sum_{i=1}^N ||y_i-f_i(x_i)||$$

where $N$ is the number of constraints. The number of constraints becomes very large when a large number of measurements are observed. As a result, the optimization problem becomes a large-scale problem that is difficult to solve in real-time. A strategy to solve this problem is to apply a sliding window to restrict the number of residual blocks. A general method to implement a sliding window with preserves all previous information is marginalization. The marginalization can be described with Schur's complement. Consider the variable $x= \left[x_m, x_r \right]^T $, where $x_m$ is the variable part to be marginalized out, and $x_r$ is the remaining part. The problem can be described as

$$\min_{x}{||y- Ax||^2}= \min \left[\left(y- Ax \right) ^T \left(y- Ax \right) \right],  $$

$$\left.\left[ \begin{array}{cc} A_m & A_c \\\ A_c^T & A_r \end{array} \right. \right] \left[ \begin{array}{c} x_m \\\ x_r\end{array}\right] = \left[\begin{array}{c} y_m \\\ y_r \end{array} \right],
$$

where 

$$A =\left.\left[ \begin{array}{cc} A_m & A_c \\\ A_c^T & A_r \end{array} \right. \right], y=\left[ \begin{array}{c} y_m \\\ y_r\end{array}\right],$$

and then

$$ \begin{bmatrix}I&0 \\\ -A_c^T A_m^{-1}&I\end{bmatrix} \left.\left[ \begin{array}{cc} A_m & A_c \\\ A_c^T & A_r \end{array} \right. \right] \left[ \begin{array}{c} x_m \\\ x_r\end{array}\right] = \begin{bmatrix}I&0 \\\ -A_c^T A_m^{-1}&I\end{bmatrix}  \left[\begin{array}{c} y_m \\\ y_r \end{array} \right],
$$

$$
\begin{bmatrix}A_m&A_c \\\ 0&A_r-A_c^TA_m^{-1}A_c\end{bmatrix}\begin{bmatrix} x_m \\\ x_r \end{bmatrix}= \begin{bmatrix} y_m \\\ y_r-A_c^TA_m^{-1}y_m \end{bmatrix},
$$

and finally, we have

$$ 
\left(A_r- A_c^T A_m^{-1} A_c \right) x_r= y_r-A_c^TA_m^{-1} y_m.
$$

Using the equation involving only $x_r$ above, we can marginalize out $x_m$ from the optimization problem. With $A_c^T A_m^{-1} A_c$ and $A_c^TA_m^{-1} y_m$ in the equation above, we can keep the information of $x_m$ as prior information when calculating $x_r$. From the above equation, we can calculate the prior information related to $x_m$. 
