---
title: "Recurrent Networks"
date: 2022-01-28
toc: true
mathjax: true
categories:
  - study
tags:
  - RNNs
---

This post covers the definition, principles, and basic workings of recurrent neural nets. The materials referenced in this post include the obvious ones, such as Stanford's [CS224n lectures](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/), but also Afshine and Shervine Amidi's recurrent neural networks [cheatsheet](https://stanford.edu/~shervine/teaching/cs-230/cheatsheet-recurrent-neural-networks) and [CS230](https://stanford.edu/~shervine/teaching/cs-230/).

# Introduction

Recurrent nets are useful structures for processing sequences. A net is *recurrent* in the sense that it involves parameter sharing between timesteps. Specifically, to calculate the hidden state at timestep $t$, we multiply a set weight matrix to the previous hidden state, add the dot product between another set weight matrix and the input, and finally add a fixed bias. Then at each timestep, we find the probability distribution of the next piece of the sequence, compare this distribution with the one-hot encoded "true" vector, and calculate loss with cross entropy. Weight update, involving a process called BPTT, or "backpropagation through time," includes adding the loss at each timestep and using gradient descent w.r.t. a specific weight. 


Let's take the example of a language model, for instance, to illustrate the above process. Assume that at timestep $t$, a new word input gets converted into the word vector $x$, with dimension $V$.  

$$
x^{(t)} \in \mathbb{R}^{|V|} \tag{1}
$$

$$$$
That word vector $x$ then is converted to another vector $e$, through a word embedding matrix $E$. 

$$
e^{(t)}=E \cdot x^{(t)}  \tag{2}
$$

$$$$
Then considering the hidden state $h^{(t-1)}$ at timestep $t-1$, as well as the weights $W_{h}$ and $W_{e}$, we find the hidden state  $h^{(t)}$ at timestep $t$.

$$
h^{(t)}=\sigma\left(W_{h} \cdot h^{(t-1)}+W_{e} \cdot e^{(t)}+b_{1}\right)  \tag{3}
$$

$$$$
With the hidden state $h^{(t)}$ at timestep $t$, all that is left is to find the probability distribution $\hat{y}^{(t)}$, which describes the prospective probability of the word at timestep $t+1$. 

$$
\hat{y}^{(t)}=\operatorname{softmax}\left(U \cdot h^{(t)}+b_{2}\right)  \tag{4}
$$

A key advantage to a recurrent structure, according to the *Deep Learning* book, is the following:
 
> Most recurrent networks can also process sequences of variable length. $$$$ -*Deep Learning*-

Indeed, with recurrent structures, we do not need a longer or bigger model to process a longer input sequence.

Recurrent neural networks are employed in the following fields: 

  + Language models: 
  + Part-of-speech tagging
  + Named entity recognition
  + Sentiment classification
  + Speech recognition 
  + etc. 

## Problems with RNNs

Notwithstanding the advantages, all recurrent structures--especially the barebone minimum ones that don't use any regularization--suffer from several issues, among which these two below are perhaps the most famous.  

  + Difficulty in remembering information across many timesteps
  + Vanishing or exploding gradient problems

To update a specific weight, one has to compute the gradient of the loss function with respect to that weight--a process that involves applying the chain rule multiple times. In such a process, multiplying small values repeatedly leads to an exponential decay of numerical value, a phenomenon often referred to as the vanishing gradient problem. In fact, this problem is particularly acute when trying to remember a particular piece of information, such as an important word, amidst a long sequence of words. For a language model, attempting to reference information presented in a word that appeared at the very beginning of a sentence (for the sake of generating a new word) may prove difficult due to the vanishing gradient problem. On the other side of the coin is the exploding gradient problem--the exact opposite of the vanishing gradient problem. A 2013 paper titled "On the difficulty of training recurrent neural networks" by Pascanu, Mikolov, and Bengio provides a more detailed proof of the vanishing and exploding gradient problems, but here's a brief mathematical sketch of the proof.

We begin with the widely known equation for a recurrent network's hidden state. 

$$h^{(t)}=\sigma\left(W_{h} \cdot h^{(t-1)}+W_{x} \cdot x^{(t)}+b_{1}\right) \tag{10}$$

$$$$

Suppose $ \mathbf{z} = f(\mathbf{x})$. Then each element of matrix $\frac{\partial{\mathbf{z}}}{\partial{\mathbf{x}}}$ can written as the following. 
 
$$ \left(\frac{\partial \mathbf{z}}{\partial \mathbf{x}}\right)_{i j}=\frac{\partial z_{i}}{\partial x_{j}}=\frac{\partial f\left(x_{i}\right)}{\partial x_{j}}=\left\{\begin{array}{l}
f^{\prime}\left(x_{i}\right) \;\; \text{(if $i=j$)}\\
0 \;\; \text{(if otherwise)}
\end{array}\right.$$

And thus, 
$$\frac{\partial \bf{z}}{\partial \bf{x}}=\operatorname{diag}\left(f^{\prime}(x)\right).$$

$$$$

Going back to equation 10, taking one hidden state vector's partial derivative with respect to the previous hidden state vector yields the following result. 


$$\frac{\partial h^{(t)}}{\partial h^{(t-1)}}=\frac{\partial\left[\sigma\left(W_{h} \cdot h^{(t-1)}+W_{x} \cdot x^{(t)}+b_{1}\right)\right]}{\partial h^{(t-1)}} \tag{11}$$

$$=\operatorname{diag}\left(\sigma^{\prime}\left(W_{h} h^{(t-1)}+W_{x} \cdot x^{(t)}+b_{1}\right)\right) W_{h} \tag{12}$$

$$$$

If we take a vector derivative of loss function $J^{(i)}$ with respect to the hidden state vector at previous timestep $j$, using the chain rule, we get a bunch of products of the term at equation (12). 


$$\frac{\partial J^{(i)}(\theta)}{\partial h^{(j)}}=\frac{\partial J^{(i)}(\theta)}{\partial h^{(i)}} \cdot \prod_{j<t \leq i} \frac{\partial h^{(t)}}{\partial h^{(t-1)}}$$

$$$$

Using equation (12) and pulling out $W_{h}$ to the front, we see that the repeated dot product of the term may result in a case where that term *and* the gradient both converge to zero--especially if $W_{h}$ consist of small terms. 


$$=\frac{\partial J^{(i)}(\theta)}{\partial h^{(i)}} \cdot \underbrace{W_{h}^{(i-j)}}_{\text {(converges to zero) }} \cdot \prod_{j<t \leq i} \operatorname{diag}\left(\sigma^{\prime}\left(W_{h} h^{(t-1)}+W_{x} \cdot x^{(t)}+b_{1}\right)\right)$$

## Solutions



There are several strategies for dealing with gradient issues, and some of the most widely-known ones include the following: 

+ Structure-wise
  + LSTMs
  + GRUs
  + Multilayer, bidirectional structures
 
+ Other strategies
  + Teacher forcing
  + Gradient clipping (exploding)


Note that L1 or L2 regularization are strategies for dealing with overfitting by penalizing large weight values, and are not solutions to the vanishing gradient issue. In fact, both reduce the values of the weight matrix, making the vanishing gradient problem worse. Adding more layers will also not help solve the problem, as doing so will result in more instances of the weight matrix being multiplied. 

# Conclusion

It has been over thirty-five years since the first recurrent network structure was proposed in 1986, and over twenty years since the first solutions to the gradient decay issues were suggested. In addition, nearly ten years have passed since the "new kids on the block," such as transformers and GRUs, alongside the older ones, such as long short-term memory, were invented. In that sense, I feel as though I should have written this post a long time ago, especially given that I first started studying about this topic six months ago. Hopefully, I will be more diligent in finishing the writing work on time. In the next posts, LSTMs and other variations of basic recurrent structures will be discussed. 
