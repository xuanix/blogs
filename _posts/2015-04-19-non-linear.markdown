---
layout:     post
title: "Support vector machine #4 - Classification with Non-linear Decision Boundaries"
subtitle:   "Support vector machine solves non-linear decision boundaries"
author: "Tianxiang Gao"
date: "April 19, 2015"
header-img: "img/post-bg-02.jpg"
---

In this blog, I am gonna introduce how support vector machine seeks the separating hyperplane
when the decision boundary is **non-linear**.

## Finding Separating Hyperplane for Non-linear decision boundary
When we are dealing with **linearly non-separable** case, we find an separating hyperplane by solving an optimization problem

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\newline 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq M - \xi_i, \; i = 1, \ldots, n. \newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C
\end{align}

Once the optimization problem is solved and get $\hat{\beta}$ and $\hat{\beta}_0$, we obtain the decision function (or classifier) as

\begin{align}
	G(x) = & sign[\hat{f}(x)] \nonumber \newline
         = & sign[\hat{\beta}^Tx + \hat{\beta}_0]
\end{align}

It is obvious that the decision function $\hat{f}(x)$ is a linear function. The performance of $\hat{f}(x)$ can suffer when there is non-linear relationship between predictors and the outcome, such as the figure below.

<img src="{{site.baseurl}}/img/svm/nonlinear.png">

