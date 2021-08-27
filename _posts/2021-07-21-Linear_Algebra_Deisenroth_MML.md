---
title: "Linear Mapping & Basis Change"
date: 2021-07-21
mathjax: true
toc: true
toc_sticky: true
categories:
  - study
tags:
  - linear-algebra
---



In this post, I will go over some basic linear algebra used in machine learning.  

## Motivation

Over the past two months, I have been learning from professor Andrew Ng's famous Coursera course, [Machine Learning](https://www.coursera.org/learn/machine-learning), provided by Stanford University. In the course, professor Ng went over some linear algebra and other math topics, but wanting to learn more, I started to self-study with the textbook, *Mathematics for Machine Learning* by Deisenroth, Faisal, and Ong. I started with the second chapter, which was about linear algebra--something I already learned in high school. Not surprisingly, I could skim through most of the chapter without any difficulty--except for some topics, including linear mapping and affine subspaces. For these specific topics, I studied with another text, simply called *Linear Algebra*, by professor Sang-gu Lee at Sungkyunkwan University. 

As always, I decided to make a record of what I learned, for me to easily recall the materials in the future. The following contents of this blog post reference a copyright-waived online textbook called *Linear Algebra*, written by college professors at several Korean institutions, and freely available at [BigBook](http://www.bigbook.or.kr/). BigBook is a Project Gutenberg-like initiative based in South Korea that aims to promote the distribution of eBooks with waived copyrights. For this text specifically, a notice on the front cover happened to specify that any redistributions/documentations of the content are allowed, and do not require explicit permission from the authors.

So without any further due, let's get right to some linear algebra.  




## Linear Mappings

### Linear Transformation

If the transformation $T: R^n \rightarrow R^m$ from $R^n$ to $R^m$ satisfies the two conditions below, the transformation $T$ is called a *linear transformation*, or a *linear mapping*.

$$(1) \:\:\: T(\textbf u + \textbf v) = T(\textbf u) + T(\textbf v)$$ 

$$(2) \:\:\: T(k \cdot \textbf u) = k \cdot T(\textbf u) \:\:\:(k \in \mathbb{R})$$



It is also import to remember that $T(\textbf 0) = \textbf 0$. If the linear transformation is $T: R^n \rightarrow R^n$, then the transformation is called a *linear operator*. 

### Special Mappings: *Injective*, *Surjective*, *Bijective*

If $T: R^m \rightarrow R^n$ satisfies the following condition, 

$$T(\textbf u) = T(\textbf v) \:\: \longrightarrow \:\: \textbf u = \textbf v$$

the transformation is called *injective*, or *one-to-one*.


Similarly, if $T: R^m \rightarrow R^n$ satisfies this condition, 

$$\forall \; \textbf w \in R^m \;\; \exists \; \textbf v \in R^n \;\; s.t \;\; T(\textbf v) = \textbf w$$

then the transformation is called *surjective*, or *onto*.

If $T: R^m \rightarrow R^n$ is both *injective* and *surjective*, the transformation is called *bijective*.


A quick method to see whether a linear transformation is *injective*, or *one-to-one*, is to check the following condition:

> Conditions for an *Injective* Transformation <br /><br />
  If $T: R^m \rightarrow R^n$ is a linear transformation, the sufficient and necessary condition for $T$ to be injective is $ker \; T = \textbf 0$. In other words... <br /><br />
    $$ [T(\textbf u) = T(\textbf v) \rightarrow \textbf u = \textbf v] \; \Longleftrightarrow \; ker \; T = \textbf 0 $$ <br /> 
    ... where the kernal of $T$ is defined as <br /> <br />
    $$ ker \; T = \{\textbf v \in R^n \; | \; T(\textbf v) = \textbf 0 $$



The quick proof is as follows:

> For the sufficient condition $ [T(\textbf u) = T(\textbf v) \rightarrow \textbf u = \textbf v] \; \Longrightarrow \; ker \; T = \textbf 0 $, where $ \forall \; v \in ker \; T $, the following statement is obvious: $T(v) = 0 = T(0) $. Since $T$ is *injective*, $ \textbf{v}= \textbf{0} $. Therefore, $\;ker \;T = \textbf{0}$. 

> For the necessary condition $ [T(\textbf u) = T(\textbf v) \rightarrow \textbf u = \textbf v] \; \Longleftarrow \; ker \; T = \textbf 0 $, we assume that for vectors $v_1$ and $v_2$, $T(v_1) = T(v_2)$. Then by the property of linear transformations, $T(v_1)-T(v_2) = T(v_1-v_2) = \textbf{0}$. Therefore, vector $v_1 - v_2 \in ker\;T = {\textbf{0}}$, and therefore, $ v_1 = v_2 $. Thus $T$ is *injective*.  



## Basis Change

### Ordered Basis & Transition Matrix

If a set of basis vectors $\alpha = \{x_1, x_2, ... x_n\}$ spans $R^n$, then all the vectors within $R^n$ can be expressed as a linear combination of the basis vectors. From such a basic definition of a set of basis vectors, we can define a *coordinate vector* of an arbitrary vector $x \in R^n$ as $[c_1, c_2, c_3, ... c_n]^T$, if $\textbf{x} = c_1x_1 + c_2x_2 + ... + c_nx_n$ ($\forall \; n \in \mathbb{N}, c_n \in \mathbb{R}$). 

The common notation for a coordinate vector of $x \in R^n $ is $[x]_\alpha$, where $\alpha$ is the *ordered basis* $\alpha = [x_1, x_2, ... x_n]$

$$ [x]_\alpha = [c_1, c_2, c_3, ... c_n]^T $$

Although the following properties of coordinate vectors may be somewhat obvious, for the sake of clarity, here are their two properties: 

$$ \text{1. }[x + y]_\alpha = [x]_\alpha + [y]_\alpha $$ \\
$$ \text{2. }[c \cdot x]_ \alpha = c \cdot [x]_\alpha $$

Now, in order to define a *transition matrix* in discussing basis change, suppose that $\alpha = [x_1, x_2, ... x_n]$ and $\beta = [y_1, y_2, ... y_n]$ are two different ordered bases in $R^n$. We wish to consider the relationship between those two ordered bases. If we define vector $\textbf{x}$ as... 

$$\textbf{x} = c_1y_1 + c_2y_2 + ... + c_ny_n$$ 

...with all coefficients as real numbers, then the coordinate vector of $\textbf{x}$ with respect to ordered basis $\beta$ would be: 

$$ [x]_\beta = \begin{bmatrix} c_1 \\ c_2 \\ c_3 \\ \vdots \\ c_n \end{bmatrix} $$

Considering the two properties provided above, the following equality holds if we express vector $\textbf{x}$ in terms of ordered basis $\alpha$: 

$$ [\textbf{x}]_\alpha = [c_1\textbf{y}_1 + c_2\textbf{y}_2 + ... + c_n\textbf{y}_n]_\alpha = c_1[\textbf{y}_1]_\alpha + c_2[\textbf{y}_2]_\alpha + ... + c_n[\textbf{y}_n]_\alpha $$

Since all of the basis vectors $y_j$ in the ordered basis $\beta = \{y_1, y_2, ... y_n\}$ are in $R^n$, they too can be expressed by a linear combination of basis vectors in ordered basis $\alpha$. Therefore, given that $[y_j]_\alpha$ $= [p_{1j}, p_{2j}, p_{3j}, ... p_{nj}]^T $ , $[\textbf{x}]_\alpha$ can be expressed by the following equality:

$$ [\textbf{x}]_\alpha = c_1 \begin{bmatrix} p_{11} \\ p_{21} \\ p_{31} \\ ... \\ p_{n1} \end{bmatrix} + c_2 \begin{bmatrix} p_{12} \\ p_{22} \\ p_{32} \\ ... \\ p_{n2} \end{bmatrix} + ... + c_n \begin{bmatrix} p_{1n} \\ p_{2n} \\ p_{3n} \\ ... \\ p_{nn} \end{bmatrix} = \begin{bmatrix} p_{11} p_{12} ... p_{1n} \\ p_{21} p_{22} ... p_{2n} \\ \vdots \vdots  \;\;\;\;\;\; \vdots \\ p_{n1} p_{n2} ... p_{nn}\end{bmatrix} \cdot \begin{bmatrix} c_1 \\ c_2 \\ c_3 \\ \vdots \\ c_n \end{bmatrix}  = P \cdot [\textbf{x}]_\beta$$ 



In the above equality, the matrix $P$ transforms vector $\textbf{x}$'s coordinate vector with respect to $\alpha$ to the coordinate vector with respect to $\beta$. Because the matrix changes the ordered basis with which the coordinate vector of $x$ was expressed in, the matrix $P$ is called a *transition matrix*. 

### Rank-Nullity Theorem

The Rank-Nullity theorem neatly describes the relationship between the dimensions of $Im(T)$ and $ker(T)$. The theorem has two forms:

> **Rank-Nullity Theorem**: <br /><br />
  For an $m$ by $n$ matrix $A$, <br /><br /> 
      $$ \text{1. rank}(A) + \text{nullity}(A) = n $$ <br /> <br />
      $$ \text{2. dim(Im}(T)) + \text{dim(ker}(T)) = \text{dim}(R^n) = n $$ 

The first statement, which is rather obvious, says that if there are $n$ vectors in an $m$ by $n$ matrix, the sum of the number of linearly independent vectors or pivot columns (rank$A$) and the number of linearly dependent vectors (nullity($A$)) is the total number of vectors in the matrix (or $n$).

The second statement, for which there was unfortunately no proof in the public domain textbook, required some time searching the Internet. Ultimately, I found an intuitive [proof](https://users.math.msu.edu/users/wwwu/Teaching%202012%20Fall/linfun2.pdf) provided by Michigan State University, which is described below. The proof, in my opinion, was not as detailed enough as I had hoped, which is why I am offering my own understanding of the proof in this section.  

Before we dive into it, it is important to know what it means to take the dimension of a vector set (i.e. dim(ker($T$))). To take the dimension of a vector set is to essentially look at how many linearly independent vectors are within the basis of the vector set. So dim(ker($T$)) would be the number of basis vectors in the kernel of $T$, and dim(im($T$)) would be the corresponding number for the image of $T$.

**Proof of Statement 2**:

We start with three suppositions: 

> For linear transformation $T: V \rightarrow W$ <br /><br />
    1. dim(ker($T$)) = $r$ <br />
    2. ker($T$) = $span$\{$v_1$, ... $v_r$\} <br />
    3. $V$ = $span$\{$v_1$, ... $v_r$ ... $v_{r+1}$, ... $v_{n}$\}






The proof "extends" the basis of ker($T$) $\subset$ $V$ to form the basis of $V$, but does not offer a numerical example to better illustrate this part, so here is a simple example that I came up with. If $T: \mathbb{R}^4 \rightarrow \mathbb{R}^4$, suppose $T$([$x_1$, $x_2$, $x_3$, $x_4$]$^T$) = [0, 0, $x_2$, $x_3$]$^T$.

Then the kernel of $T$ would be $span$ \{[$1$, $0$, $0$, $0$]$^T$, [$0$, $0$, $0$, $1$]$^T$\}, and the basis vectors would be [$1$, $0$, $0$, $0$]$^T$, [$0$, $0$, $0$, $1$]$^T$, both of which are basis vectors for $\mathbb{R}^4$. Therefore, we can "extend", or add more vectors on to, the list of basis vectors of ker($T$), to form the basis vectors of $V$. 

Since the statement to prove is $ \text{dim(Im}(T)) + \text{dim(ker}(T)) = \text{dim}(R^n) = n $, if we can show that \{$T(v_{r+1}), ..., T(v_{n})$\} is the basis of $Im$($T$), the equality $ r + n - r = n $ allows us to prove the statement. 

To show that a vector set is the *basis* of a vector space, we need to show two things: 

> 1. The vector set *spans* the vector space. 
> 2. The vector set is linearly independent. 

To show that the vector set \{$T(v_{r+1}), ..., T(v_{r+1})$\} spans Im($T$), we take an arbitrary vector $w$ to be an element of Im($T$), and show that $w$ can be expressed as a linear combination of the vectors in the vector set. Since $w$ $\in$ Im($T$),  $\exists v \in V \; \text{s.t}\; T(v) = w$. 

Express $v$ as $v = t_1v_1 + ... + t_rv_r + t_{r+1}v_{r+1} + ... + t_nv_n$, since the vectors $v_1 ... v_n$ are basis vectors for $V$. Then the following equalities hold as a direct consequence of the properties of linear transformation.

$$ w = T(v) $$

$$ w = T(t_1v_1 + ... + t_rv_r + t_{r+1}v_{r+1} + ... + t_nv_n) $$

$$ = t_1T(v_{r+1})+ ... +t_{r}T(v_{r})+t_{r+1}T(v_{r+1})+ ... +t_nT(v_{r+n}) $$

As the proof explicitly states, the last line is due to the fact that the vector set \{$T(v_{1}), ..., T(v_{r})$\} consists of zero vectors--a direct result of our assumption that ker($T$) = $span$\{$v_1$, ... $v_r$\}. Thus, we have now shown the first part--that the set \{$T(v_{r+1}), ..., T(v_{r+n})$\} *spans* vector space *w*. 

To show the second part--that \{$T(v_{r+1}), ..., T(v_{r+n})$\} is linearly independent--we need to set a linear combination of those vectors to the zero vector, and show that all the coefficients are zero. 

To do that, we start with an assumption and later show the validity of this assumption. 

> Assumption: <br /><br />
    $$ t_{r+1}T(v_{r+1})+ ... +t_nT(v_{r+n}) = \textbf{0}, \text{ for some } t_k \in \mathbb{R}, k \in \mathbb{N}, r < k < r+n+1$$ 


Then by the properties of linear transformation, $T(t_{r+1}v_{r+1} + ... + t_nv_n) = \mathbb{0}$, and $t_{r+1}v_{r+1} + ... + t_nv_n \in \text{ ker }(T)$. By the assumption at the beginning of the proof, ker($T$) = $span$\{$v_1$, ... $v_r$\}, which means that $t_{r+1}v_{r+1} + ... + t_nv_n$ can be expressed by a linear combination of the vectors in that set. 

In other words, $ t_{r+1}(v_{r+1})+ ... +t_n(v_{r+n}) = s_{1}(v_{1})+ ... +s_n(v_{r})$, for some $s_k \in \mathbb{R}, k \in \mathbb{N}, 0 < k < r+1$. Therefore, $ t_{r+1}(v_{r+1})+ ... +t_n(v_{r+n}) - [s_{1}(v_{1})+ ... +s_n(v_{r})] = \textbf{0}$, and considering the fact that we initially set the basis of $V$ to be \{$v_1$, ... $v_r$ ... $v_{r+1}$, ... $v_{n}$\}, we now know that coefficients $s_k$ and $t_k$ $\in \mathbb{R}$ are all 0.  

The above finding allows us to go back to our assumption immediately above, and because all the coefficients are zero, the set \{$T(v_{r+1}), ..., T(v_{n})$\} is linearly independent. Thus, we have shown the second part of the proof--that the above set is a basis for $Im(T)$--and conclude that dim($Im$($T$))$ = n-r. $ 

Finally, because $ dim(ker(T)) = r$, $dim(Im)(T) + dim(ker)(T) = r + (n - r) = n =  dim(V).$  $\blacksquare$.

## Conclusion

In this post, I documented my own understanding of some linear algebra topics that I was not completely familiar with. Although I initially started learning with *Mathematics for Machine Learning*, I admittedly fell down a rabbit hole with some concepts, looking into them more carefully and attempting to prove them. For the proofs that I looked into, I took some time to point out the reasoning behind some of the statements, which I thought were a bit rushed-over. Should I review these topics later on, I believe this post will undoubtedly be a helpful guide to refresh my memory. This is the end of the post, and as always, I hope you enjoyed reading it.  

