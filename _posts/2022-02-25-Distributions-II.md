---
title: "Distributions II"
date: 2022-02-25
toc: true
mathjax: true
categories:
  - study
tags:
  - statistics
---

# Distribution Post 2

## Gamma Distribution

$$\text{Symbol}\;\;\; X \sim \Gamma(\alpha, \beta) $$

$$ \text{PMF.}\;\;\;
f(x) = 
\left\{\begin{array}{ll} \frac{1}{\Gamma(\alpha) {\beta}^{\alpha}} {x}^{\alpha-1} {e}^{-x/\beta} & \text { if } 0 < x < \infty \\ \ 0 & \text { if } \text{elsewhere} \end{array}\right.
$$



$$ \text{MGF.}\;\;\; M(t) = \int^{\infty}_{0} {e}^{tx} \frac{{x}^{\alpha-1}}{\Gamma(\alpha) {\beta}^{\alpha}} {e}^{-\frac{x}{\beta}} dx = \frac{1}{(1-\beta t)^\alpha} $$
  

$$ \text{MEAN.}\;\;\; \mu = M'(0) = \alpha\beta$$

$$ \text{STD.}\;\;\; {\sigma}^{2} = M''(0) - {\mu}^{2} = \alpha {\beta}^{2} $$


```python
import numpy as np
import matplotlib.pyplot as plt
import scipy.stats as stats
import os
import seaborn as sns
from scipy.stats import gamma

def show_gamma():
    """Show an example of Poisson distributions"""
    a1 = 1.99
    a2 = 3.99
    a3 = 6.99

    x1 = np.linspace(gamma.ppf(0.01, a1), gamma.ppf(0.99, a1), 100)
    x2 = np.linspace(gamma.ppf(0.01, a2), gamma.ppf(0.99, a2), 100)
    x3 = np.linspace(gamma.ppf(0.01, a2), gamma.ppf(0.99, a2), 100)

    plt.plot(x1, gamma.pdf(x1, a1), 'r', lw=5, alpha=0.6, label='$a={0}$'.format(a1))
    plt.plot(x2, gamma.pdf(x2, a2), 'y', lw=5, alpha=0.6, label='$a={0}$'.format(a2))
    plt.plot(x3, gamma.pdf(x3, a3), 'b', lw=5, alpha=0.6, label='$a={0}$'.format(a3))

    # Arbitrarily select 3 lambda values
    # lambdas = [1,4,10]
    
    # k = np.arange(20)       # generate x-values
    # markersize = 8
    # for par in lambdas:
    #     plt.plot(k, stats.poisson.pmf(k, par),  '*--', label='$\lambda={0}$'.format(par))
    
    # Format the plot
    plt.legend()
    plt.title('Gamma Distribution')
    plt.xlabel('X')
    plt.ylabel('P(X)')

show_gamma()
```


    
![png](distributions_files/distributions_8_0.png)
    


## $\chi^{2}$ Distribution


```python
def showChi2():
    '''Utility function to show Chi2 distributions'''
    
    t = np.arange(0, 8, 0.05)
    Chi2Vals = [1,2,3,5]
    
    for chi2 in Chi2Vals:
        plt.plot(t, stats.chi2.pdf(t, chi2), label='n={0}'.format(chi2))
    plt.legend()
        
    plt.xlim(0,8)
    plt.xlabel('X')
    plt.ylabel('pdf(X)')
    plt.axis('tight')

showChi2()
```


    
![png](distributions_files/distributions_10_0.png)
    


## Exponential Distribution


```python
def showExp():
    '''Utility function to show exponential distributions'''
    
    t = np.arange(0, 3, 0.01)
    lambdas = [0.5, 1, 1.5]
    
    for par in lambdas:
        plt.plot(t, stats.expon.pdf(t, 0, par), label='$\lambda={0:3.1f}$'.format(par))
    plt.legend()
        
    plt.xlim(0,3)
    plt.xlabel('X')
    plt.ylabel('pdf(X)')
    plt.axis('tight')
    plt.legend()

showExp()
```


    
![png](distributions_files/distributions_12_0.png)
    