You can find the figures from the website of [Introduction to Statistical Learning (**ISL**)][#1]. In order to deal with nonlinear case, we consider enlarging the feature space by using **functions of the predictors** such as quadratic and cubic term. Assume the original feature space is $p$-dimention, $X_1, X_2, ..., X_P$. Then we introduce quadratic terms to enlarge the feature space to $2p$-dimention, $X_1, X_1^2, X_2, X_2^2, ..., X_P, X_P^2$. Hence, $\beta = \begin{bmatrix}\beta_1\newline \beta_2\end{bmatrix}$, and the optimization problem can be reformulated as 

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\newline 
& \text{subject to}
& & y_i(\beta_1^Tx_i+\beta_2^Tx^2_i+\beta_0) \geq M - \xi_i, \; i = 1, \ldots, n. \newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C
\end{align}

One could enlarge the feature space by introducing higher-order dimension and intersection temrs, and even not polynomial terms. So we can define a mapping (or function)

\begin{equation}
    h(x): \mathbb{R}^p \rightarrow \mathbb{R}^H
\end{equation}

which maps the feature space from $p$-dimension to $H$-dimension. Thus, the optimization problem can be reformulated as 

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\newline 
& \text{subject to}
& & y_i[\beta^Th(x_i)+\beta_0] \geq M - \xi_i, \; i = 1, \ldots, n. \label{12}\newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C
\end{align}

Such as [previous blog](http://gaotx.com/blogs/2015/04/18/soft-margin/#), in order to solve this problem, we reformulated this optimization by droping <span markdown="0">$||\beta||=1$</span> and replacing \eqref{12} with 

$$ \cfrac{1}{||\beta||}y_i[\beta^Th(x_i)+\beta_0] \geq M(1-\xi_i) $$

Since for any $\beta$ and $\beta_0$ satisfying the inequation, its scaled positively multiple also satisifies the inequation, we just arbitrarily assume 
$ M = \cfrac{1}{||\beta||} $. Then, we minimizing 
$\cfrac{1}{2}\beta^T\beta$ for easier solving. 

<div markdown="0">
\begin{align} 
&\underset{\beta,\beta_0}{\text{min}}& & \cfrac{1}{2}\beta^T\beta\newline 
& \text{subject to}
& & y_i[\beta^T h(x)_i+\beta_0] \geq 1 - \xi_i, \; i = 1, \ldots, n. \newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C\label{18}
\end{align}
</div>

We drop the "budget" constrain \eqref{18} by introducing it to the objective function and obtain the final formulation

\begin{align} 
&\underset{\beta,\beta_0}{\text{min}}& & \cfrac{1}{2}\beta^T\beta+C\sum_{i=1}^{n}\xi_i\label{19}\newline 
& \text{subject to}
& & y_i[\beta^Th(x_i)+\beta_0] \geq 1 - \xi_i, \; i = 1, \ldots, n. \label{20}\newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\label{21}\newline
\end{align}

This is quadratic optimization problem. We drop the contrains \eqref{20} and \eqref{21} by multiplying the langrange multipliers $\alpha$ and $\mu$. Then lagrange function 

<div markdown="0">
\begin{align}
L_p(\beta, \beta_0,\xi; \alpha,\mu)= & \cfrac{1}{2}\beta^T\beta+C\sum_{i=1}^{n}\xi_i \nonumber \newline
	&-\sum_{i=1}^{n}\alpha_i\{y_i[\beta^Th(x_i)+\beta_0]-(1 - \xi_i)\}-\sum_{i=1}^{n}\mu_i\xi_i\label{14} 
\end{align}
</div>

Then the dual function is 

\begin{equation}
	g(\alpha, \mu) = min \,L_p(\beta, \beta_0,\xi; \alpha,\mu)
\end{equation}

In order to the minimum, set the partial derivatives to zero w.r.t $\beta, \beta_0$ and $\xi_i$, we obtain

<div markdown="0">
\begin{align}
&\frac{\partial L_p}{\partial \beta} \rightarrow \beta=\sum_{i=1}^{n}\alpha_iy_ih(x_i),\label{24}\newline
&\frac{\partial L_p}{\partial \beta} \rightarrow 0= \sum_{i=1}^{n}\alpha_iy_i,\label{25}\newline
&\frac{\partial L_p}{\partial \beta} \rightarrow \alpha_i=C-\mu_i, \label{26}\forall i.
\end{align}
</div>

By substituting \eqref{24}-\eqref{26} into \eqref{14}, we obtain 

<div markdown="0">
\begin{equation}
g(\alpha, \mu)
= \sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jh(x_i)^Th(x_j)
\end{equation}
</div>

which gives us a lower bound on the objective function\eqref{19}. Then the dual problem is 

<div markdown="0">
\begin{align}
&\underset{\alpha,\mu}{\text{max}}& & g(\alpha, \mu)
= \sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jh(x_i)^Th(x_j)\newline 
& \text{subject to}
& & \sum_{i=1}^{n}\alpha_iy_i=0 \newline
& & & \alpha_i + \mu_i = C, \; i = 1, \ldots, n.\label{22}\newline
& & & \alpha_i, \mu_i \geq 0, \; i = 1, \ldots, n.\newline
\end{align}
</div>

We can make it easier by droping variable $\mu$ and replacing constrain\eqref{22} with $\alpha_i\leq C$. Then we obtain

<div markdown="0">
\begin{align}
&\underset{\alpha}{\text{max}}& & 
\sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jh(x_i)^Th(x_j)\newline 
& \text{subject to}
& & \sum_{i=1}^{n}\alpha_iy_i=0 \newline
& & & 0 \leq \alpha_i \leq C, \; i = 1, \ldots, n.\newline
\end{align}
</div>

Any standard software can help us solve this problem. Substituting the optimal $\alpha^{*}$ into \eqref{24} obtains $\hat{\beta}$. In order to get $\hat{\beta_0}$, we use KKT conditions including the constrains

\begin{align}
	\alpha_i[y_i(\beta^Th(x_i)+\beta_0) -(1 - \xi_i)] = 0,\label{27}\newline
	\mu_i\xi_i=0,\label{28}
\end{align}

for $i=1,2, ..., n$. For any $\alpha_i>0$, associated observations are support vectors, and 
\begin{align}
	y_i(\beta^Th(x_i)+\beta_0) -(1 - \xi_i)=0, \newline
	\mu_i=C-\alpha_i>0,
\end{align}

based on \eqref{27} and \eqref{18}, respectively. Hence, $\xi_i=0$ because of KKT condition \eqref{28}. $\hat{\beta}_0$ can be obtained by 

$$ y_i(\hat{\beta}^Th(x_i)+\beta_0) =1$$

Typically we use an average of all the solutions for numerical stability. 

Given the solutions for $\hat{\beta}$ and $\hat{\beta}_0$, the decision function (or classifier) is 

\begin{align}
	G(x) = & sign[\hat{f}(x)] \nonumber \newline
		   & sign[\hat{\beta}^Th(x) + \hat{\beta}_0]
\end{align}

The tunning parameter $C$ can be obtained through *cross-validation*.

## Kernel Function

[#1]:http://www-bcf.usc.edu/~gareth/ISL/index.html