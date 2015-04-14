---
layout:     post
title: "Suport vector machine #1 - hyperplane and margin"
subtitle:   "Suport vector machine is an extention of a simple and intuitive classifier called the maximal margin classifier"
author: "Tianxiang Gao"
date: "April 12, 2015"
header-img: "img/post-bg-01.jpg"
---

In this blog, I am gonan define *hyperplanes*, *margin*. The concept of Maximal margin classifier, *support vector classifier* and  *support vector machine* that will be covered in future blogs.

## Hyperplane
> In a p-dimensional space, a *hyperplane* is a flat affine subspace of dimension p-1.

The word *affine* indicates that the subspace need not pass through the origin. For example, in two dimensional space, a hyperplane is just a line. The mathmatical definition of a two-dimensional hyperplan is the equation below

\begin{equation}
  \beta_0 + \beta_1X_1+\beta_2X_2 = 0
\end{equation}

for parameters $\beta_0, \beta_1$, and $\beta_2$. We can see the equation above is an equation of a line and the data point $X = (X_1, X_2)^T$ is on that line (or hyperplane). The equation can be easily extended to the p-dimensional hyperplane:

\begin{equation}
  \beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p = 0
  \label{hyperplane}
\end{equation}

A *p*-dimensional data point $X = (X_1, X_2, ..., X_P)^T$ would lie on that hyperplane if it satisfies the equation. So, a hyperplane naturally divides a *p*-dimensional space into two halves. If a data point is not on the hyperplane, it would lie on sides of that hyperplane, then it either satisifies 

\begin{equation}
	\beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p > 0 
\end{equation}

or 

\begin{equation}
	\beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p < 0 
\end{equation}

One can easily determine on which side of hyperplane a data point lies by calculating the sign of the left hand side of \eqref{hyperplane}. 

## Derivation of Marigin

Suppose a training dataset is **linearly separable**, and it is possbile to construct a hyperplane that sperates all training observations perfectly. We can compute the perpendicular distance from each training observation to that separating hyperplane.

> the smallest such distance is the minimal distance from the observations to the hyperplane, and is known as the margin. 

<img src="{{site.baseurl}}/img/maximal-margin-classifier.png">

The figure above shows two classes have been sperarated by three different but parallel hyperplanes. The solid line is the optimal (separating) hyperplane. The \eqref{hyperplane} can be wrote in vector format

\begin{equation}
	\beta^Tx+\beta_0 = 0
	\label{hp} 
\end{equation}


Let's calculate the smallest distance $M$ from the seprating hyperplane to observations. Suppose the class labels of green data points are $y_i = 1$, while red point's are $y_i = -1$, and a data point $x \in y_{+1} $. $\beta$ is the normal vector (not necessarily unit vector) to the hyperplane. We projected $x$ on to the hyperplane and get data point $ x^* $. Data points $x$ and $ x^* $ has a relationship that can be descripted into the follwoing equation

\begin{equation}
	x^* = x - M\cfrac{\beta}{||\beta||}
	\label{project}
\end{equation}

Since $ x^* $ lies on that hyperplane. On another words, it satisfies \eqref{hp}. Substitudes $ x^* $ with \eqref{project}:

\begin{equation}
	\beta^T(x - M\cfrac{\beta}{||\beta||})+\beta_0=0
\end{equation}

Since 
$
	\beta^T\beta=||\beta||^2
$
, we can get the equation for $M$, that is 

\begin{equation}
	M=\cfrac{\beta^Tx+\beta_0}{||\beta||}
\end{equation}

Based on our assumption, $ x\in y_{+1} $, if $x \in y_{-1}$, then the signs of equations are reversed: 

\begin{equation}
	x^*=x+M\cfrac{\beta}{||\beta||}
\end{equation}

\begin{equation}
	\beta^T(x + M\cfrac{\beta}{||\beta||})+\beta_0=0
\end{equation}

\begin{equation}
	M=\cfrac{-(\beta^Tx+\beta_0)}{||\beta||}
\end{equation}

On the other hand, we can take advantages of signs of $y_i$, then the final equations are 

\begin{equation}
	x^*=x-yM\cfrac{\beta}{||\beta||}
\end{equation}

\begin{equation}
	\beta^T(x - yM\cfrac{\beta}{||\beta||})+\beta_0=0
\end{equation}

\begin{equation}
	M=\cfrac{y(\beta^Tx+\beta_0)}{||\beta||}
\end{equation}

That is derivation of margin. However, in computer science community, people always use $w$ and b to representative $\beta$ and $\beta_0$, so the defination of hyperplane is 

\begin{equation}
	w^Tx+b = 0
	\label{cs:hp} 
\end{equation}

Furthermore, the defination of margin is changed to

\begin{equation}
	M=\cfrac{y(w^Tx+b)}{||w||}
\end{equation}

The reason why in the figure
$
M=\cfrac{1}{||\beta||}
$
is that people assumes the equations of those two dashed lines have the property $ y(w^Tx+b)=1 $, which will be explaned in detail in further blogs.


[1]: http://www-bcf.usc.edu/~gareth/ISL/
[2]: http://statweb.stanford.edu/~tibs/ElemStatLearn/