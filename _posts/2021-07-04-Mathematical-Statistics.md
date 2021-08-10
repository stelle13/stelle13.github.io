---
title: "Mathematical Statistics"
date: 2021-07-04
mathjax: true
toc: true
categories:
  - study
tags:
  - statistics
  - math
---


In this post, I will discuss some of the basic statistical underpinnings of machine learning. 

# Motivation

Over the past few days, I was thinking about previewing the course materials for a class called Mathematical Statistics I. Interestingly though, almost as if by a coincidence, I realized that much of the statistics in the course textbook also appeared in one of the textbooks I was using to study the mathematical foundations of machine learning--*Mathematics for Machine Learning*, by Deisenroth, Faisal, and Ong. So to get an early grasp of math used in machine learning, as well as to preview the course material, I decided to study mathematical statistics, using publicly available lectures videos and notes from [MIT OpenCourseWare](https://ocw.mit.edu/index.htm). Note that the materials referenced in this blog post are under the Creative Commons License, and that any redistributions of the materials do not infringe copyrights.

Just as a side note, I have often noticed that when I preview course materials during summer break, I end up forgetting most of what I studied by the time fall semester begins--that is, if I don't record what I have learned. So to create a visible record of what I studied, I decided to use this blog post to explain to my future self what I learned, in my own terms. I believe that once school semester begins, the contents of this blog post will serve as a useful reference. As mentioned above, the following contents heavily reference a lecture titled: [Introduction to Probability and Statistics](https://ocw.mit.edu/courses/mathematics/18-655-mathematical-statistics-spring-2016/lecture-notes/), by lecturer Jeremy Orloff. 







***
# Joint Probability Density Functions



First, let's go over the definition of a joint probability density function (jpdf). The joint probability density function is simply a function that provides the probability of a specific random vector. A random vector, expressed in notation $(X_1, X_2)$, essentially describes a scenario in which the values of the random variables $X_1$ and $X_2$ are as follows: $X_1 = x_1$, $X_2 = x_2$. The joint probability density function is expressed through the following notation:


$$ f_{X_1, X_2} (x_1, x_2) = P[X_1 = x_1, X_2 = x_2] $$

Of course, the properties of the jpdf function are as follows:

In the case of a discrete random vector $(X_1, X_2)$,

$$ (i)\;\; 0\leq f_{X_1, X_2} (x_1, x_2) \leq 1 \;\;\; \text{and} \;\;\; (ii) \;\; \sum\sum \; f_{X_1, X_2} (x_1, x_2) = 1 $$ 

, and for the case of a continuous random vector $(X_1, X_2)$, condition (ii) would simply be changed to... 

$$ (ii) \;\; \int \int_D f_{X_1, X_2}(x_1, x_2) \,dx_1dx_2 = 1 $$

for $(X_1,X_2)$ defined on space D, and function $f_{X_1,X_2} (x_1, x_2)$ being the jpdf. All the above formulas simply mean that if the sum or the integral of a jpdf is taken on the entire sample space, the value will be one--which is an intuitive result, given that the sample space is the entire collection of possible events and values of random variables $X_1 \text{ and } X_2$.






***
# Sum Rule, Product Rule, and Bayes' Theorem

## Sum Rule (Marginalization Property)

Next, let's look at the marginalization property of joint probability density functions. This property allows one to find the probability that a single random variable would take a certain value, by summing up the jpdf with respect to the other random variables. More specifically, one could do this by fixing the value for the particular random variable to a certain constant, and summing or integrating the function with regards to the other variables. 



To put that into more concise mathematical terms, 


$$
p(\boldsymbol{x}) = \left\{\begin{array}{ll} \sum\limits_{y\in Y} p(\boldsymbol x, \boldsymbol y) \;\; \text{(if y is discrete)} \\ \int_Y p(\boldsymbol x, \boldsymbol y) \, dy \;\; \text{(if y is continuous)}
\end{array} \right.
$$

In other words, we can find the single variable pdf of random variable X, using the sum rule, applied to the joint pdf of random variables X and Y. 

## Product Rule and Bayes' Theorem

Next is the famous Bayes' theorem, which I previously mentioned in the earlier posts, but never actually explained in mathematical terms. 

The following, rather intuitive, mathematical statement is the product rule

$$ p(x,y) = p(y|x) \cdot p(x) $$ 

, and from the product rule follows the famous Bayes' theorem

$$ p(x|y) = \frac{p(y|x) \cdot p(x)}{p(y)} $$

The two theorems work under the condition that the random variables $x$ and $y$ are independent. 

***
# Means and Covariances

## Means and Expectation values

In this section, let's briefly look over the definition of means and covariances. The rather obvious definition of the expectation value of $g(x)$, with respect to the continuous random variable $X$, is the following, where $p(x)$ denotes the pdf of X: 

$$ \mathbb{E}_X[g(x)] = \int_X g(x) \cdot p(x) \, dx $$

This equation has a rather interesting name, it turns out--the [Law of the Unconscious Statistician](https://en.wikipedia.org/wiki/Law_of_the_unconscious_statistician).

If $X$ is discrete, simply replace the integral with the sigma notation. 

$$ \mathbb{E}_X[g(x)] = \sum_{x \in X} g(x) \cdot p(x) $$

... where $\bf{X}$ in the subscript denotes the target space of $x$

If the random variable $X$ consists of more than one variable--that is, if $X$ is a multivariate random variable--then we denote its expectation value a bit differently. If $X = (X_1, X_2, X_3, ... X_n)$, with $X_n$ being univariate, then $X$'s expectation value is an D-dimensional vector. 

$$ E_X[\bf x] = \begin{bmatrix} E_{X_1}[x_1]\\E_{X_2}[x_2] \\. \\. \\. \\E_{X_D}[x_D] \\\end{bmatrix}\in \mathbb{R}^D $$

In the vector, each element is defined as the following: 


$$
E_{X_d}[x_d] = \left\{
\begin{array}{ll}\int_X x_d \cdot p(x_d) \, dx_d \;\; \text{(if X is continuous)} \\ \sum_\limits{x_i \in X} x_i \cdot p(x_d = x_i) \;\; \text{(if X is discrete)}\end{array}\right.
$$



In other words, $x_d$ represents the *d*-th element of all the multivariate random variable $X$ being considered, and if we sum up those d-th elements, the sum is simply the *d*-th expectation value element in the vector.

Another interesting property of expectation values is the following.

$$ E(X_2) = \int_{-\infty}^\infty\int_{-\infty}^\infty x_2f(x_1,x_2) dx_2dx_1$$
$$ = E[E(X_2|X_1)] $$

In other words, to find the expectation value $E(X_2)$ of random variable $X_2$, one has to first realize that $f(x_1,x_2)$ describes the probability of observing $x_2$, given the fact that $x_1$ was observed for $X_1$. The value after only integrating $x_2f(x_1,x_2)$ with respect to $dx_2$ is the expectation value of $X_2$ when $X_1=x_1$. 

But to find $E(X_2)$, one not only needs to find the $E(X_2)$ for a specific value $X_1 = x_1$, but $E(X_2)$ for all values of  $X_1$. Therefore, one needs to integrate $\int_{-\infty}^\infty x_2f(x_1,x_2) dx_2$ once again, with respect to $x_1$. The end value is $E(X_2)$. 



$$ E(X_2) = \int_{-\infty}^\infty\int_{-\infty}^\infty x_2f(x_1,x_2) dx_2dx_1 $$

$$ = \int_{-\infty}^{\infty} \{\int_{-\infty}^{\infty} x_2 \frac{f(x_1,x_2)}{f_1(x_1)} dx_2\} \cdot f_1(x_1) \cdot dx_1$$

$$ = \int_{-\infty}^{\infty} E(X_2 | x_1) \cdot f_1(x_1) dx_1 $$

$$ = E\{E(X_2 | X_1)\} $$

## Covariances

The definition of covariance is relatively simple enough. With $\mu_1$ and $\mu_2$ being the mean of X and Y, respectively, the covariance is simply the expectation value of the multiple of $(X-\mu_1)(Y-\mu_2)$.

$$ cov(X,Y) = E[(X-\mu_1)(Y-\mu_2)]$$ 

The variable $\rho$, which stands for the correlation coefficient, is then calculated by dividing the covariance by the two standard deviations. 

$$ \rho = \frac{cov(X,Y)}{\sigma_1\sigma_2} $$ 

# Conclusion

In this post, I skimmed over some basic probability concepts used in machine learning. While this post is, by no means, a comprehensive review of the concerned topic, I hope I can take some time to review this post in the near future and refresh my memory. For other readers--in particular, students who wish to learn more about probability in machine learning--I hope this post was useful. Thanks for reading. 
