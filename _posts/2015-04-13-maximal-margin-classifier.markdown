---
layout:     post
title: "Suport vector machine #2 - Maximal margin classifier"
subtitle:   "Suport vector machine is an extention of a simple and intuitive classifier called the maximal margin classifier"
author: "Tianxiang Gao"
date: "April 13, 2015"
header-img: "img/post-bg-02.jpg"
---

In this blog, I am gonna cover the derivation of **maximal marigin classifier**. In order to understand the process, you need to know *KKT condition*, and *Lagrange dual*. If you are not so familier with those knowledge, please check another [blog](http://gaotx.com/blogs/2015/04/12/maximal-margin-classifier/).

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
is not realy constraint, since if $\beta^Tx+\beta_0=0$ defines a separating hyperplane, so does $k(\beta^Tx+\beta_0)=0$ for any $k\neq0$.