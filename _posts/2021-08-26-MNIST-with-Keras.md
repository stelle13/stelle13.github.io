---
title: "MNIST with Keras"
date: 2021-08-26
mathjax: true
toc: true
categories:
  - study
tags:
  - Keras
  - Tensorflow
---



In this post, I will discuss my first attempt at creating a neural network with Keras, in which I will classify hand-written digits using the MNIST dataset. 



## Recap

As anyone who has looked up the relationship between A.I., M.L, and D.L would know, deep learning is a subset of machine learning that uses neural networks. As such, when I first started studying machine learning, I didn't jump into deep learning immediately, instead experimenting with simple ML models, such as naive Bayes, SVM, and k-means clustering. 

In the process, during summer break, I ventured into learning R--a statistical tool that I would end up learning in a statistical methodology course next semester. Although still somewhat novice at R, I was delighted to learn a data analysis language other than Python. 

And recently, I finally had the opportunity to learn elementary deep learning with Tensorflow--a task that first seemed daunting to be honest. After searching for the best books to learn deep learning for beginners on Google, I stumbled upon three books: *Deep Learning* by professor Yoshua Bengio, *Deep learning with Python* by François Chollet, and *Hands-On Machine Learning with Scikit* by Aurelien Geron. While flipping through *Deep Learning* at my college library, I figured that my mathematics background was insufficient to cover the entirety of the math in the text, while Geron's and Chollet's seemed perfect--in fact, this post's code is from Chollet's book. For the mathematics behind the model, I looked to Youtube videos for assistance, most notably those of [1blue3brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw) and [StatQuest](https://www.youtube.com/user/joshstarmer). 

As a side note, today I joined Tensorflow KR, a Facebook group of about 50,000 members dedicated to encouraging the use of Tensorflow in South Korea. I hope that in the near future, I will be able to make use of more communities like these in my quest of learning machine learning. 


### Tensors

François Chollet defines a tensor simply as a "container for data," with the dimension of the tensor determining the tensor's shape. For example, a matrix, which has two axes, is a 2D tensor, whereas a vector, which only has one axis, is a 1D tensor. In TensorFlow, tensors are stored as `Numpy` arrays, and the tensor's  dimension can be checked with `.ndim`.   

The expression "number of axes" is interchangeable with the term "rank"--for example, a 2D tensor (matrix) has two axes, so the rank is two. The "shape" of a tensor describes how many elements a tensor has along certain dimensions. To give a more concrete example, a 3D tensor containing 60,000 matrices that consist of twenty-eight vectors with twenty-eight elements each has the shape (60,000, 28, 28). 

## Implementation: Part One

In this post, I will be using the MNIST dataset, supplied by `keras` to create a simple two-layer neural network.

To do that, first import the MNIST dataset from `keras.datasets` module. `train_images` is the 3D Numpy tensor whose shape is `(60,000, 28, 28)`, meaning that it consists of 60,000 images of 28 x 28 pixels. The goal is to use layers to extract information from the training dataset, as well as to fine-tune the weight and bias tensors in the layers by using the training labels, a loss function, and an optimizer. 



```python
from keras.datasets import mnist
(train_images, train_labels), (test_images, test_labels) = mnist.load_data()
```

    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/mnist.npz
    11493376/11490434 [==============================] - 0s 0us/step
    11501568/11490434 [==============================] - 0s 0us/step



```python
print(train_images.shape)
print(test_images.shape)
```

    (60000, 28, 28)
    (10000, 28, 28)


Below is the first image, in the form of a matrix (2D tensor). The images in the 60,000-images training set are all 28 x 28 pixel images, meaning that in the 2D tensor below, one can see 28 vectors consisting of 28 elements. The grayscale images are, in fact, a series of vectors whose elements' magnitudes signify the darkness of the pixels--zero for white, greater for gray or black. 


```python
train_images[1] # first image
```




    array([[  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,  51, 159, 253, 159,  50,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,  48, 238, 252, 252, 252, 237,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
             54, 227, 253, 252, 239, 233, 252,  57,   6,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,  10,  60,
            224, 252, 253, 252, 202,  84, 252, 253, 122,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0, 163, 252,
            252, 252, 253, 252, 252,  96, 189, 253, 167,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,  51, 238, 253,
            253, 190, 114, 253, 228,  47,  79, 255, 168,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,  48, 238, 252, 252,
            179,  12,  75, 121,  21,   0,   0, 253, 243,  50,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,  38, 165, 253, 233, 208,
             84,   0,   0,   0,   0,   0,   0, 253, 252, 165,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   7, 178, 252, 240,  71,  19,
             28,   0,   0,   0,   0,   0,   0, 253, 252, 195,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,  57, 252, 252,  63,   0,   0,
              0,   0,   0,   0,   0,   0,   0, 253, 252, 195,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0, 198, 253, 190,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0, 255, 253, 196,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  76, 246, 252, 112,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0, 253, 252, 148,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  85, 252, 230,  25,   0,   0,   0,
              0,   0,   0,   0,   0,   7, 135, 253, 186,  12,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  85, 252, 223,   0,   0,   0,   0,
              0,   0,   0,   0,   7, 131, 252, 225,  71,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  85, 252, 145,   0,   0,   0,   0,
              0,   0,   0,  48, 165, 252, 173,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  86, 253, 225,   0,   0,   0,   0,
              0,   0, 114, 238, 253, 162,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  85, 252, 249, 146,  48,  29,  85,
            178, 225, 253, 223, 167,  56,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  85, 252, 252, 252, 229, 215, 252,
            252, 252, 196, 130,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,  28, 199, 252, 252, 253, 252, 252,
            233, 145,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,  25, 128, 252, 253, 252, 141,
             37,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0],
           [  0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,   0,
              0,   0]], dtype=uint8)



The code block down below show the digits as `matplotlib`'s `pyplot` images. 


```python
digit = train_images[7]
# digit1 = train_images[7, 14:, 14:] # bottom right corner, 14 pixels by 14 pixels
digit2 = train_images[18]
digit3 = train_images[8]

import matplotlib.pyplot as plt
plt.imshow(digit, cmap=plt.cm.binary)
# plt.imshow(digit1, cmap=plt.cm.binary)
plt.show()

import matplotlib.pyplot as plt
plt.imshow(digit2, cmap=plt.cm.binary)
# plt.imshow(digit1, cmap=plt.cm.binary)
plt.show()

import matplotlib.pyplot as plt
plt.imshow(digit3, cmap=plt.cm.binary)
# plt.imshow(digit1, cmap=plt.cm.binary)
plt.show()
```


​    
![png](1_files/1_9_0.png)
​    




![png](1_files/1_9_1.png)
    




![png](1_files/1_9_2.png)
    


Then two `Dense` layers will be added.  


```python
from keras import models
from keras import layers
network = models.Sequential()
network.add(layers.Dense(512, activation='relu', input_shape=(28 * 28,)))
network.add(layers.Dense(10, activation='softmax'))
```

Before proceeding to the next set of code blocks, the following terms must be defined. 

1. ReLu activation function
2. Softmax activation function
3. Categorical cross-entropy function
4. Epochs
5. Batch size


## Activation Function

To discuss activation functions, one must be aware of the composition of the basic building block behind layers--the node. The node, or the artificial neuron, is made up of three parts: the synapses providing the weights, the adder that calculates the sum of the weighted inputs, and the activation function that takes the sum as an input and produces an output. An image from this [blog](https://morioh.com/p/d70aa769173a) by Abdul Larson shows a clear representation of a neural network node. 





The input for an activation function is as follows: 

$$ v = w_0 + \sum_{j=1}^{m} w_jx_j $$

For the first layer, all the nodes have `relu` as the activation function. `relu` is short for "rectified linear activation function," the form of which is $max(0,x)$. Basically, if the input $v$ is negative, the node ouputs zero, while if the input is positive, the node ouputs the input itself. 

The second layer, which is the last one, has `softmax` as the activation function. The softmax activation function is most widely used for the last layer, espcially if the desired output is a multinomial ("multi-class") probability distribution. In this example, we are classifying images into ten categories--from zero to nine. Because we want a probability distribution at the end that tells us which category, or output number, is the most probable for an image,  we use the `softmax` activation function. 

The softmax activation function is as follows:


$$ softmax(z)= 
\begin{bmatrix} \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}  \\ \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}  \\ ...\\\frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}  \end{bmatrix} $$




