---
layout:     post
title: "Derivation of Determinant of Square Matrix"
subtitle:   ""
author: "Tianxiang Gao"
date: "April 24, 2015"
header-img: "img/post-bg-02.jpg"
---

In this blog, we only consider **square** matricies. The properties we used or derivatived works for all square matrices. If some properties that can be extended to other kind of matrices, they will be noted. In addition, methods or formulas used in this blog can be reviewed in an online course, [Linear Algribra][#1] which is offered by [Prof. Gilbert Strang](http://www-math.mit.edu/~gs/) on [MIT Open Courseware](http://ocw.mit.edu/index.htm).

No introduction of previous knwoledge are here. Instead, I will just go through main steps, but whenever needs, the knowledge will be introduced.

Typically, we need to derivate 

\begin{equation}
 \frac{\partial }{\partial x}det(A)\label{1}
\end{equation}

where $A$ is a $n\times n$ square matrix and any such kind of matrix can be representative as

\begin{equation}
A_{n\times n} =
 \begin{bmatrix}
  a_{11} & a_{12} & \cdots & a_{1n} \newline
  a_{21} & a_{22} & \cdots & a_{2n} \newline
  \vdots  & \vdots  & \ddots & \vdots  \newline
  a_{n1} & a_{n2} & \cdots & a_{nn}
 \end{bmatrix}
 \end{equation}

 and each element $a_{ij}$ is a function of a scalar $x$. In \eqref{1}, $det(A)$ is to denote the **Determinant** of $A$ that is the product of *pivots* multiplying $(-1)^m$, where $m$ is the number of row exchanges. Sometimes, $det(A)$ write as <span markdown="0">$|A|$</span>, but I use $det(A)$ here.

## Multivariable Chain Rule

Let $x=x(t)$ and $y=y(t)$ be differentiable at $t$ and suppose that $z=f(x,y)$ is differentiable at the point $[x(t), y(t)]$. Then $z=f[x(t), y(t)]$ is differentiable and 

\begin{equation}
	\frac{\partial z}{\partial t}=\frac{\partial z}{\partial x}\frac{\partial x}{\partial t}
	+\frac{\partial z}{\partial y}\frac{\partial y}{\partial t}.
\end{equation}

The [proof](http://en.wikipedia.org/wiki/Chain_rule) is on the wiki for those who are interested in. Here, instead, we can just intuitively understand it through an example of transport. We have a batch of goods needed to be delivered from city $z$ to $t$. We decided to use two different ways to ship them, one is expedited shipping $x$, another is regular shipping $y$. Though the batch of goods have been divided into two parts and shipped through two different ways. However, at the end, we sum the goods and they are equal to the original.

Thus, we can condiser \eqref{1} to be divided into $n^2$ parts wrt each $a_{ij}$. Condiser each $a_{ij}$ as a method to deliver the part to $x$. Then, we obtain

\begin{equation}
 \frac{\partial }{\partial x}det(A) 
 = \sum_{i=1}^{n}\sum_{j=1}^{n}\frac{\partial det(A)}{\partial a_{ij}}\frac{\partial a_{ij}}{\partial x}\label{4}
\end{equation}

## Laplace Expansion:

Laplace expansion also is known as **Co-factor formula**
<div markdown="0">
\begin{align}
	det(A)=\sum_{j=1}^{n}a_{ij}C_{ij}
\end{align}
</div>

where the *co-factor* $C_{ij}$ is equal to $(-1)^{i+j}$ multiplying the determiant of the matrix that results from $A$ by removing the $i^{th}$ row and the $j^{th}$ column.

Thus, 
\begin{align}
	\frac{\partial det(A)}{\partial a_{ij}} = C_{ij}
\end{align}

So \eqref{4} is equal to 

<div markdown="0">
\begin{align}
 \frac{\partial }{\partial x}det(A) 
 &= \sum_{i=1}^{n}\sum_{j=1}^{n}\frac{\partial det(A)}{\partial a_{ij}}\frac{\partial a_{ij}}{\partial x}\nonumber \newline 
  &= \sum_{i=1}^{n}\sum_{j=1}^{n}C_{ij}\frac{\partial a_{ij}}{\partial x}\label{7}
\end{align}
</div>

## Adjugate matrix and Co-factor matrix
We can generate the **co-factor matrix of A**, denote $C$ by calculating all co-factor $C_{ij}$ and puting them into a matrix

\begin{equation}
C_{n\times n} =
 \begin{bmatrix}
  C_{11} & C_{12} & \cdots & C_{1n} \newline
  C_{21} & C_{22} & \cdots & C_{2n} \newline
  \vdots  & \vdots  & \ddots & \vdots  \newline
  C_{n1} & C_{n2} & \cdots & C_{nn}
 \end{bmatrix}
 \end{equation}

We also extends $\frac{\partial A}{\partial x}$

\begin{equation}
\frac{\partial A}{\partial x} =
 \begin{bmatrix}
  \frac{\partial a_{11}}{\partial x} & \frac{\partial a_{12}}{\partial x} & \cdots & \frac{\partial a_{1n}}{\partial x} \newline
  \frac{\partial a_{21}}{\partial x} & \frac{\partial a_{22}}{\partial x} & \cdots & \frac{\partial a_{2n}}{\partial x} \newline
  \vdots  & \vdots  & \ddots & \vdots  \newline
  \frac{\partial a_{n1}}{\partial x} & \frac{\partial a_{n2}}{\partial x} & \cdots & \frac{\partial a_{nn}}{\partial x}
 \end{bmatrix}
\end{equation}

**Adjugate matrix** is the transpose of Co-factor matrix $C$ and denote as $adj(A)$.
We multiply $adj(A)$ by $\frac{\partial A}{\partial x}$ and obtain

\begin{equation}
adj(A)\frac{\partial A}{\partial x} =
 \begin{bmatrix}
  \sum_{i=1}^{n}C_{i1}\frac{\partial a_{i1}}{\partial x} & \sum_{i=1}^{n}C_{i1}\frac{\partial a_{i2}}{\partial x} & \cdots & \sum_{i=1}^{n}C_{i1}\frac{\partial a_{in}}{\partial x} \newline
  \sum_{i=1}^{n}C_{i2}\frac{\partial a_{i1}}{\partial x} & \sum_{i=1}^{n}C_{i2}\frac{\partial a_{i2}}{\partial x} & \cdots & \sum_{i=1}^{n}C_{i2}\frac{\partial a_{in}}{\partial x} \newline
  \vdots  & \vdots  & \ddots & \vdots  \newline
  \sum_{i=1}^{n}C_{in}\frac{\partial a_{i1}}{\partial x} & \sum_{i=1}^{n}C_{in}\frac{\partial a_{i2}}{\partial x} & \cdots & \sum_{i=1}^{n}C_{in}\frac{\partial a_{in}}{\partial x}
 \end{bmatrix}
\end{equation}

Take the sume of the diagonal of the matrix above, that is definition of the **Trace**, and for any suqare matrix $A$, its trace denotes as $Tr(A)$. Hence, we obtian

\begin{equation}
	Tr(adj(A)\frac{\partial A}{\partial x})= \sum_{i=1}^{n}\sum_{j=1}^{n}C_{ij}\frac{\partial a_{ij}}{\partial x}
\end{equation}

which is equal to \eqref{7}. Then we obtain

\begin{equation}
	\frac{\partial det(A)}{\partial x} = Tr(adj(A)\frac{\partial A}{\partial x})
\end{equation}

which is known as **Jacobi's formula**. We have an important corollary

\begin{equation}
	\frac{\partial In \; det(A)}{\partial x} = Tr(A^{-1}\frac{\partial A}{\partial x})
\end{equation}

by using property of adjugade matrix 

\begin{align}
	A\; adj(A) = det(A)\; I_{n\times n}\newline
\end{align}

If A is invertiable

\begin{align}
	A^{-1}= \frac{1}{det(A)}\; adj(A)
\end{align}

## Leibnize Formula
 We have **Leibnize formula** to calcualte the $det(A)$

<div markdown="0">
\begin{align}
	det(A)=\sum_{\sigma\in P_n} \color{blue}{sgn(\sigma)} \prod_{i=1}^{n}a_{i,\color{red}{\sigma(i)}}
\end{align}
</div>

where $P_n$ is the set of all permutation of (1,2,..,n), and 

<div markdown="0">
\begin{gather*}
\color{blue}{sgn(\sigma)} =
\begin{cases}
1 & \text{if $\sigma$ is even permutaiton}\newline
-1 & \text{otherwise}
\end{cases}
\end{gather*}
</div>

Example.

<div markdown="0">
\begin{align}
	det(A_{3\times 3}) = 
	\color{blue}{(-1)^0}a_{1,\color{red}{1}}a_{2,\color{red}{2}}a_{3,\color{red}{3}}
	+\color{blue}{(-1)^1}a_{1,\color{red}{1}}a_{2,\color{red}{3}}a_{3,\color{red}{2}}
	+\color{blue}{(-1)^1}a_{1,\color{red}{2}}a_{2,\color{red}{1}}a_{3,\color{red}{3}}\newline
	+\color{blue}{(-1)^2}a_{1,\color{red}{2}}a_{2,\color{red}{3}}a_{3,\color{red}{1}}
	+\color{blue}{(-1)^2}a_{1,\color{red}{3}}a_{2,\color{red}{1}}a_{3,\color{red}{2}}
	+\color{blue}{(-1)^1}a_{1,\color{red}{3}}a_{2,\color{red}{2}}a_{3,\color{red}{1}}
\end{align}
</div>

Based on this formula, we know

* There are $n!$ products in the formulate since there are $n!$ permutation for a $n\times n$ matrix.


[#1]: http://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/