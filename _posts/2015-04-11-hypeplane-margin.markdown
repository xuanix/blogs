---
layout:     post
title: "Suport vector machine #1 - Hyperplane and margin"
subtitle:   "Hyperplane and margin are the foundation for maximal margin classifiers and suppor vector machines."
author: "Tianxiang Gao"
date: "April 12, 2015"
header-img: "img/post-bg-01.jpg"
---

In this blog, I am gonan define *hyperplanes*, *margin*. The concept of Maximal margin classifier, *support vector classifier* and  *support vector machine* that will be covered in future blogs.

## Hyperplane
> In a *p*-dimensional space, a **hyperplane** is a flat affine subspace of dimension *p-1*.

The word **affine** indicates that the subspace need not pass through the origin. Mathmatic defination for *linear* is $\beta^Tx=0$, where $\beta$ and $x$ are both vectors. Mathmatic defination for *affine* is $\beta^Tx+\beta_0=0$, where $\beta_0$ is a real number. Because of $\beta_0$, *affine* does not pass through the origin.

Here is an example of hyperplane in two dimensional space, a hyperplane is just a line. The mathmatical definition of a two-dimensional hyperplan is the equation below

\begin{equation}
  \beta_0 + \beta_1X_1+\beta_2X_2 = 0
  \label{1}
\end{equation}

for parameters $\beta_0, \beta_1$, and $\beta_2$. We can see the equation above is an equation of a line and the data point $X = (X_1, X_2)^T$ is on that line. The equation can be easily extended to the p-dimensional hyperplane:

\begin{equation}
  \beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p = 0
  \label{2}
\end{equation}

A *p*-dimensional data point $X = (X_1, X_2, ..., X_P)^T$ would lie on that hyperplane if it satisfies the equation. So, a hyperplane naturally divides a *p*-dimensional space into two halves. If a data point is not on the hyperplane, it would lie on sides of that hyperplane, then it either satisifies 

\begin{equation}
	\beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p > 0 
\end{equation}

or 

\begin{equation}
	\beta_0 + \beta_1X_1+\beta_2X_2+...+\beta_pX_p < 0 
\end{equation}

One can easily determine on which side of hyperplane a data point lies by calculating the sign of the left hand side of \eqref{hyperplane}. We define it as a function of x, and and use $\beta=(\beta_1,\beta_2,...\beta_p)^T$,

\begin{equation}
  f(x)=\beta^Tx + \beta_0
  \label{f(x)}
\end{equation}

## Derivation of Marigin

Suppose a training dataset is **linearly separable**, and it is possbile to construct at lease a hyperplane that sperates all training observations perfectly. 

<img src="{{site.baseurl}}/img/svm/maximal-margin-classifier.png">

The figure above shows there are three possible hyperplanes, two dash line and one solid line, that are able to sperate all training data perfectly. However, if we only can pick one, which one should be selected? 

If we select the left dash line as separating hyperplane, and we are given a test observation which actually is from red class, and is very close to that red dot on the dash line but shifted a tiny bit up, this test observation would be misclassified. This could also happened if select the right dash line. So, the solid line is better. Furthermore, the solid line has the farthest (perpendicular) distance from nearest training observation, which distance is known as **margin**. We define the distance from the nearest training observations to the hyperplane as $M$, which is half of marigin. Those training observations are **suppor vectors**. In the figure above, there are three supor vectors, two green dots and one red dots.

<img src="{{site.baseurl}}/img/maximal-margin/margin.png">

Let's derivative of $M$. Suppose the class labels of green dots are $y_i = 1$, while red dots are $y_i = -1$, and $x_1 \in y_{+1}, x_2 \in y_{-1} $ are the nearest obersvations to the hyperplane. $\beta$ is the *normal vector* (not necessarily unit vector) to the hyperplane. Then the distance, $M$, from $x_1$ to hyperplane is

\begin{align} 
M& =\cfrac{\beta^T}{||\beta||}(x_1-x_0)\\\\ 
\label{7}
& =\cfrac{1}{||\beta||}(\beta^Tx_1-\beta^Tx_0)\\\\ 
\label{8}
& =\cfrac{1}{||\beta||}(\beta^Tx_1+\beta_0) 
\end{align}

From \eqref{7} to \eqref{8}, because $x_0$ lies on the hyperplane, it satisifies the equations \eqref{1} and \eqref{2}. We substitudes $\beta^Tx_0$ with $-\beta_0$. On the other hand, the distance from $x_2$ to the hypeplane is 

\begin{align} 
M& =\cfrac{\beta^T}{||\beta||}(x_0-x_2)\\\\ 
& =\cfrac{1}{||\beta||}(\beta^Tx_0-\beta^Tx_2)\\\\ 
& =\cfrac{1}{||\beta||}(-\beta_0-\beta^Tx_2)\\\\ 
& =-\cfrac{1}{||\beta||}(\beta^Tx_2+\beta_0) 
\end{align}  

Based on our assumption, $x_1 \in y_{+1}, x_2 \in y_{-1}$, $M$ can be reformulated for any support vector $x$ as

\begin{equation}
	M =\cfrac{y(\beta^Tx+\beta_0)}{||\beta||}
	\label{hypeplane}
\end{equation}

So the margin is just $2M$. However, in computer science community, people always use $w$ and $b$ to representative $\beta$ and $\beta_0$, so the defination of hyperplane is 

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
