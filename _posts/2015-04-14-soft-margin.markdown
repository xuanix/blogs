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

<p><span class="MathJax_Preview" style="color: inherit;"></span><div class="MathJax_Display"><span class="MathJax" id="MathJax-Element-51-Frame"><nobr><span class="math" id="MathJax-Span-819" role="math" style="width: 100%; display: inline-block; min-width: 27.566em;"><span style="display: inline-block; position: relative; width: 100%; height: 0px; font-size: 117%; min-width: 27.566em;"><span style="position: absolute; clip: rect(3.72em 1000.002em 10.216em -999.998em); top: -7.22em; left: 0.002em; width: 100%;"><span class="mrow" id="MathJax-Span-820"><span class="mtable" id="MathJax-Span-821" style="min-width: 32.224em;"><span style="display: inline-block; position: relative; width: 100%; height: 0px; min-width: 27.566em;"><span style="display: inline-block; position: absolute; width: 25.301em; height: 0px; clip: rect(-3.502em 1000.002em 2.994em -999.998em); top: 0.002em; left: 50%; margin-left: -12.647em;"><span style="position: absolute; clip: rect(2.438em 1000.002em 6.583em -999.998em); top: -4.87em; left: 0.002em;"><span style="display: inline-block; position: relative; width: 7.353em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.447em -999.998em); top: -5.639em; right: 0.002em;"><span class="mtd" id="MathJax-Span-822"><span class="mrow" id="MathJax-Span-823"><span class="msubsup" id="MathJax-Span-824"><span style="display: inline-block; position: relative; width: 0.985em; height: 0px;"><span style="position: absolute; clip: rect(3.25em 1000.002em 4.147em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-825" style="font-family: STIXGeneral-Italic;">L<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.558em;"><span class="mi" id="MathJax-Span-826" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">p</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-827" style="font-family: STIXGeneral-Regular;">(</span><span class="mi" id="MathJax-Span-828" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span class="mo" id="MathJax-Span-829" style="font-family: STIXGeneral-Regular;">,</span><span class="msubsup" id="MathJax-Span-830" style="padding-left: 0.173em;"><span style="display: inline-block; position: relative; width: 0.942em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-831" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.515em;"><span class="mn" id="MathJax-Span-832" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">0</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-833" style="font-family: STIXGeneral-Regular;">,</span><span class="mi" id="MathJax-Span-834" style="font-family: STIXGeneral-Italic; padding-left: 0.173em;">ξ</span><span class="mo" id="MathJax-Span-835" style="font-family: STIXGeneral-Regular;">;</span><span class="mi" id="MathJax-Span-836" style="font-family: STIXGeneral-Italic; padding-left: 0.173em;">α</span><span class="mo" id="MathJax-Span-837" style="font-family: STIXGeneral-Regular;">,</span><span class="mi" id="MathJax-Span-838" style="font-family: STIXGeneral-Italic; padding-left: 0.173em;">μ</span><span class="mo" id="MathJax-Span-839" style="font-family: STIXGeneral-Regular;">)</span><span class="mo" id="MathJax-Span-840" style="font-family: STIXGeneral-Regular; padding-left: 0.301em;">=</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.891em 1000.002em 4.147em -999.998em); top: -2.434em; right: 0.002em;"><span class="mtd" id="MathJax-Span-884"><span class="mrow" id="MathJax-Span-885"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span><span style="display: inline-block; width: 0px; height: 4.874em;"></span></span><span style="position: absolute; clip: rect(3.72em 1000.002em 10.216em -999.998em); top: -7.22em; left: 7.353em;"><span style="display: inline-block; position: relative; width: 17.951em; height: 0px;"><span style="position: absolute; clip: rect(2.139em 1000.002em 5.387em -999.998em); top: -5.639em; left: 0.002em;"><span class="mtd" id="MathJax-Span-841"><span class="mrow" id="MathJax-Span-842"><span class="mfrac" id="MathJax-Span-843"><span style="display: inline-block; position: relative; width: 0.643em; height: 0px; margin-right: 0.13em; margin-left: 0.13em;"><span style="position: absolute; clip: rect(2.908em 1000.002em 4.489em -999.998em); top: -4.784em; left: 50%; margin-left: -0.254em;"><span class="mrow" id="MathJax-Span-844"><span class="mpadded" id="MathJax-Span-845"><span style="display: inline-block; position: relative; width: 0.002em; height: 0px;"><span style="position: absolute; clip: rect(3.891em 1000.002em 4.147em -999.998em); top: -4.015em; left: 0.002em;"><span class="mrow" id="MathJax-Span-846"><span class="mrow" id="MathJax-Span-847"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mstyle" id="MathJax-Span-848"><span class="mrow" id="MathJax-Span-849"><span class="texatom" id="MathJax-Span-850"><span class="mrow" id="MathJax-Span-851"><span class="mn" id="MathJax-Span-852" style="font-family: STIXGeneral-Regular;">1</span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(2.908em 1000.002em 4.489em -999.998em); top: -3.118em; left: 50%; margin-left: -0.254em;"><span class="mrow" id="MathJax-Span-853"><span class="mpadded" id="MathJax-Span-854"><span style="display: inline-block; position: relative; width: 0.002em; height: 0px;"><span style="position: absolute; clip: rect(3.891em 1000.002em 4.147em -999.998em); top: -4.015em; left: 0.002em;"><span class="mrow" id="MathJax-Span-855"><span class="mrow" id="MathJax-Span-856"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mstyle" id="MathJax-Span-857"><span class="mrow" id="MathJax-Span-858"><span class="texatom" id="MathJax-Span-859"><span class="mrow" id="MathJax-Span-860"><span class="mn" id="MathJax-Span-861" style="font-family: STIXGeneral-Regular;">2</span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(0.9em 1000.002em 1.199em -999.998em); top: -1.28em; left: 0.002em;"><span style="border-left-width: 0.643em; border-left-style: solid; display: inline-block; overflow: hidden; width: 0px; height: 0.045em; vertical-align: 0.002em;"></span><span style="display: inline-block; width: 0px; height: 1.071em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-862"><span style="display: inline-block; position: relative; width: 1.071em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-863" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -4.442em; left: 0.558em;"><span class="mi" id="MathJax-Span-864" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">T<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.045em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mi" id="MathJax-Span-865" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span class="mo" id="MathJax-Span-866" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">+</span><span class="mi" id="MathJax-Span-867" style="font-family: STIXGeneral-Italic; padding-left: 0.259em;">C<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.045em;"></span></span><span class="munderover" id="MathJax-Span-868" style="padding-left: 0.173em;"><span style="display: inline-block; position: relative; width: 1.284em; height: 0px;"><span style="position: absolute; clip: rect(2.908em 1000.002em 4.618em -999.998em); top: -4.015em; left: 0.002em;"><span class="mo" id="MathJax-Span-869" style="font-family: STIXSizeOneSym; vertical-align: -0.511em;">∑</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.421em 1000.002em 4.276em -999.998em); top: -2.861em; left: 0.13em;"><span class="texatom" id="MathJax-Span-870"><span class="mrow" id="MathJax-Span-871"><span class="mi" id="MathJax-Span-872" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span class="mo" id="MathJax-Span-873" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">=</span><span class="mn" id="MathJax-Span-874" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">1</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.464em 1000.002em 4.147em -999.998em); top: -5.212em; left: 0.472em;"><span class="texatom" id="MathJax-Span-875"><span class="mrow" id="MathJax-Span-876"><span class="mi" id="MathJax-Span-877" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">n</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-878" style="padding-left: 0.173em;"><span style="display: inline-block; position: relative; width: 0.729em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.318em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-879" style="font-family: STIXGeneral-Italic;">ξ</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.472em;"><span class="mi" id="MathJax-Span-880" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(2.267em 1000.002em 5.387em -999.998em); top: -2.434em; left: 0.002em;"><span class="mtd" id="MathJax-Span-886"><span class="mrow" id="MathJax-Span-887"><span class="mi" id="MathJax-Span-888"></span><span class="mo" id="MathJax-Span-889" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">−</span><span class="munderover" id="MathJax-Span-890" style="padding-left: 0.259em;"><span style="display: inline-block; position: relative; width: 1.284em; height: 0px;"><span style="position: absolute; clip: rect(2.908em 1000.002em 4.618em -999.998em); top: -4.015em; left: 0.002em;"><span class="mo" id="MathJax-Span-891" style="font-family: STIXSizeOneSym; vertical-align: -0.511em;">∑</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.421em 1000.002em 4.276em -999.998em); top: -2.861em; left: 0.13em;"><span class="texatom" id="MathJax-Span-892"><span class="mrow" id="MathJax-Span-893"><span class="mi" id="MathJax-Span-894" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span class="mo" id="MathJax-Span-895" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">=</span><span class="mn" id="MathJax-Span-896" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">1</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.464em 1000.002em 4.147em -999.998em); top: -5.212em; left: 0.472em;"><span class="texatom" id="MathJax-Span-897"><span class="mrow" id="MathJax-Span-898"><span class="mi" id="MathJax-Span-899" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">n</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-900" style="padding-left: 0.173em;"><span style="display: inline-block; position: relative; width: 0.814em; height: 0px;"><span style="position: absolute; clip: rect(3.464em 1000.002em 4.147em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-901" style="font-family: STIXGeneral-Italic;">α</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.558em;"><span class="mi" id="MathJax-Span-902" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-903" style="font-family: STIXGeneral-Regular;">[</span><span class="msubsup" id="MathJax-Span-904"><span style="display: inline-block; position: relative; width: 0.729em; height: 0px;"><span style="position: absolute; clip: rect(3.464em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-905" style="font-family: STIXGeneral-Italic;">y</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.429em;"><span class="mi" id="MathJax-Span-906" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-907" style="font-family: STIXGeneral-Regular;">(</span><span class="msubsup" id="MathJax-Span-908"><span style="display: inline-block; position: relative; width: 1.071em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-909" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -4.442em; left: 0.558em;"><span class="mi" id="MathJax-Span-910" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">T<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.045em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-911"><span style="display: inline-block; position: relative; width: 0.729em; height: 0px;"><span style="position: absolute; clip: rect(3.464em 1000.002em 4.147em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-912" style="font-family: STIXGeneral-Italic;">x<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.429em;"><span class="mi" id="MathJax-Span-913" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-914" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">+</span><span class="msubsup" id="MathJax-Span-915" style="padding-left: 0.259em;"><span style="display: inline-block; position: relative; width: 0.942em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-916" style="font-family: STIXGeneral-Italic;">β<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.002em;"></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.515em;"><span class="mn" id="MathJax-Span-917" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">0</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-918" style="font-family: STIXGeneral-Regular;">)</span><span class="mo" id="MathJax-Span-919" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">−</span><span class="mo" id="MathJax-Span-920" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">(</span><span class="mn" id="MathJax-Span-921" style="font-family: STIXGeneral-Regular;">1</span><span class="mo" id="MathJax-Span-922" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">−</span><span class="msubsup" id="MathJax-Span-923" style="padding-left: 0.259em;"><span style="display: inline-block; position: relative; width: 0.729em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.318em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-924" style="font-family: STIXGeneral-Italic;">ξ</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.472em;"><span class="mi" id="MathJax-Span-925" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="mo" id="MathJax-Span-926" style="font-family: STIXGeneral-Regular;">)</span><span class="mo" id="MathJax-Span-927" style="font-family: STIXGeneral-Regular;">]</span><span class="mo" id="MathJax-Span-928" style="font-family: STIXGeneral-Regular; padding-left: 0.259em;">−</span><span class="munderover" id="MathJax-Span-929" style="padding-left: 0.259em;"><span style="display: inline-block; position: relative; width: 1.284em; height: 0px;"><span style="position: absolute; clip: rect(2.908em 1000.002em 4.618em -999.998em); top: -4.015em; left: 0.002em;"><span class="mo" id="MathJax-Span-930" style="font-family: STIXSizeOneSym; vertical-align: -0.511em;">∑</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.421em 1000.002em 4.276em -999.998em); top: -2.861em; left: 0.13em;"><span class="texatom" id="MathJax-Span-931"><span class="mrow" id="MathJax-Span-932"><span class="mi" id="MathJax-Span-933" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span class="mo" id="MathJax-Span-934" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">=</span><span class="mn" id="MathJax-Span-935" style="font-size: 70.7%; font-family: STIXGeneral-Regular;">1</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; clip: rect(3.464em 1000.002em 4.147em -999.998em); top: -5.212em; left: 0.472em;"><span class="texatom" id="MathJax-Span-936"><span class="mrow" id="MathJax-Span-937"><span class="mi" id="MathJax-Span-938" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">n</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-939" style="padding-left: 0.173em;"><span style="display: inline-block; position: relative; width: 0.814em; height: 0px;"><span style="position: absolute; clip: rect(3.464em 1000.002em 4.361em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-940" style="font-family: STIXGeneral-Italic;">μ</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.515em;"><span class="mi" id="MathJax-Span-941" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span><span class="msubsup" id="MathJax-Span-942"><span style="display: inline-block; position: relative; width: 0.729em; height: 0px;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.318em -999.998em); top: -4.015em; left: 0.002em;"><span class="mi" id="MathJax-Span-943" style="font-family: STIXGeneral-Italic;">ξ</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span><span style="position: absolute; top: -3.844em; left: 0.472em;"><span class="mi" id="MathJax-Span-944" style="font-size: 70.7%; font-family: STIXGeneral-Italic;">i</span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span><span style="display: inline-block; width: 0px; height: 7.224em;"></span></span></span><span style="display: inline-block; position: absolute; width: 1.669em; height: 0px; clip: rect(0.814em 1000.002em 1.925em -999.998em); top: 0.002em; right: 0.002em; margin-left: 0.9em; margin-right: 0.9em;"><span style="position: absolute; clip: rect(3.207em 1000.002em 4.318em -999.998em); top: -2.434em; right: 0.002em;"><span class="mtd" id="mjx-eqn-14"><span class="mrow" id="MathJax-Span-882"><span class="mtext" id="MathJax-Span-883" style="font-family: STIXGeneral-Regular;">(14)</span></span></span><span style="display: inline-block; width: 0px; height: 4.019em;"></span></span></span></span></span></span><span style="display: inline-block; width: 0px; height: 7.224em;"></span></span></span><span style="border-left-width: 0.003em; border-left-style: solid; display: inline-block; overflow: hidden; width: 0px; height: 7.353em; vertical-align: -3.397em;"></span></span></nobr></span></div><script type="math/tex; mode=display" id="MathJax-Element-51">\begin{align}
L_p(\beta, \beta_0,\xi; \alpha,\mu)= & \cfrac{1}{2}\beta^T\beta+C\sum_{i=1}^{n}\xi_i \nonumber \newline
	&-\sum_{i=1}^{n}\alpha_i[y_i(\beta^Tx_i+\beta_0)-(1 - \xi_i)]-\sum_{i=1}^{n}\mu_i\xi_i\label{14} 
\end{align}</script></p>

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