---
title: "Backprop with LSTM"
date: 2022-02-15
toc: true
categories:
  - study
tags:
  - LSTM
  - Backprop
---

# Background

In the last post, I discussed some of the most famous problems with recurrent networks--namely, the vanishing and exploding gradient problems that can throw a wrench in the works of recurrent backpropagation. These issues are also known as long-term dependency problems, where a recurrent network, such as a language model, needs a particular piece of information presented at the beginning of a time-series sequence to predict the next part of that sequence. When applying the chain rule with respect to all the dependencies, if a gradient value too small or large comes across multiple timesteps, gradient update through backprop becomes almost impossible. 

To resolve the problem associated with long-term dependencies, gated recurrent nets with a variety of structures were proposed, such as the famous [long short-term memory](https://direct.mit.edu/neco/article-abstract/9/8/1735/6109/Long-Short-Term-Memory?redirectedFrom=fulltext) by Hochreiter and Schmidhuber (1997) and the [gated recurrent unit](https://arxiv.org/abs/1406.1078) by Cho et al (2014). 


The long short-term memory is a type of gated recurrent net with three gates, called "input, output, and forget gates" that add or eliminate information from the cell state, a kind of pipeline where information can flow between the hidden states of different timesteps. The three gates each perform different functions that can be surmised from their names; the input, forget, and output gates add, eliminate, and produce information in the form of cell states and hidden states, each respectively. The three gates, armed with their own weights and biases, all have a sigmoid activation part at the end—ensuring that the gates' outputs are between zero and one. These output values, in turn, signify what percentage of information should be allowed through, or conversely, obstructed. 


Unlike the input and forget gates, the role of the output gate is to determine the information that will be included in the next hidden state. On the contrary, the cell state determines what information that will be preserved for future timesteps. 

To summarize, three inputs are necessary for a single unit of long-short term memory: $c_{t-1}$ and $h_{t-1}$—the cell and hidden states of the previous timestep—and $x_t$, the current input. The unit then produces two outputs: $c_{t}$ and $h_{t}$, the cell and hidden states of the current timestep. Overall, the long-short term memory is not a perfect solution to the long-term dependency issue, as it still suffers from the exploding gradient problem, as argued by [Greff, et al (2016)](https://arxiv.org/abs/1503.04069v2). 

# Structure

The below six equations describe the structure of long short-term memory, where $f_{t}$, $o_{t}$, and $i_{t}$ are the forget, output, and input gate values. The other variables are for the intermediate values used to find the hidden and cell states. The $\odot$ or $\circ$ operators both denote the hadamard product, or element-wise multiplication operator.  

$$f_{t}=\sigma\left(W_{f} x_{t}+U_{f} h_{t-1}+b_{f}\right)$$

$$i_{t}=\sigma\left(W_{i} x_{t}+U_{i} h_{t-1}+b_{i}\right)$$

$$o_{t}=\sigma\left(W_{0} x_{t}+U_{0} h_{t-1}+b_{0}\right)$$

$$\tilde{c}_{t}=\tanh \left(W_{c} x_{t}+U_{c} h_{t-1}+b_{c}\right)$$

$$c_{t}=f_{t} \circ c_{t-1}+i_{t} \circ \tilde{c}_{t}$$

$$h_{t}=o_{t} \circ \tanh \left(c_{t}\right)$$




# Backpropagation

In any recurrent network, to apply the chain rule to update a particular weight through backpropagation, one must first identify all the dependencies associated with that weight. This involves essentially "backtracking" all the steps needed to calculate the loss function, and with a long short-term memory, there may be several thousands of weights to update, depending on how many cells are in each layer. But in this post, I will discuss how to update one particular kind of weight—the input weight $W_{i}$ applied on value $z_{t}=\left[h_{t-1}, x_{t}\right]$, where $h_{t-1}$ and $x_{t}$ are the previous hidden state and new input value vectors. In particular, let's look at how to apply the chain rule to identify and link all the dependencies. This part references Christina Kouridi's [blog post](https://christinakouridi.blog/2019/06/19/backpropagation-lstm/), Arun Mallya's [backpropagation notes](http://arunmallya.github.io/writeups/nn/backprop.html)
), and Kevin Clark's CS224N [gradient notes](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/readings/gradient-notes.pdf).


## Backprop on Input Matrix $W_{i}$

Loss is normally calculated via cross entropy between the predicted softmax distribution $\hat{y}_{t}$ and the one-hot encoded ground truth. 




$$ \begin{array}{l}
J=J (\hat{y}_{t}) \\
\hat{y}_{t}=\operatorname{softmax}\left(\vec{v}_{t}\right)
\end{array} $$

 



We can then apply the chain rule to find the partial derivative of $J$ with respect to $\vec{v}_{t}$. 

$$$$

$$ \left.\begin{array}{l}
J=J (\hat{y}_{t}) \\
\hat{y}_{t}=\operatorname{softmax}\left(\vec{v}_{t}\right)
\end{array}\right\} \rightarrow \frac{\partial J}{\partial \vec{v}_{t}}=\frac{\partial J}{\partial \hat{y}_{t}} \cdot \frac{\partial \hat{y}_{t}}{\partial \vec{v}_{t}} $$ 

The $\frac{\partial \hat{y}_{t}}{\partial \vec{v}_{t}}$ expression is a matrix with the following elements, and the detailed derivations of the softmax operator can be found at this [site](https://deepnotes.io/softmax-crossentropy). 

$$$$

$$
\frac{\partial \hat{y_{i}}}{\partial \hat{v_{j}}}=\left\{\begin{array}{ll}
y_{i}\left(1-y_{j}\right) & \text { if } i=j \\
-y_{j} \cdot y_{i} & \text { if } i \neq j
\end{array}\right.$$

The $\vec{v}_{t}$ is calculated via applying the weight matrix $W_{v}$ and bias vector $\vec{b}_v$ to the hidden state.  

$$ \begin{array}{l}
\hat{y}_{t}=\operatorname{softmax}\left(\vec{v}_{t}\right) \\
\vec{v}_{t}=W_{v} \cdot \vec{h}_{t}+\vec{b}_{v}
\end{array}$$


We then apply the chain rule, similar to the above. 

$$$$

$$ \left.\begin{array}{l}
\hat{y}_{t}=\operatorname{softmax}\left(\vec{v}_{t}\right) \\
\vec{v}_{t}=W_{v} \cdot \vec{h}_{t}+\vec{b}_{v}
\end{array}\right\} \rightarrow \frac{\partial J}{\partial \vec{h}_{t}}=\frac{\partial J}{\partial \vec{v}_{t}} \cdot \frac{\partial \vec{v}_{t}}{\partial \vec{h}_{t}} \tag{1}$$ 


Notice that we already calculated the value $\frac{\partial J}{\partial \vec{v}_{t}}$in the above steps, so simply plug in that value for $\frac{\partial J}{\partial \vec{v}_{t}}$ in $(1)$. Since $\vec{v}_{t}=W_{v} \cdot \vec{h}_{t}+\vec{b}_{v}$, equation $(1)$ turns out like below. 

$$ \frac{\partial J}{\partial h_{t}} = {W}^\top_{v} \cdot \frac{\partial J}{\partial v_{t}} $$

Next in our backtracking sequence is calculating $\vec{h}_t$ by taking the hadamard product between $\text{tanh}(\vec{c}_t)$ and $\vec{o}_{t}$. 

$$ \begin{array}{l}
\vec{v}_{t}=W_{v} \cdot \vec{h}_{t}+\vec{b}_{v} \\
\vec{h}_{t}=\vec{o}_{t} \odot \tanh \left(\vec{c}_{t}\right)
\end{array}$$

Apply the chain rule once more, 


$$$$
$$ \left.\begin{array}{l}
\vec{v}_{t}=W_{v} \cdot \vec{h}_{t}+\vec{b}_{v} \\
\vec{h}_{t}=\vec{o}_{t} \odot \tanh \left(\vec{c}_{t}\right)
\end{array}\right\} \rightarrow \frac{\partial J}{\partial \vec{c}_{t}}=\frac{\partial J}{\partial \vec{h}_{t}} \cdot \frac{\partial \vec{h}_{t}}{\partial \vec{c}_{t}} \tag{2}$$ 
$$$$


This step is a bit more tricky, as it involves differentiating with respect to a term surrounded by a [hyperbolic tangent function](https://mathworld.wolfram.com/HyperbolicTangent.html) and the hadamard product. Referencing Arun Mallya's [blog post](http://arunmallya.github.io/writeups/nn/backprop.html), we end up with this expression. 


$$\frac{\partial J}{\partial {h}_{t}} \cdot \frac{\partial \vec{h}_{t}}{\partial \vec{c}_{t}} = \frac{\partial J}{\partial h_{t}} \odot o_{t} \odot (1-tanh(c_{t})^2)$$

Next in the line is finding $\vec{c}_t$. 

$$ \begin{array}{l}
\vec{h}_{t}=\vec{o}_{t} \odot \tanh \left(\vec{c}_{t}\right) \\
\vec{c}_{t}=\vec{i}_{t} \odot \hat{c}_{t}+\vec{f}_{t} \odot \vec{c}_{t-1} 
\end{array} $$

Again, apply the chain rule, and substitute the value for $\frac{\partial J}{\partial \vec{c}_{t}}$ that we found in equation $(2)$ in equation $(3)$.

$$$$
$$ \left.\begin{array}{l}
\vec{h}_{t}=\vec{o}_{t} \odot \tanh \left(\vec{c}_{t}\right) \\
\vec{c}_{t}=\vec{i}_{t} \odot \hat{c}_{t}+\vec{f}_{t} \odot \vec{c}_{t-1} 
\end{array}\right\} \rightarrow \frac{\partial J}{\partial \vec{i}_{t}}=\frac{\partial J}{\partial \vec{c}_{t}} \cdot \frac{\partial \vec{c_{t}}}{\partial \vec{i}_{t}} \tag{3}$$ 
$$$$

$$\frac{\partial J}{\partial \vec{i}_{t}} = \frac{\partial J}{\partial \vec{c}_{t}} \odot \hat{c}_{t}
$$

But since the structure of long short-term memory requires that we find $\vec{i}_t$ by applying $\text{softmax}$ to $\vec{a}_{i}=W_{i} \cdot \vec{z}_{t}+\vec{b}_{i}$ first, we backtrack once more to these two equations. 


$$ \begin{array}{l}
\vec{c}_{t}=\vec{i}_{t} \odot \hat{c}_{t}+\vec{f}_{t} \odot \vec{c}_{t-1}  \\
\vec{i}_{t}=\sigma\left(\vec{a}_{i}\right)
\end{array}$$

Apply the chain rule here, and we get the following. 

$$$$
$$ \left.\begin{array}{l}
\vec{c}_{t}=\vec{i}_{t} \odot \hat{c}_{t}+\vec{f}_{t} \odot \vec{c}_{t-1}  \\
\vec{i}_{t}=\sigma\left(\vec{a}_{i}\right)
\end{array}\right\} \rightarrow \frac{\partial J}{\partial a_{i}}=\frac{\partial J}{\partial \vec{i}_{t}} \cdot \frac{{\partial \vec{i}_{t}}}{\partial a_{i}} $$ 
$$$$


 

