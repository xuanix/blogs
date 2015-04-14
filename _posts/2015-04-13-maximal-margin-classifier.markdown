---
layout:     post
title: "Suport vector machine #2 - Maximal margin classifier"
subtitle:   "Suport vector machine is an extention of a simple and intuitive classifier called the maximal margin classifier"
author: "Tianxiang Gao"
date: "April 13, 2015"
header-img: "img/post-bg-02.jpg"
---

In this blog, I am gonna cover the derivation of **maximal marigin classifier**. In order to understand the process, you need to know *KKT condition*, and *Lagrange dual*. If you are not so familier with those knowledge, please check another [blog](http://gaotx.com/blogs/2015/04/14/kkt-cvx/).

<img src="{{site.baseurl}}/img/maximal-margin/maximal-margin-classifier.png">

In the [previous blog](http://gaotx.com/blogs/2015/04/12/maximal-margin-classifier/), we derived the formulation of $M$, which is half of margin.

\begin{equation}
	M =\cfrac{y(\beta^Tx+\beta_0)}{||\beta||}
	\label{hypeplane}
\end{equation}

We want to find maximal margin hyperplane that has the farthest distances from observations. This separating hyperplane is just the solution to the optimization problem.

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\\\\ 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq M, \; i = 1, \ldots, n.
\end{align}

The set of constraints ensure that every observations are at least a signed distance $M$ from the hyperplane which is defined as $\beta^Tx+\beta_0=0$. Note that 
$
	||\beta||=1
$
is not realy constraint, since if $\beta^Tx+\beta_0=0$ defines a separating hyperplane, so does $k(\beta^Tx+\beta_0)=0$ for any $k\neq0$. However, it adds meaning to constraint (3), because of $\beta$ has been normalized, constraint (3) is the perpendicular distance from the *i*th observation to the hyperplane. On the other hand we can get rid of it by replacing the constrain (3) with 

$$ \cfrac{1}{||\beta||}y_i(\beta^Tx_i+\beta_0) \geq M $$

(which redefines $\beta_0$) or equivalently

$$ y_i(\beta^Tx_i+\beta_0) \geq M||\beta|| $$

Since for any $\beta_0$ and $\beta$ satisfying these inequaties, any positively scaled multiple satisfies them too, just like define a separating hyperplane, we can arbitrarily set $||\beta|| = 1/M$. Thus (2)-(3) is equivalent to 

\begin{align} 
&\underset{\beta,\beta_0}{\text{max}}& & \cfrac{1}{||\beta||}\\\\ 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq 1, \; i = 1, \ldots, n.
\end{align}

Maximizing $1/||\beta||$ is equivalent to minimizing $||\beta||$, and further minimizing
 $
 \cfrac{1}{2}||\beta||^2= \cfrac{1}{2}\beta^T\beta
 $
 for mathmatic convinence. Thus, (4)-(5) is equivalent to

 \begin{align} 
&\underset{\beta,\beta_0}{\text{min}}& & \cfrac{1}{2}\beta^T\beta \newline 
\label{primal:contraint}
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq 1, \; i = 1, \ldots, n.
\end{align}

This is a convex optimization problem (quadratic criterion with linear inequality constraints). The Lagrange (primal) function is 

\begin{equation}
  L(\beta,\beta_0,\alpha)=\cfrac{1}{2}\beta^T\beta-\sum_{i=1}^{n}\alpha_i[y_i(\beta^Tx_i+\beta_0)-1]
  \label{primal}
\end{equation}

and the corresponding dual function is 

\begin{equation}
	g(\alpha) = \underset{\alpha}{\text{min}}\;L(\beta,\beta_0,\alpha)
	\label{dual:function1}
\end{equation}

In order to calculate the minimum of $L(\beta,\beta_0,\alpha)$, as it is quadratic function, we can set the partial derivatives for $\beta$ and $\beta_0$ to zero, we obtain:

\begin{equation}
	\beta = \sum_{i=1}^{n}\alpha_iy_ix_i
	\label{beta}
\end{equation}

\begin{equation}
	0 = \sum_{i=1}^{n}\alpha_iy_i
	\label{0}
\end{equation}

and substituing these in \eqref{primal} and \eqref{dual:function1}, we obtain the dual function

\begin{equation}
	g(\alpha) = \sum_{i=1}^{n}\alpha_1-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{k=1}^{n}\alpha_i\alpha_ky_iy_kx_i^Tx_k
	\label{dual:function2}
\end{equation}

The dual problem is 
 \begin{align} 
&\underset{\alpha}{\text{max}}& & g(\alpha) = \sum_{i=1}^{n}\alpha_1-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{k=1}^{n}\alpha_i\alpha_ky_iy_kx_i^Tx_k\newline
\label{alpha}
& \text{subject to}
& & \alpha_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\alpha_iy_i = 0
\end{align}

The solution is obtained by solving this optimal problem through a standard software. In addition, the solution  satisfy the Karush-Kuhn-Tucher conditions, which include \eqref{primal:contraint},\eqref{beta}, \eqref{0}, \eqref{alpha} and

\begin{equation}
	\alpha_i[y_i(\beta^Tx_i+\beta_0)-1]=0, \forall i.
	\label{16}
\end{equation}

From these we can see that,

* If $\alpha>0$, then $y_i(\beta^Tx_i+\beta_0)=1$, in other words, $x_i$ is support vector;
* If $\alpha=0$, then $y_i(\beta^Tx_i+\beta_0)>1$, and $x_i$ is not support vector.

Solving the optimal problem obtains the values of $\alpha$. $\beta$ and $\beta_0$ can be obtained by \eqref{beta} and \eqref{16} for any suppor vectors.

The optimal separating hyperplane produces a function 
$
\hat{f}(x)=\hat{\beta}^Tx+\hat{\beta}_0
$
for classifying new test observations: 
\begin{equation}
	\hat{G} = sign\; \hat{f}(x)
	\label{17}
\end{equation}

Although none of the training observations fall in the margin, this will not necessarily the case for test observations. The intuition is that a large margin on the training data will lead to good separation on the test data. The higher the magnitude of $\hat{f}(x)$, the more confidence we are to classify the test observations.