The an image from this [paper](https://www.researchgate.net/figure/Working-principles-of-softmax-function_fig3_349662206), neatly summarizes the role of the softmax activation function. 

The output of a softmax activation function may look as follows: $[0.09003057, 0.66524096, 0.24472847]$. Since the elements of the array are probabilities, they sum up to one. To convert this output to a single integer, we use the `argmax` function, which returns the index of the greatest element. In this example, the output of `argmax` would be one.

The more complete picture of the interaction between the input layer and the softmax output layer is provided below. The [diagram](https://www.researchgate.net/figure/Softmax-layer-with-neural-network-31_fig2_349823091) below, which is under a [Creative Commons license](https://creativecommons.org/licenses/by-sa/4.0/) for redistribution, was taken from this [paper](https://www.researchgate.net/publication/349823091_Deep_learning_model_for_glioma_meningioma_and_pituitary_classification) by A. Sadoon, T. Mohammed, and A. Mohammed. 

![dia25.PNG]

In the input layer, each of the nodes produces a scalar value $x_j$. A total of $n$ scalar values make up the input vector $\textbf{x}$, and the $j$th node is matched to the transpose of vector $\textbf{w}_j^{T}$ to produce $z_j$ as a cross-product output. That is, the $n$th node is matched with $\textbf{w}_n^{T}$ to produce $z_n = (\textbf{w}_n)^{T} \cdot \textbf{x}  + b$ as an output. Then the vector whose elements are these output values is fed into the softmax function to produce the probability distribution that is visible to the right.  


## Cross Entropy Function

Then there's the loss function and the optimizer. There are two types of cross-entropy functions: binary and categorical. In this example, the loss function is set to be categorical cross entropy. 

The equation for categorical-cross entropy is as follows, with the log being base two:

$$ CE = -\sum_{i=1}^{C} t_i \cdot log(f(\textbf{z})_i) $$

$$ f(\textbf{z})_i := \frac{e^{z_i}}{\sum_{j=1}^{C} e^{z_j}} $$

* $ f(\textbf{z})_i$ : Probability for each class after applying softmax. 
* $C$ is the total number of classes. 

This Youtube [video](https://www.youtube.com/watch?v=ILmANxT-12I&t=12s&ab_channel=BadriAdhikari) by Badri Adhikari concisely summarizes the concept. 


Applied to an example softmax layer in this [blog](https://towardsdatascience.com/cross-entropy-loss-function-f38c4ec8643e), it becomes clear that the purpose of the loss function is to calculate the distance between the predicted results and the actual labels. The arithmetic involved in the process is shown below, which provides 0.3677 as the loss score. 


$$ CE = -\sum_{i=1}^{C} t_i \cdot log(f(\textbf{z})_i) \\ = [1 \cdot log_2(0.775) + 0 \cdot log_2(0.116) + 0 \cdot log_2(0.039) + 0 \cdot log_2(0.07)] 
= 0.3677 $$

The role of the optimizer then, is to use the loss score provided by the loss function to adjust the weights of the layers, in a process known as *backpropagation*.


## Epochs and Batches

In stochastic gradient descent, there are two hyperparameters called "batch" and "epoch." To put it simply, a *batch* is the number of training samples that go through the layers before the weight and bias parameters of the layers are updated *once.* An *epoch* is the number of times that the entire training set goes through the layers. 

When graphing the error provided by the loss function, the method of finding the minimum of this graph is called the optimization process. The most famous optimization process is called *gradient descent*, which is essentially "following down the slope or *gradient*" to find the optimum weight combination that minimizes the error. The step size and the learning rate are two parameters that determine how gradient descent tunes the weights. 

To be more specific, the amount that the weights are updated after one batch is the value: weight error $\cdot$ learning rate. Here, the weight error is the estimated proportion of the error attributable to the weights in the layers. In fact, the reason why the learning rate is named as such is precisely because it determines the rate at which the model "learns." Determining the step size is usually a hands-on, empirical process done by trial and error, although techniques to speed up this process using momentum do exist. The topic of momentum will be discussed in another post, but momentums do not affect the step size. 

More on gradient descent however, this excellent [post](https://machinelearningmastery.com/difference-between-a-batch-and-an-epoch/) by Jason Brownlee does a good job of explaining the differences between stochastic, mini-batch, and batch gradient descent. When assigning $n$ samples to a batch, the following naming conventions hold: 

* stochastic gradient descent: $n=1$
* mini-batch gradient descent: $ 1 < n < $ training sample size
* batch gradient descent: $n = $ training sample size

An epoch may contain many batches (stochastic or mini-batch g.d.), or in the case of batch-gradient descent, one epoch may consist of one batch only. 



## Implementation: Part Two

Now that importing the dataset, defining the training/testing images/labels, and creating layers have all been done, the next step is to specify the loss function and the optimizer. In this example, the loss function and the optimizer will be designated as categorical cross entropy and RMSProp, each respectively. 


```python
network.compile(optimizer='rmsprop',
loss='categorical_crossentropy', 
metrics=['accuracy'])




```


```python
train_images = train_images.reshape((60000, 28 * 28))
train_images = train_images.astype('float32') / 255
test_images = test_images.reshape((10000, 28 * 28))
test_images = test_images.astype('float32') / 255
```


```python
from tensorflow.keras.utils import to_categorical
train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)
```


```python
network.fit(train_images, train_labels, epochs=5, batch_size=128)
```

    Epoch 1/5
    469/469 [==============================] - 6s 12ms/step - loss: 0.2569 - accuracy: 0.9250
    Epoch 2/5
    469/469 [==============================] - 6s 12ms/step - loss: 0.1014 - accuracy: 0.9700
    Epoch 3/5
    469/469 [==============================] - 5s 10ms/step - loss: 0.0666 - accuracy: 0.9802
    Epoch 4/5
    469/469 [==============================] - 5s 12ms/step - loss: 0.0482 - accuracy: 0.9857
    Epoch 5/5
    469/469 [==============================] - 5s 11ms/step - loss: 0.0371 - accuracy: 0.9888





    <keras.callbacks.History at 0x7fe45c736d50>




```python
test_loss, test_acc = network.evaluate(test_images, test_labels)
print('test_acc:', test_acc)
```

    313/313 [==============================] - 1s 3ms/step - loss: 0.0644 - accuracy: 0.9799
    test_acc: 0.9799000024795532


After five epochs, which is tiny when compared to deep learning models trained with thousands of epochs, one can see that the neural network has achieved 98.88% accuracy on predicting hand-written digits. With the testing set, the accuracy falls to 97.99%, which is due to *overfitting*--the problem of tuning the model too much based on the training set. 

## Conclusion

This post marks my first attempt at using Tensorflow to create a deep learning model. In the near future, I hope to go for more interesting tasks using Tensorflow, such as those of computer vision and natural language processing. I hope you enjoyed this post, and thanks for reading. 