The  expression $\frac{{\partial \vec{i}_{t}}}{\partial a_{i}}$ is a matrix with $\frac{\partial \hat{i}_{t}}{\partial \hat{a}_{i}}$ as elements, defined in the following manner. 

$$
\frac{\partial \hat{i}_{t}}{\partial \hat{a}_{i}}=\left\{\begin{array}{ll}
i_{a}\left(1-i_{b}\right) & \text { if } a=b \\
-i_{a} \cdot i_{b} & \text { if } a \neq b
\end{array}\right.$$

Finally, take the partial derivative with respect to the weight matrix $W_{i}$. 

$$$$
$$ \left.\begin{array}{l}
\vec{i}_{t}=\sigma\left(\vec{a}_{i}\right) \\
\vec{a}_{i}=W_{i} \cdot \vec{z}_{t}+\vec{b}_{i} 
\end{array}\right\} \rightarrow \frac{\partial J}{\partial W_{i}}=\frac{\partial J}{\partial a_{i}} \cdot \frac{\partial a_{i}}{\partial W_{i}} $$ 
$$$$

$$ \frac{\partial J}{\partial W_{i}} = \frac{\partial J}{\partial a_{i}} \cdot {z}^\top_{t}, \;\;\; \text{where}\; z_{t}=\left[h_{t-1}, x_{t}\right]
 $$

