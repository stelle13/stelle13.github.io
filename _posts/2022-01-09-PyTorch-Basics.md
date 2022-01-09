---
title: "PyTorch Basics"
date: 2022-01-09
toc: true
mathjax: true
categories:
  - study
tags:
  - PyTorch
---



# Motivation

This post covers my attempts at learning PyTorch--a framework that I had long intended to use, but never exactly had time to master in depth. Whereas experimenting with TensorFlow had been the main content of this blog, trying to create future posts for readers using PyTorch while not knowing it in detail seemed like a case of the blind leading the blind--which lead me to write this post as as an attempt to set PyTorch basics in stone for my memory.  



# Tensors

First, let's import all the necessary modules and functions. In PyTorch, the `torch.nn` class provides a comprehensive collection of widely-used layer, activation, and loss functions--all packaged nicely inside the class. 




```python
import torch
import torch.nn as nn

import pprint
import torch.nn as nn
import torch.nn.functional as F

pp = pprint.PrettyPrinter()


```

Like TensorFlow, all data that will be processed with PyTorch modules must be in a set format--in this case, a tensor. For more information on PyTorch tensors, check out this [tutorial](https://pytorch.org/tutorials/beginner/blitz/tensor_tutorial.html#sphx-glr-beginner-blitz-tensor-tutorial-py).

Tensors can be initialized by converting arrays, lists, or even other tensors using `torch.tensor()`.

Conversion 

- NumPy → Tensor
- List → Tensor
- Tensor → Tensor



```python
data = [0,1]

print(torch.tensor(data))
print(torch.tensor(data, dtype = torch.float))
print(torch.tensor(data, dtype = torch.bool))
```

    tensor([0, 1])
    tensor([0., 1.])
    tensor([False,  True])
    

Specifying the dimensions of the desired tensor is often more efficient than writing every number from scratch. Below are some examples of tensor manipulations, such as reshaping and taking the transpose. 


```python
tensor234 = torch.zeros((2,3,4)) # 2 by 3 by 4 tensor of 1s
tensor234
```




    tensor([[[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]],
    
            [[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]]])




```python
tensor234.shape
```




    torch.Size([2, 3, 4])




```python
torch.reshape(tensor234, (3,2,4)) # reshaped from 2 X 3 X 4 tensor to 3 X 2 X 4

```




    tensor([[[0., 0., 0., 0.],
             [0., 0., 0., 0.]],
    
            [[0., 0., 0., 0.],
             [0., 0., 0., 0.]],
    
            [[0., 0., 0., 0.],
             [0., 0., 0., 0.]]])




```python
print(tensor234, end='\n\n')
print(tensor234 + 1)
```

    tensor([[[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]],
    
            [[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]]])
    
    tensor([[[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]],
    
            [[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]]])
    


```python
print(tensor234, end='\n\n') # original: 2 by 3 by 4
print(tensor234.T) # transpose: 4 by 3 by 2
```

    tensor([[[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]],
    
            [[0., 0., 0., 0.],
             [0., 0., 0., 0.],
             [0., 0., 0., 0.]]])
    
    tensor([[[0., 0.],
             [0., 0.],
             [0., 0.]],
    
            [[0., 0.],
             [0., 0.],
             [0., 0.]],
    
            [[0., 0.],
             [0., 0.],
             [0., 0.]],
    
            [[0., 0.],
             [0., 0.],
             [0., 0.]]])
    


```python
print(tensor234.shape)
print(tensor234.T.shape)
```

    torch.Size([2, 3, 4])
    torch.Size([4, 3, 2])
    

Dot products can be performed using the `@` operator, and concatenation with `.cat()`. 


```python
# 2 X 3 matrix (dot product) 3 X 2 matrix
torch.Tensor([[1,2,3], [3,2,1]]) @ torch.Tensor([[3,2], [2,1], [3,4]])
```




    tensor([[16., 16.],
            [16., 12.]])




```python
a = torch.Tensor([[1,2,3],
                 [1,2,3]])

print(torch.cat([a,a,a], dim=0))
print(torch.cat([a,a,a], dim=1))
```

    tensor([[1., 2., 3.],
            [1., 2., 3.],
            [1., 2., 3.],
            [1., 2., 3.],
            [1., 2., 3.],
            [1., 2., 3.]])
    tensor([[1., 2., 3., 1., 2., 3., 1., 2., 3.],
            [1., 2., 3., 1., 2., 3., 1., 2., 3.]])
    

# `torch.autograd`

A unique feature of PyTorch is its `torch.autograd`--a package that "provides classes and functions implementing automatic differentiation of arbitrary scalar valued functions," according to the official [PyTorch](https://pytorch.org/docs/stable/autograd.html#default-gradient-layouts) documentation. 

Basically, the `torch.autograd` package allows one to specify a function, take the derivative of the function by applying the `.backward()` function to it, and then calculate the derivative function's output in response to an input value. 


For example, suppose we have input $x = 5$ and a function $y = x^2$. The derivative is obviously $y = 6x$, and by adding `.backward()` to `y`, we tell the compiler that we wish to later calculate a derivative value of `y`. Then, add `.grad()` to the input value in the `.tensor()` format to save the derivative function as an **attribute** of this specific tensor. Printing `x.grad` yields an output value of the function $y=6x$ saved as the attribute, with the input being tensor $x$'s value. 


```python
x  = torch.tensor([5.], requires_grad=True)
```




    tensor([5.], requires_grad=True)




```python
# Calculating the gradient of y with respect to x

y = 3*x**2    # 특정 함수를 정의하고, 
y.backward()  # 밑에서 그 특정 함수에 .backward()라고 쓰면. 
              # 그 함수의 편미분 값을 계산하겠다는 말이다. 

pp.pprint(x.grad) # d(y)/d(x) = d(3x^2)/d(x) = 6x = 12
              # 특정 tensor에 .grad를 추가하면, 위에서 .backward()라고 쓴 함수...
              # ...함수의 편미분 함수를 이 특정 tensor의 attribute로 저장하고, 
              # 이를 print하면 특정 tensor의 실수값을 attribute로 저장한 편미분 함수에 **대입을 한 값**을 표시한다. 
```

    tensor([30.])
    

# Layers

Neural networks consist of stacks of layers with multiple units, as well as the weights and biases assigned to those units. `torch.nn` is a useful package containing all sorts of layers, such as `.linear()` and etc. 

+ `torch.nn`
  + `nn.Linear`
  + `nn.Conv2d`
  + `nn.Dropout`
  + `nn.MSELoss`
  + ... `etc`
  

Below are some very basic applications of `torch.nn`. 

## Linear Layer

`nn.Linear(4,2)` takes matrix of (n, p, 4) dimensions and outputs a matrix of (n, p, 2)


```python
linear = nn.Linear(4,2)
linear
 
```




    Linear(in_features=4, out_features=2, bias=True)



- in_features = 4, 
- input = (n, p, 4)
- out_features = 2
- output = (n, p, 2)


```python
input = torch.ones(2,3,4)
input # input = (2,3,4)
```




    tensor([[[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]],
    
            [[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]]])




```python
output = linear(input)
output # input = (2,3,2)

```




    tensor([[[-0.3322,  0.1648],
             [-0.3322,  0.1648],
             [-0.3322,  0.1648]],
    
            [[-0.3322,  0.1648],
             [-0.3322,  0.1648],
             [-0.3322,  0.1648]]], grad_fn=<AddBackward0>)



`linear.parameters()` allows us to see the weight and bias tensors. 


```python
list(linear.parameters()) # Ax + b
```




    [Parameter containing:
     tensor([[ 0.0776,  0.1701,  0.1028, -0.3622],
             [-0.4695,  0.0630,  0.2066,  0.3651]], requires_grad=True),
     Parameter containing:
     tensor([-0.3205, -0.0004], requires_grad=True)]



Weight:

$$
\begin{bmatrix} 0.0776 & -0.4695  \\ 0.1701 & 0.0630 \\ 0.1028 & 0.2066 \\ -0.3622 & 0.3651 \\ \end{bmatrix} 
$$

Bias:

$$
\begin{bmatrix} -0.3205 & -0.0004 \\ -0.3205 & -0.0004 \\ -0.3205 & -0.0004 \\ \end{bmatrix}
$$

Let's now visualize the `nn.Linear` layer in more detail.  

$$A \cdot x + b = \text{Output}$$


$$ \begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 1\\ \end{bmatrix} \cdot \begin{bmatrix} 0.0776 & -0.4695  \\ 0.1701 & 0.0630 \\ 0.1028 & 0.2066 \\ -0.3622 & 0.3651 \\ \end{bmatrix} + \begin{bmatrix} -0.3205 & -0.0004 \\ -0.3205 & -0.0004 \\ -0.3205 & -0.0004 \\ \end{bmatrix} = \begin{bmatrix} -0.3322 & 0.1648 \\ -0.3322 & 0.1648  \\ -0.3322 & 0.1648 \\ \end{bmatrix}$$


## Sigmoid Layer


```python
sigmoid = nn.Sigmoid()
output2 = sigmoid(torch.tensor([0.012, 0.043, -0.34, 0.58]))
output2
```




    tensor([0.5030, 0.5107, 0.4158, 0.6411])



## Sequential Layer


```python
def layers(tensorInput):
  block = nn.Sequential(
      nn.Linear(4,2),
      nn.Sigmoid()
  )
  return block(tensorInput) 

# print(type(torch.ones(2,3,4)))

input = torch.ones(2,3,4)
print(input, end='\n\n')
output = layers(torch.ones(2,3,4))
output


```

    tensor([[[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]],
    
            [[1., 1., 1., 1.],
             [1., 1., 1., 1.],
             [1., 1., 1., 1.]]])
    
    




    tensor([[[0.2605, 0.4933],
             [0.2605, 0.4933],
             [0.2605, 0.4933]],
    
            [[0.2605, 0.4933],
             [0.2605, 0.4933],
             [0.2605, 0.4933]]], grad_fn=<SigmoidBackward0>)



## Network of Layers

So in my previous posts, I have dealt with creating some basic convolution neural net structures with TensorFlow, but given that I haven't done so with PyTorch yet, I decided to give it a try. As for creating a complete training pipeline with a comprehensive dataset, that will be reserved for a different post, with this section focusing on setting up a functioning CNN framework. 

Suppose we had a tensor of dimension `shape = [3,5,4,2]`, and let's try to pass this tensor through a `nn.Conv2d` layer. `nn.Conv2d` has the following list of parameters that must be specified, so let's walk through each of them one step at a time.  

`Conv2d(in_channels, out_channels, kernel_size, stride, padding)` 

The first argument is `in_channels`, which is the second number in the input tensor shape, or 5. The `out_channels` is the second argument, which can be any arbitrary integer that we would like to set. Similarly, the kernel size, or the size of the convolution filter, can equally be set as a random integer, as long as it does not exceed the size of the tensor input. Suppose we had the following layer. 

`conv = nn.Conv2d(5, 6, kernel_size=2, stride=1, padding=1)`

Then the shape of the output, which we see as `torch.Size([3, 6, 5, 3])` below, has 3 input channels--the same as the input tensor--and 6 output channels, which is the number specified above. The other two numbers, 5 and 3, are the numbers of output height and width planes in pixels calculated using the two formulas specified [here](https://pytorch.org/docs/1.9.1/generated/torch.nn.Conv2d.html). 




```python
t3322 = torch.zeros(3,5,4,2)

def conv(x):
  conv = nn.Conv2d(5, 6, kernel_size=2, stride=1, padding=1)
  y = conv(x)
  return y

conv(t3322).shape 

# kernel = 2 X 2, stride = 1 (default)
# padding = zero-padding
# padding 1 added to all four sides 

# N: Batch size
# C: Number of Channels
# H: Height
# W: Width
```




    torch.Size([3, 6, 5, 3])



Let's add `relu` and `maxpool` layers to augment the convolution layer above, and see how the output shape `torch.Size([3, 6, 5, 3])` changes. 


```python
def conv2d_relu_maxpool(x):

  conv = nn.Conv2d(5, 6, kernel_size=2, stride=1, padding=1)
  relu = nn.ReLU(inplace=True)
  maxpool = nn.MaxPool2d(kernel_size=2, stride=1)

  conv_Output = conv(x)
  relu_Output = relu(conv_Output)
  maxpool_Output = maxpool(relu_Output)

  outputF = maxpool_Output
  return outputF

conv2d_relu_maxpool(t3322).shape 
```




    torch.Size([3, 6, 4, 2])



As we see above, the expected tensor shape has 4 and 2 as the last two numbers--the products of downsampling operations done by the `maxpool` layer. Now let's add the second `nn.Conv2D`, `relu`, and `maxpool` layers, with 6--the second number in `torch.Size([3, 6, 5, 3])`--as the first argument value in `nn.Conv2D`. 



```python
def partialCnn(x):

  conv = nn.Conv2d(5, 6, kernel_size=2, stride=1, padding=1)
  relu = nn.ReLU(inplace=True)
  maxpool = nn.MaxPool2d(kernel_size=2, stride=1)

  conv2 = nn.Conv2d(6, 4, kernel_size=2, stride=1, padding=1)
  
  conv_Output = conv(x)
  relu_Output = relu(conv_Output)
  maxpool_Output = maxpool(relu_Output)

  conv2_Output = conv2(maxpool_Output)
  relu2_Output = relu(conv2_Output)
  maxpool2_Output = maxpool(relu2_Output)  

  outputF = maxpool2_Output
  return outputF

partialCnn(t3322).shape  
```




    torch.Size([3, 4, 4, 2])




```python
flattened = torch.flatten(partialCnn(t3322), 1)
flattened.shape
```




    torch.Size([3, 32])



The final step is to create a `nn.flatten()` layer, which will flatten the input, and a fully-connected `nn.Linear()` layer. When using the `nn.Linear()` layer, the first parameter must be the last number in the `nn.flatten()` layer output--in this case, 32, or the product of the last three numbers (4,4,2) of the last `nn.maxpool()` layer's output shape. Let's set the `nn.Linear()` layer's output shape as 10. 


```python
def linLayer(x):
  linear = nn.Linear(4*4*2,10) 

  linear_Output = linear(x)
  return linear_Output

linLayer(flattened).shape
```




    torch.Size([3, 10])



By creating a new object that contains the two methods `partialCnn` and `linLayer`, we can easily visualize the entire schematic of the convolutional neural net.


```python
class convolutionNet(nn.Module):   
    def __init__(self):
        super(convolutionNet, self).__init__()

        self.conv = nn.Sequential(
          nn.Conv2d(5, 6, kernel_size=2, stride=1, padding=1),
          nn.ReLU(inplace=True),
          nn.MaxPool2d(kernel_size=2, stride=1),
          nn.Conv2d(6, 4, kernel_size=2, stride=1, padding=1),
          nn.ReLU(inplace=True),
          nn.MaxPool2d(kernel_size=2, stride=1)
        )

        self.linear = nn.Sequential(
            nn.Linear(3*4*4*2,10) 
        )
  
    def forward(self, x):
        convO = self.conv(x)
        # x = self.conv(x)
        flattened = torch.flatten(convO)
        print(flattened.shape)
        x = self.linear(flattened)

        return x
```


```python
net = convolutionNet()
print(net)
```

    convolutionNet(
      (conv): Sequential(
        (0): Conv2d(5, 6, kernel_size=(2, 2), stride=(1, 1), padding=(1, 1))
        (1): ReLU(inplace=True)
        (2): MaxPool2d(kernel_size=2, stride=1, padding=0, dilation=1, ceil_mode=False)
        (3): Conv2d(6, 4, kernel_size=(2, 2), stride=(1, 1), padding=(1, 1))
        (4): ReLU(inplace=True)
        (5): MaxPool2d(kernel_size=2, stride=1, padding=0, dilation=1, ceil_mode=False)
      )
      (linear): Sequential(
        (0): Linear(in_features=96, out_features=10, bias=True)
      )
    )
    


```python
net(t3322)
```

    torch.Size([96])
    




    tensor([ 0.1569, -0.0509, -0.0752,  0.0604,  0.1773, -0.2800, -0.1459,  0.0778,
             0.0178, -0.0364], grad_fn=<AddBackward0>)



Just as a matter of fact, PyTorch offers an easy way of converting an image into a PyTorch tensor. Training a complete convolutional neural net with image input will be reserved for a later post. 


```python
import torch
from torchvision import transforms
from PIL import Image
```


```python
img = Image.open("/content/drive/MyDrive/MilkyWay.jpg")

img
```




    
![png](PyTorch_Basics_files/PyTorch_Basics_56_0.png)
    




```python
trans = transforms.Compose([transforms.ToTensor()])

trans(img)
```




    tensor([[[0.1569, 0.0627, 0.0863,  ..., 0.1686, 0.0824, 0.1176],
             [0.1608, 0.0784, 0.0863,  ..., 0.0000, 0.0706, 0.1255],
             [0.0902, 0.0471, 0.0745,  ..., 0.0627, 0.0078, 0.0157],
             ...,
             [0.0000, 0.0157, 0.0000,  ..., 0.0000, 0.0431, 0.0941],
             [0.0039, 0.0471, 0.0118,  ..., 0.1020, 0.0784, 0.0118],
             [0.0510, 0.0549, 0.0000,  ..., 0.0000, 0.0902, 0.0510]],
    
            [[0.1922, 0.0980, 0.1294,  ..., 0.2157, 0.1216, 0.1451],
             [0.1961, 0.1137, 0.1294,  ..., 0.0431, 0.1137, 0.1569],
             [0.1333, 0.0902, 0.1176,  ..., 0.1098, 0.0510, 0.0471],
             ...,
             [0.0745, 0.1098, 0.0941,  ..., 0.0588, 0.1451, 0.1961],
             [0.0902, 0.1333, 0.1059,  ..., 0.1882, 0.1647, 0.0980],
             [0.1373, 0.1412, 0.0784,  ..., 0.0784, 0.1765, 0.1373]],
    
            [[0.3137, 0.2196, 0.2471,  ..., 0.3098, 0.2196, 0.2471],
             [0.3176, 0.2353, 0.2549,  ..., 0.1294, 0.2000, 0.2471],
             [0.2510, 0.2157, 0.2431,  ..., 0.1961, 0.1373, 0.1373],
             ...,
             [0.2157, 0.2510, 0.2353,  ..., 0.1255, 0.2118, 0.2627],
             [0.2353, 0.2784, 0.2471,  ..., 0.2706, 0.2471, 0.1804],
             [0.2824, 0.2863, 0.2196,  ..., 0.1608, 0.2588, 0.2196]]])



# Conclusion

In this post, I went over the basics of using PyTorch, including setting up tensors to perform basic operations on them, and used functions and modules in `torch.nn` to create simple network structures. Although this post tended to focus on the basic grammar of the PyTorch framework, I hope to make more fun and practical use of it in the near future. 
