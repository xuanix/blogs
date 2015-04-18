---
layout:     post
title: "Suport vector machine #3 - Soft Margin"
subtitle:   "Support vector classifier with soft margin can deal with non-separable case."
author: "Tianxiang Gao"
date: "April 18, 2015"
header-img: "img/post-bg-02.jpg"
---

In [previous blog](http://gaotx.com/blogs/2015/04/13/maximal-margin-classifier/), we found a separating hyperplane for **linearly separable** case by solving an optimization problem 

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\\\\ 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq M, \; i = 1, \ldots, n.
\end{align}

which is known as *maximal margin classifier*. Based on the constraint (2), any observation should be at least $M$ away from the separating hyperplane. So there is no any observation falls into the margin area, and even falls in the wrong side of separating hyperplane. In anthoer word, the dataset should be **separable**. However, this margin is to sensitive to the support vectors, which are observations very close to margin. With or without a specific observation, the generated separating hyperplane changes a lot. In essense, it is overfitting to training dataset. Moreover, in practice, various factors effect the collection of data. As result, overlapping between classes is inevitable such as the two figures below.

<img src="{{site.baseurl}}/img/svm/nonseparable.png">

You can find the figures from the website of [Introduction to Statistical Learning (**ISL**)][#1]. According to the left figure, we allow two points falling into the margin area. To the right figure, we even allows small observations on the wrong side of the separating hyperplane. This kind of margin called **soft margin**.

We can get such soft margin by solving an variant of the optimization problem and the classifier are knonw as **support vector classifer**. We introduce a set of nonnegative slack variables $\xi$. Then we can reformulate the above problem  

\begin{align} 
&\underset{\beta,\beta_0, ||\beta||=1}{\text{max}}& & M\newline 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq M - \xi_i, \; i = 1, \ldots, n. \label{4}\newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C
\end{align}

where $C$ a nonnegative tuning parameter. The slack variable $\xi_i$ in \eqref{4}tells us the location of the $i$th observation. 

* If $\xi_i=0$, the $i$the observation is on the correct side of margin such as observations 7, 9, 10, 2, 3, 4, 5, and 6 in both figures.
* If $0<\xi_i<M$, the $i$the observation falls on the margin area but on the correct side of the separating hyperplane such as observations 8 and 1 in the left figure.
* If $\xi_i>M$, the $i$the observation is on the wrong side of separating hyperplane such as observations 11, 8, 1, and 12 in right figure.

Now we consider the role of tunning parameter $C$. $C$ bounds the sum of $\xi_i$'s, and determins the tolerance of violations to the margin and to the hyperplane. We can consider $C$ as the total budget for the amount that the margin can be violated by the $n$ observations. 

* If $C=0$, then there is no **budget** for the violation. All $\xi_i$ should be equal to zero, in which case amonuts to maixmal margin classifier. Of cause, maximal margin hyperplane only exists if separabble case. So there is no feasible solution for the optimization problem.
* If $0<C$, there is no more than $C$ observations can be on the wrong side of hyperplane, because if $i$th observation is on the wrong side, then $\xi_i>1$ and $\sum\xi_i\leq C$. 

$C$ also controls the [bias-variance trade-off]() of the support vector classifier. When $C$ small, we seek narrow margin which amounts to a classifier that is highly fit (or even overfit) to the training data. So the variance is very hight, while bias is low. When $C$ larger, the margin is wider and allows more violation of observations. It amounts to a classifier that is more biased but lower variance. 

In order to solve this problem easily through [KKT condition and lagrange dual][#2], we reformulate this optimization problem. We replace constrain \eqref{4} with

$$ y_i(\beta^Tx_i+\beta_0) \geq M(1-\xi_i) $$

to make it convex optimization (the original is not a convex optimization). If you are not familier with *convex optimization*, just consider this process is to make the problem easier to solve. We drop the constrain 
$ ||\beta||=1 $ by replacing constrain \eqref{4} with

$$ \cfrac{1}{||\beta||}y_i(\beta^Tx_i+\beta_0) \geq M(1-\xi_i) $$

Since for any $\beta$ and $\beta_0$ satisfying the inequation, its scaled positively multiple also satisifies the inequation, we just arbitrarily assume 
$ M = \cfrac{1}{||\beta||} $. Then, we minimizing $ \cfrac{1}{2}\beta^T\beta$ for easier solving. 

\begin{align} 
&\underset{\beta,\beta_0}{\text{min}}& & \cfrac{1}{2}\beta^T\beta\newline 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq 1 - \xi_i, \; i = 1, \ldots, n. \newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\newline
& & & \sum_{i=1}^{n}\xi_i \leq C\label{10}
\end{align}

We drop the "budget" constrain \eqref{10} by introducing it to the objective function and obtain the final formulation

\begin{align} 
&\underset{\beta,\beta_0}{\text{min}}& & \cfrac{1}{2}\beta^T\beta+C\sum_{i=1}^{n}\xi_i\label{11}\newline 
& \text{subject to}
& & y_i(\beta^Tx_i+\beta_0) \geq 1 - \xi_i, \; i = 1, \ldots, n. \label{12}\newline
& & & \xi_i \geq 0, \; i = 1, \ldots, n.\label{13}\newline
\end{align}

Then, 

* If $C\rightarrow +\infty$, becaue of minimization, all $\xi_i=0$, that is maximial margin classifier. 
* If $C\rightarrow 0$, then $\xi_i$ can be any nonnegative value. Because of minimization, $\beta$ will be as small as possible.

This is quadratic optimization problem. Let's solve it by langrange dual. We drop the contrains \eqref{12} and \eqref{13} by multiplying the langrange multipliers $\alpha$ and $\mu$. Then lagrange function 

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable columnalign="right left right left right left right left right left right left" rowspacing="3pt" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" displaystyle="true">
    <mtr>
      <mtd>
        <msub>
          <mi>L</mi>
          <mi>p</mi>
        </msub>
        <mo stretchy="false">(</mo>
        <mi>&#x03B2;<!-- β --></mi>
        <mo>,</mo>
        <msub>
          <mi>&#x03B2;<!-- β --></mi>
          <mn>0</mn>
        </msub>
        <mo>,</mo>
        <mi>&#x03BE;<!-- ξ --></mi>
        <mo>;</mo>
        <mi>&#x03B1;<!-- α --></mi>
        <mo>,</mo>
        <mi>&#x03BC;<!-- μ --></mi>
        <mo stretchy="false">)</mo>
        <mo>=</mo>
      </mtd>
      <mtd>
        <mfrac>
          <mrow>
            <mpadded width="0" height="8.6pt" depth="3pt">
              <mrow />
            </mpadded>
            <mstyle displaystyle="false" scriptlevel="0">
              <mrow class="MJX-TeXAtom-ORD">
                <mn>1</mn>
              </mrow>
            </mstyle>
          </mrow>
          <mrow>
            <mpadded width="0" height="8.6pt" depth="3pt">
              <mrow />
            </mpadded>
            <mstyle displaystyle="false" scriptlevel="0">
              <mrow class="MJX-TeXAtom-ORD">
                <mn>2</mn>
              </mrow>
            </mstyle>
          </mrow>
        </mfrac>
        <msup>
          <mi>&#x03B2;<!-- β --></mi>
          <mi>T</mi>
        </msup>
        <mi>&#x03B2;<!-- β --></mi>
        <mo>+</mo>
        <mi>C</mi>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </munderover>
        <msub>
          <mi>&#x03BE;<!-- ξ --></mi>
          <mi>i</mi>
        </msub>
      </mtd>
    </mtr>
    <mlabeledtr>
      <mtd id="mjx-eqn-14">
        <mtext>(14)</mtext>
      </mtd>
      <mtd />
      <mtd>
        <mi></mi>
        <mo>&#x2212;<!-- − --></mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </munderover>
        <msub>
          <mi>&#x03B1;<!-- α --></mi>
          <mi>i</mi>
        </msub>
        <mo stretchy="false">[</mo>
        <msub>
          <mi>y</mi>
          <mi>i</mi>
        </msub>
        <mo stretchy="false">(</mo>
        <msup>
          <mi>&#x03B2;<!-- β --></mi>
          <mi>T</mi>
        </msup>
        <msub>
          <mi>x</mi>
          <mi>i</mi>
        </msub>
        <mo>+</mo>
        <msub>
          <mi>&#x03B2;<!-- β --></mi>
          <mn>0</mn>
        </msub>
        <mo stretchy="false">)</mo>
        <mo>&#x2212;<!-- − --></mo>
        <mo stretchy="false">(</mo>
        <mn>1</mn>
        <mo>&#x2212;<!-- − --></mo>
        <msub>
          <mi>&#x03BE;<!-- ξ --></mi>
          <mi>i</mi>
        </msub>
        <mo stretchy="false">)</mo>
        <mo stretchy="false">]</mo>
        <mo>&#x2212;<!-- − --></mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </munderover>
        <msub>
          <mi>&#x03BC;<!-- μ --></mi>
          <mi>i</mi>
        </msub>
        <msub>
          <mi>&#x03BE;<!-- ξ --></mi>
          <mi>i</mi>
        </msub>
      </mtd>
    </mlabeledtr>
  </mtable>
</math>

Then the dual function is 

\begin{equation}
	g(\alpha, \mu) = min \,L_p(\beta, \beta_0,\xi; \alpha,\mu)
\end{equation}

In order to the minimum, set the partial derivatives to zero w.r.t $\beta, \beta_0$ and $\xi_i$, we obtain

\begin{align}
&\frac{\partial L_p}{\partial \beta} \rightarrow \beta=\sum_{i=1}^{n}\alpha_iy_ix_i,\label{16}\newline
&\frac{\partial L_p}{\partial \beta} \rightarrow 0= \sum_{i=1}^{n}\alpha_iy_i,\label{17}\newline
&\frac{\partial L_p}{\partial \beta} \rightarrow \alpha_i=C-\mu_i, \label{18}\forall i.
\end{align}

By substituting \eqref{16}-\eqref{18} into \eqref{14}, we obtain 

\begin{equation}
g(\alpha, \mu)
= \sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jx_i^Tx_j
\end{equation}

which gives us a lower bound on the objective function\eqref{11}. Then the dual problem is 

\begin{align}
&\underset{\alpha,\mu}{\text{max}}& & g(\alpha, \mu)
= \sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jx_i^Tx_j\newline 
& \text{subject to}
& & \sum_{i=1}^{n}\alpha_iy_i=0 \newline
& & & \alpha_i + \mu_i = C, \; i = 1, \ldots, n.\label{22}\newline
& & & \alpha_i, \mu_i \geq 0, \; i = 1, \ldots, n.\newline
\end{align}

We can make it easier by droping variable $\mu$ and replacing constrain\eqref{22} with $\alpha_i\leq C$. Then we obtain

\begin{align}
&\underset{\alpha}{\text{max}}& & 
\sum_{i=1}^{n}\alpha_i-\cfrac{1}{2}\sum_{i=1}^{n}\sum_{j=1}^{n}\alpha_i\alpha_jy_iy_jx_i^Tx_j\newline 
& \text{subject to}
& & \sum_{i=1}^{n}\alpha_iy_i=0 \newline
& & & 0 \leq \alpha_i \leq C, \; i = 1, \ldots, n.\newline
\end{align}

Any standard software can help us solve this problem. Substituting the optimal $\alpha^{*}$ into \eqref{16} obtains $\hat{\beta}$. In order to get $\hat{\beta_0}$, we use KKT conditions including the constrains

\begin{align}
	\alpha_i[y_i(\beta^Tx_i+\beta_0) -(1 - \xi_i)] = 0,\label{27}\newline
	\mu_i\xi_i=0,\label{28}
\end{align}

for $i=1,2, ..., n$. For any $\alpha_i>0$, associated observations are support vectors, and 
\begin{align}
	y_i(\beta^Tx_i+\beta_0) -(1 - \xi_i)=0, \newline
	\mu_i=C-\alpha_i>0,
\end{align}

based on \eqref{27} and \eqref{18}, respectively. Hence, $\xi_i=0$ because of KKT condition \eqref{28}. $\hat{\beta}_0$ can be obtained by 

$$ y_i(\hat{\beta}^Tx_i+\beta_0) =1$$

Typically we use an average of all the solutions for numerical stability. 

Given the solutions for $\hat{\beta}$ and $\hat{\beta}_0$, the decision function (or classifier) is 

\begin{align}
	G(x) = & sign[\hat{f}(x)] \nonumber \newline
		   & sign[\hat{\beta}^T(x) + \hat{\beta}_0]
\end{align}

The tunning parameter $C$ can be obtained through *cross-validation*.

[#1]:http://www-bcf.usc.edu/~gareth/ISL/index.html
[#2]: http://gaotx.com/blogs/2015/04/14/kkt-cvx/