# Weight Update

The weight, repeatedly updated by gradient descent, involves the sum of all the values of $\frac{\partial J}{\partial W_{i}}$ that we found above at each timestep. 


$$ W_{i} ⟵ W_{i}+\alpha \cdot \frac{\partial J}{\partial W_{i}} $$ 

$$ \text{where} \;\; \frac{\partial J}{\partial W_{i}}= \sum^{T}_{t=1} \frac{\partial J}{\partial W_{i}} \bigg|_{t} $$


# Conclusion

In this post, I went over the long short-term memory's structure and briefly introduced how to update a particular weight parameter. In my previous posts, I never exactly discussed backpropagation and parameter update at length, rather choosing to use the convenient modules in PyTorch and Tensorflow and brushing off the specifics. But by backtracking all the steps and tracing all the dependencies, I believe I realized just how conveniently the modules pack all the intermediate processes into neat one-liner functions. In that sense, I will end this post on a note of praise and respect for the engineers at Google and Facebook. 

# Citations:

Cho, Kyunghyun, et al. "Learning phrase representations using RNN encoder-decoder for statistical machine translation." arXiv preprint arXiv:1406.1078 (2014).

Greff, Klaus, et al. "LSTM: A search space odyssey." IEEE transactions on neural networks and learning systems 28.10 (2016): 2222-2232.

Hochreiter, S, Schmidhuber, J.; Long Short-Term Memory. Neural Comput 1997; 9 (8): 1735–1780. doi: https://doi.org/10.1162/neco.1997.9.8.1735

 
