---
title: "Classifying Movie Reviews: Keras"
date: 2021-08-27
mathjax: true
toc: true
categories:
  - study
tags:
  - Keras
  - Tensorflow
---



In this blog post, I will continue on from the last post in experimenting with Keras, mainly referencing François Chollet's text, *Machine Learning with Python*. The technical approach to designing the neural network is similar to the previous post's, so more detailed explanations of each individual steps can be found there. So without any further due, let's start by classifying the IMDB dataset--which contains approximately 50,000 polarized movie reviews--into positive and negative review categories.

## Loading the IMDB dataset


```python
from keras.datasets import imdb

(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)
```

    Downloading data from https://storage.googleapis.com/tensorflow/tf-keras-datasets/imdb.npz
    17465344/17464789 [==============================] - 0s 0us/step
    17473536/17464789 [==============================] - 0s 0us/step



```python
train_data  # 25,000 movie reviews in train_data, each in vector form of integers
```




    array([list([1, 14, 22, 16, 43, 530, 973, 1622, 1385, 65, 458, 4468, 66, 3941, 4, 173, 36, 256, 5, 25, 100, 43, 838, 112, 50, 670, 2, 9, 35, 480, 284, 5, 150, 4, 172, 112, 167, 2, 336, 385, 39, 4, 172, 4536, 1111, 17, 546, 38, 13, 447, 4, 192, 50, 16, 6, 147, 2025, 19, 14, 22, 4, 1920, 4613, 469, 4, 22, 71, 87, 12, 16, 43, 530, 38, 76, 15, 13, 1247, 4, 22, 17, 515, 17, 12, 16, 626, 18, 2, 5, 62, 386, 12, 8, 316, 8, 106, 5, 4, 2223, 5244, 16, 480, 66, 3785, 33, 4, 130, 12, 16, 38, 619, 5, 25, 124, 51, 36, 135, 48, 25, 1415, 33, 6, 22, 12, 215, 28, 77, 52, 5, 14, 407, 16, 82, 2, 8, 4, 107, 117, 5952, 15, 256, 4, 2, 7, 3766, 5, 723, 36, 71, 43, 530, 476, 26, 400, 317, 46, 7, 4, 2, 1029, 13, 104, 88, 4, 381, 15, 297, 98, 32, 2071, 56, 26, 141, 6, 194, 7486, 18, 4, 226, 22, 21, 134, 476, 26, 480, 5, 144, 30, 5535, 18, 51, 36, 28, 224, 92, 25, 104, 4, 226, 65, 16, 38, 1334, 88, 12, 16, 283, 5, 16, 4472, 113, 103, 32, 15, 16, 5345, 19, 178, 32]),
           list([1, 194, 1153, 194, 8255, 78, 228, 5, 6, 1463, 4369, 5012, 134, 26, 4, 715, 8, 118, 1634, 14, 394, 20, 13, 119, 954, 189, 102, 5, 207, 110, 3103, 21, 14, 69, 188, 8, 30, 23, 7, 4, 249, 126, 93, 4, 114, 9, 2300, 1523, 5, 647, 4, 116, 9, 35, 8163, 4, 229, 9, 340, 1322, 4, 118, 9, 4, 130, 4901, 19, 4, 1002, 5, 89, 29, 952, 46, 37, 4, 455, 9, 45, 43, 38, 1543, 1905, 398, 4, 1649, 26, 6853, 5, 163, 11, 3215, 2, 4, 1153, 9, 194, 775, 7, 8255, 2, 349, 2637, 148, 605, 2, 8003, 15, 123, 125, 68, 2, 6853, 15, 349, 165, 4362, 98, 5, 4, 228, 9, 43, 2, 1157, 15, 299, 120, 5, 120, 174, 11, 220, 175, 136, 50, 9, 4373, 228, 8255, 5, 2, 656, 245, 2350, 5, 4, 9837, 131, 152, 491, 18, 2, 32, 7464, 1212, 14, 9, 6, 371, 78, 22, 625, 64, 1382, 9, 8, 168, 145, 23, 4, 1690, 15, 16, 4, 1355, 5, 28, 6, 52, 154, 462, 33, 89, 78, 285, 16, 145, 95]),
           list([1, 14, 47, 8, 30, 31, 7, 4, 249, 108, 7, 4, 5974, 54, 61, 369, 13, 71, 149, 14, 22, 112, 4, 2401, 311, 12, 16, 3711, 33, 75, 43, 1829, 296, 4, 86, 320, 35, 534, 19, 263, 4821, 1301, 4, 1873, 33, 89, 78, 12, 66, 16, 4, 360, 7, 4, 58, 316, 334, 11, 4, 1716, 43, 645, 662, 8, 257, 85, 1200, 42, 1228, 2578, 83, 68, 3912, 15, 36, 165, 1539, 278, 36, 69, 2, 780, 8, 106, 14, 6905, 1338, 18, 6, 22, 12, 215, 28, 610, 40, 6, 87, 326, 23, 2300, 21, 23, 22, 12, 272, 40, 57, 31, 11, 4, 22, 47, 6, 2307, 51, 9, 170, 23, 595, 116, 595, 1352, 13, 191, 79, 638, 89, 2, 14, 9, 8, 106, 607, 624, 35, 534, 6, 227, 7, 129, 113]),
           ...,
           list([1, 11, 6, 230, 245, 6401, 9, 6, 1225, 446, 2, 45, 2174, 84, 8322, 4007, 21, 4, 912, 84, 2, 325, 725, 134, 2, 1715, 84, 5, 36, 28, 57, 1099, 21, 8, 140, 8, 703, 5, 2, 84, 56, 18, 1644, 14, 9, 31, 7, 4, 9406, 1209, 2295, 2, 1008, 18, 6, 20, 207, 110, 563, 12, 8, 2901, 2, 8, 97, 6, 20, 53, 4767, 74, 4, 460, 364, 1273, 29, 270, 11, 960, 108, 45, 40, 29, 2961, 395, 11, 6, 4065, 500, 7, 2, 89, 364, 70, 29, 140, 4, 64, 4780, 11, 4, 2678, 26, 178, 4, 529, 443, 2, 5, 27, 710, 117, 2, 8123, 165, 47, 84, 37, 131, 818, 14, 595, 10, 10, 61, 1242, 1209, 10, 10, 288, 2260, 1702, 34, 2901, 2, 4, 65, 496, 4, 231, 7, 790, 5, 6, 320, 234, 2766, 234, 1119, 1574, 7, 496, 4, 139, 929, 2901, 2, 7750, 5, 4241, 18, 4, 8497, 2, 250, 11, 1818, 7561, 4, 4217, 5408, 747, 1115, 372, 1890, 1006, 541, 9303, 7, 4, 59, 2, 4, 3586, 2]),
           list([1, 1446, 7079, 69, 72, 3305, 13, 610, 930, 8, 12, 582, 23, 5, 16, 484, 685, 54, 349, 11, 4120, 2959, 45, 58, 1466, 13, 197, 12, 16, 43, 23, 2, 5, 62, 30, 145, 402, 11, 4131, 51, 575, 32, 61, 369, 71, 66, 770, 12, 1054, 75, 100, 2198, 8, 4, 105, 37, 69, 147, 712, 75, 3543, 44, 257, 390, 5, 69, 263, 514, 105, 50, 286, 1814, 23, 4, 123, 13, 161, 40, 5, 421, 4, 116, 16, 897, 13, 2, 40, 319, 5872, 112, 6700, 11, 4803, 121, 25, 70, 3468, 4, 719, 3798, 13, 18, 31, 62, 40, 8, 7200, 4, 2, 7, 14, 123, 5, 942, 25, 8, 721, 12, 145, 5, 202, 12, 160, 580, 202, 12, 6, 52, 58, 2, 92, 401, 728, 12, 39, 14, 251, 8, 15, 251, 5, 2, 12, 38, 84, 80, 124, 12, 9, 23]),
           list([1, 17, 6, 194, 337, 7, 4, 204, 22, 45, 254, 8, 106, 14, 123, 4, 2, 270, 2, 5, 2, 2, 732, 2098, 101, 405, 39, 14, 1034, 4, 1310, 9, 115, 50, 305, 12, 47, 4, 168, 5, 235, 7, 38, 111, 699, 102, 7, 4, 4039, 9245, 9, 24, 6, 78, 1099, 17, 2345, 2, 21, 27, 9685, 6139, 5, 2, 1603, 92, 1183, 4, 1310, 7, 4, 204, 42, 97, 90, 35, 221, 109, 29, 127, 27, 118, 8, 97, 12, 157, 21, 6789, 2, 9, 6, 66, 78, 1099, 4, 631, 1191, 5, 2642, 272, 191, 1070, 6, 7585, 8, 2197, 2, 2, 544, 5, 383, 1271, 848, 1468, 2, 497, 2, 8, 1597, 8778, 2, 21, 60, 27, 239, 9, 43, 8368, 209, 405, 10, 10, 12, 764, 40, 4, 248, 20, 12, 16, 5, 174, 1791, 72, 7, 51, 6, 1739, 22, 4, 204, 131, 9])],
          dtype=object)




```python
max_word_index = []
for sequence in train_data:
  max_word_index.append(max(sequence))
print(max(max_word_index)) # max word index in all of the train data, 9999 since num_words = 10,000
```

    9999


To see the actual movie review in sentence form, use the `get_word_index()` function to load the words and their corresponding index numbers. Then reverse the word-number pairs to get `reverse_word_index`, a `dict_type` object that returns a word when provided the index. 


```python
word_index = imdb.get_word_index() # returns a dict_key type item

reverse_word_index = dict([(value, key) for (key, value) in word_index.items()]) # reverses the order 

print(list(word_index.items())[0:5])
print(list(reverse_word_index.items())[0:5])

# len(word_index) returns 88584, 88584 items in (key, value)
```

    [('fawn', 34701), ('tsukino', 52006), ('nunnery', 52007), ('sonja', 16816), ('vani', 63951)]
    [(34701, 'fawn'), (52006, 'tsukino'), (52007, 'nunnery'), (16816, 'sonja'), (63951, 'vani')]



```python
train_data[8][0:5] # 8th review
```




    [1, 43, 188, 46, 5]



The code block below matches the integers in `train_data[8]` to their respective words as specified by `reverse_word_index`, and joins the words to ' '. 


```python
decoded_review = ' '.join([reverse_word_index.get(i-3, '?') for i in train_data[8]])

decoded_review # positive review, 9th review among 25,000 reviews
```




    "? just got out and cannot believe what a brilliant documentary this is rarely do you walk out of a movie theater in such awe and ? lately movies have become so over hyped that the thrill of discovering something truly special and unique rarely happens ? ? did this to me when it first came out and this movie is doing to me now i didn't know a thing about this before going into it and what a surprise if you hear the concept you might get the feeling that this is one of those ? movies about an amazing triumph covered with over the top music and trying to have us fully convinced of what a great story it is telling but then not letting us in ? this is not that movie the people tell the story this does such a good job of capturing every moment of their involvement while we enter their world and feel every second with them there is so much beyond the climb that makes everything they go through so much more tense touching the void was also a great doc about mountain climbing and showing the intensity in an engaging way but this film is much more of a human story i just saw it today but i will go and say that this is one of the best documentaries i have ever seen"

As visible from the above code block, the eighth review is clearly positive, although lacking in grammar. 



### One-hot encoding

`train_data` one-hot encoded to numpy array of 25,000 vectors, with each vector consisting of 10,000 elements. 


```python
train_data
```




    array([list([1, 14, 22, 16, 43, 530, 973, 1622, 1385, 65, 458, 4468, 66, 3941, 4, 173, 36, 256, 5, 25, 100, 43, 838, 112, 50, 670, 2, 9, 35, 480, 284, 5, 150, 4, 172, 112, 167, 2, 336, 385, 39, 4, 172, 4536, 1111, 17, 546, 38, 13, 447, 4, 192, 50, 16, 6, 147, 2025, 19, 14, 22, 4, 1920, 4613, 469, 4, 22, 71, 87, 12, 16, 43, 530, 38, 76, 15, 13, 1247, 4, 22, 17, 515, 17, 12, 16, 626, 18, 2, 5, 62, 386, 12, 8, 316, 8, 106, 5, 4, 2223, 5244, 16, 480, 66, 3785, 33, 4, 130, 12, 16, 38, 619, 5, 25, 124, 51, 36, 135, 48, 25, 1415, 33, 6, 22, 12, 215, 28, 77, 52, 5, 14, 407, 16, 82, 2, 8, 4, 107, 117, 5952, 15, 256, 4, 2, 7, 3766, 5, 723, 36, 71, 43, 530, 476, 26, 400, 317, 46, 7, 4, 2, 1029, 13, 104, 88, 4, 381, 15, 297, 98, 32, 2071, 56, 26, 141, 6, 194, 7486, 18, 4, 226, 22, 21, 134, 476, 26, 480, 5, 144, 30, 5535, 18, 51, 36, 28, 224, 92, 25, 104, 4, 226, 65, 16, 38, 1334, 88, 12, 16, 283, 5, 16, 4472, 113, 103, 32, 15, 16, 5345, 19, 178, 32]),
           list([1, 194, 1153, 194, 8255, 78, 228, 5, 6, 1463, 4369, 5012, 134, 26, 4, 715, 8, 118, 1634, 14, 394, 20, 13, 119, 954, 189, 102, 5, 207, 110, 3103, 21, 14, 69, 188, 8, 30, 23, 7, 4, 249, 126, 93, 4, 114, 9, 2300, 1523, 5, 647, 4, 116, 9, 35, 8163, 4, 229, 9, 340, 1322, 4, 118, 9, 4, 130, 4901, 19, 4, 1002, 5, 89, 29, 952, 46, 37, 4, 455, 9, 45, 43, 38, 1543, 1905, 398, 4, 1649, 26, 6853, 5, 163, 11, 3215, 2, 4, 1153, 9, 194, 775, 7, 8255, 2, 349, 2637, 148, 605, 2, 8003, 15, 123, 125, 68, 2, 6853, 15, 349, 165, 4362, 98, 5, 4, 228, 9, 43, 2, 1157, 15, 299, 120, 5, 120, 174, 11, 220, 175, 136, 50, 9, 4373, 228, 8255, 5, 2, 656, 245, 2350, 5, 4, 9837, 131, 152, 491, 18, 2, 32, 7464, 1212, 14, 9, 6, 371, 78, 22, 625, 64, 1382, 9, 8, 168, 145, 23, 4, 1690, 15, 16, 4, 1355, 5, 28, 6, 52, 154, 462, 33, 89, 78, 285, 16, 145, 95]),
           list([1, 14, 47, 8, 30, 31, 7, 4, 249, 108, 7, 4, 5974, 54, 61, 369, 13, 71, 149, 14, 22, 112, 4, 2401, 311, 12, 16, 3711, 33, 75, 43, 1829, 296, 4, 86, 320, 35, 534, 19, 263, 4821, 1301, 4, 1873, 33, 89, 78, 12, 66, 16, 4, 360, 7, 4, 58, 316, 334, 11, 4, 1716, 43, 645, 662, 8, 257, 85, 1200, 42, 1228, 2578, 83, 68, 3912, 15, 36, 165, 1539, 278, 36, 69, 2, 780, 8, 106, 14, 6905, 1338, 18, 6, 22, 12, 215, 28, 610, 40, 6, 87, 326, 23, 2300, 21, 23, 22, 12, 272, 40, 57, 31, 11, 4, 22, 47, 6, 2307, 51, 9, 170, 23, 595, 116, 595, 1352, 13, 191, 79, 638, 89, 2, 14, 9, 8, 106, 607, 624, 35, 534, 6, 227, 7, 129, 113]),
           ...,
           list([1, 11, 6, 230, 245, 6401, 9, 6, 1225, 446, 2, 45, 2174, 84, 8322, 4007, 21, 4, 912, 84, 2, 325, 725, 134, 2, 1715, 84, 5, 36, 28, 57, 1099, 21, 8, 140, 8, 703, 5, 2, 84, 56, 18, 1644, 14, 9, 31, 7, 4, 9406, 1209, 2295, 2, 1008, 18, 6, 20, 207, 110, 563, 12, 8, 2901, 2, 8, 97, 6, 20, 53, 4767, 74, 4, 460, 364, 1273, 29, 270, 11, 960, 108, 45, 40, 29, 2961, 395, 11, 6, 4065, 500, 7, 2, 89, 364, 70, 29, 140, 4, 64, 4780, 11, 4, 2678, 26, 178, 4, 529, 443, 2, 5, 27, 710, 117, 2, 8123, 165, 47, 84, 37, 131, 818, 14, 595, 10, 10, 61, 1242, 1209, 10, 10, 288, 2260, 1702, 34, 2901, 2, 4, 65, 496, 4, 231, 7, 790, 5, 6, 320, 234, 2766, 234, 1119, 1574, 7, 496, 4, 139, 929, 2901, 2, 7750, 5, 4241, 18, 4, 8497, 2, 250, 11, 1818, 7561, 4, 4217, 5408, 747, 1115, 372, 1890, 1006, 541, 9303, 7, 4, 59, 2, 4, 3586, 2]),
           list([1, 1446, 7079, 69, 72, 3305, 13, 610, 930, 8, 12, 582, 23, 5, 16, 484, 685, 54, 349, 11, 4120, 2959, 45, 58, 1466, 13, 197, 12, 16, 43, 23, 2, 5, 62, 30, 145, 402, 11, 4131, 51, 575, 32, 61, 369, 71, 66, 770, 12, 1054, 75, 100, 2198, 8, 4, 105, 37, 69, 147, 712, 75, 3543, 44, 257, 390, 5, 69, 263, 514, 105, 50, 286, 1814, 23, 4, 123, 13, 161, 40, 5, 421, 4, 116, 16, 897, 13, 2, 40, 319, 5872, 112, 6700, 11, 4803, 121, 25, 70, 3468, 4, 719, 3798, 13, 18, 31, 62, 40, 8, 7200, 4, 2, 7, 14, 123, 5, 942, 25, 8, 721, 12, 145, 5, 202, 12, 160, 580, 202, 12, 6, 52, 58, 2, 92, 401, 728, 12, 39, 14, 251, 8, 15, 251, 5, 2, 12, 38, 84, 80, 124, 12, 9, 23]),
           list([1, 17, 6, 194, 337, 7, 4, 204, 22, 45, 254, 8, 106, 14, 123, 4, 2, 270, 2, 5, 2, 2, 732, 2098, 101, 405, 39, 14, 1034, 4, 1310, 9, 115, 50, 305, 12, 47, 4, 168, 5, 235, 7, 38, 111, 699, 102, 7, 4, 4039, 9245, 9, 24, 6, 78, 1099, 17, 2345, 2, 21, 27, 9685, 6139, 5, 2, 1603, 92, 1183, 4, 1310, 7, 4, 204, 42, 97, 90, 35, 221, 109, 29, 127, 27, 118, 8, 97, 12, 157, 21, 6789, 2, 9, 6, 66, 78, 1099, 4, 631, 1191, 5, 2642, 272, 191, 1070, 6, 7585, 8, 2197, 2, 2, 544, 5, 383, 1271, 848, 1468, 2, 497, 2, 8, 1597, 8778, 2, 21, 60, 27, 239, 9, 43, 8368, 209, 405, 10, 10, 12, 764, 40, 4, 248, 20, 12, 16, 5, 174, 1791, 72, 7, 51, 6, 1739, 22, 4, 204, 131, 9])],
          dtype=object)




```python
import numpy as np
def vectorize_sequences(sequences, dimension=10000):
  results = np.zeros((len(sequences), dimension))  # np.zeros(25000, 10000) creates array with 25000 vectors, each with 10000 zeros. 
  for i, sequence in enumerate(sequences): # first review: i = 0, sequence = [1,14,22,16,43 ... ]
    results[i, sequence] = 1. # first vector of np array is one hot encoded. (1st, 14th, 22nd, 16th ... indices = 1), with zeroes in place of (1st, 14th, 22nd, 16th ...) turned to 1. 
  return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)

print(x_train, "\n")
print(len(x_train), "\n") 

print(x_train[0], "\n")
print(len(x_train[0]), "\n")

```

    [[0. 1. 1. ... 0. 0. 0.]
     [0. 1. 1. ... 0. 0. 0.]
     [0. 1. 1. ... 0. 0. 0.]
     ...
     [0. 1. 1. ... 0. 0. 0.]
     [0. 1. 1. ... 0. 0. 0.]
     [0. 1. 1. ... 0. 0. 0.]] 
    
    25000 
    
    [0. 1. 1. ... 0. 0. 0.] 
    
    10000 


​    


```python
y_train = np.asarray(train_labels).astype('float32')
y_test = np.asarray(test_labels).astype('float32')

y_train # array([1., 0., 0., ..., 0., 1., 0.], dtype=float32)
y_test # array([0., 1., 1., ..., 0., 0., 0.], dtype=float32)
```




    array([0., 1., 1., ..., 0., 0., 0.], dtype=float32)



## Layers

In network architecture, there are a few options that may be as complex to a beginner as choosing the right number of layers and the number of units, or nodes, within those layers. In the following code, two `Dense` layers with sixteen output nodes each are used, with `relu` as the activation function. The input shape is `(10000,)` since there are 25000 vectors with 10000 elements each, and one element is assigned to one input node.  


```python
from keras import models
from keras import layers
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
```

The rectified linear unit function is shown below, which maps any input less than zero to zero, while mapping any positive values to the values themselves. The `relu` activation function is most widely used for intermediate layers with hidden nodes. 


```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

x = np.linspace(-10, 10, 1000)
y = np.maximum(0, x)

plt.plot(x, y)
plt.legend(['Relu'])
plt.title('Rectified Linear Unit Function')
plt.show()
```


<figure class="align-center">
  <img src="/assets/images/imdb (1).png" alt="">
  <figcaption>Rectified Linear Unit Function (ReLU)</figcaption>
</figure> 


For the last layer, since the task at hand is a binary classification (positive/negative) problem, the activation function of choice is the famous sigmoid function: $y = 1/ (1 + e^{-x}) $. The sigmoid function is useful when we wish the output scalar value to be a probability between zero and one, and is therefore one of the most widely used activation functions for the last layer.  


```python
x = np.linspace(-10, 10, 1000)
y = 1 / (1 + np.exp(-x))

plt.plot(x, y)
plt.legend(['Sigmoid'])
plt.title('Sigmoid Function')
plt.show()
```


<figure class="align-center">
  <img src="/assets/images/imdb (2).png" alt="">
  <figcaption>Sigmoid Function</figcaption>
</figure>

## Overfitting Issue

To deal with the issue of overfitting the data, one could create validation sets by splitting the training set into two parts. As will be seen later, by using validation sets, one can graph the validation loss to optimize the number of epochs that the model is trained on. In this example, `x_val`--the first 10000 samples of `x_train`--is set apart from the rest of the 15000 samples used for training the model. Later, `x_val` will be used to calculate the validation loss (`val_loss`) and the validation accuracy (`val_acc`), both of which will be graphed to see at which epoch the validation loss increases. This can provide information on the optimum number of epochs to train the model on.  


```python
x_val = x_train[:10000]
partial_x_train = x_train[10000:]
y_val = y_train[:10000]
partial_y_train = y_train[10000:]
```

## Loss function & Optimizer

### Binary Cross Entropy

In binary classification problems, the most often used loss function is the binary cross entropy function. 

Binary cross entropy / Log loss: 

$$ CE = - \frac{1}{N} \sum_{i=1}^{N} [y_i \cdot log_2(p(y_i)) + (1-y_i) \cdot log_2(1-p(y_i))]  $$

where $y_i$ and $p(y_i)$ are as provided below.  

\\
* $ y_i = 
\begin{cases}
1 \;\;\;\;\;\; (C_1) \\
0 \;\;\;\;\;\; (C_2)
\end{cases} $

* $ p(y_i) = p(i^{th} \text{ sample $\in C_1$ })$
* $ 1-p(y_i) = p(i^{th} \text{ sample $\in C_2$})$ 

\\

This blog post by Daniel Godoy does a good job introducing the concept of binary cross entropy. In his example, the binary classification task attempts to categorize points into two groups: red and green. After doing the cross entropy calculation, it becomes clear that the model that incorrectly assigns high probabilities to data points have higher cross entropy loss scores. On the other hand, models that correctly assign high probabilities to points have lower loss scores. 


```python
model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['acc'])

history = model.fit(partial_x_train,
                    partial_y_train,
                    epochs=20,
                    batch_size=512,
                    validation_data=(x_val, y_val))
```

    Epoch 1/20
    30/30 [==============================] - 4s 90ms/step - loss: 0.5063 - acc: 0.7917 - val_loss: 0.3827 - val_acc: 0.8750
    Epoch 2/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.3081 - acc: 0.9017 - val_loss: 0.3165 - val_acc: 0.8801
    Epoch 3/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.2250 - acc: 0.9285 - val_loss: 0.2775 - val_acc: 0.8917
    Epoch 4/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.1752 - acc: 0.9441 - val_loss: 0.2955 - val_acc: 0.8806
    Epoch 5/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.1453 - acc: 0.9531 - val_loss: 0.2854 - val_acc: 0.8878
    Epoch 6/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.1208 - acc: 0.9612 - val_loss: 0.2896 - val_acc: 0.8877
    Epoch 7/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0985 - acc: 0.9709 - val_loss: 0.3115 - val_acc: 0.8848
    Epoch 8/20
    30/30 [==============================] - 1s 42ms/step - loss: 0.0842 - acc: 0.9755 - val_loss: 0.3439 - val_acc: 0.8790
    Epoch 9/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0703 - acc: 0.9806 - val_loss: 0.3590 - val_acc: 0.8806
    Epoch 10/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0560 - acc: 0.9863 - val_loss: 0.3975 - val_acc: 0.8701
    Epoch 11/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0463 - acc: 0.9891 - val_loss: 0.3982 - val_acc: 0.8752
    Epoch 12/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0395 - acc: 0.9911 - val_loss: 0.4368 - val_acc: 0.8756
    Epoch 13/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0337 - acc: 0.9929 - val_loss: 0.4562 - val_acc: 0.8724
    Epoch 14/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0269 - acc: 0.9952 - val_loss: 0.4898 - val_acc: 0.8692
    Epoch 15/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0213 - acc: 0.9959 - val_loss: 0.5199 - val_acc: 0.8677
    Epoch 16/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0167 - acc: 0.9980 - val_loss: 0.5479 - val_acc: 0.8703
    Epoch 17/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0135 - acc: 0.9983 - val_loss: 0.6182 - val_acc: 0.8603
    Epoch 18/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0110 - acc: 0.9985 - val_loss: 0.6145 - val_acc: 0.8677
    Epoch 19/20
    30/30 [==============================] - 1s 41ms/step - loss: 0.0105 - acc: 0.9977 - val_loss: 0.6532 - val_acc: 0.8645
    Epoch 20/20
    30/30 [==============================] - 1s 40ms/step - loss: 0.0050 - acc: 0.9998 - val_loss: 0.6831 - val_acc: 0.8644



```python
history_dict = history.history
history_dict.keys() # [u'acc', u'loss', u'val_acc', u'val_loss']
```




    dict_keys(['loss', 'acc', 'val_loss', 'val_acc'])




```python
import matplotlib.pyplot as plt

history_dict = history.history
loss_values = history_dict['loss']
val_loss_values = history_dict['val_loss']
epochs = range(1, 21)

plt.plot(epochs, loss_values, 'bo', label='Training loss')
plt.plot(epochs, val_loss_values, 'b', label='Validation loss')
plt.title('Training and validation loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.show()
```


<figure class="align-center">
  <img src="/assets/images/imdb (3).png" alt="">
  <figcaption>Training and Validation Loss</figcaption>
</figure> 


```python
plt.clf()

acc_values = history_dict['acc']
val_acc_values = history_dict['val_acc']
plt.plot(epochs, acc_values, 'bo', label='Training acc')
plt.plot(epochs, val_acc_values, 'b', label='Validation acc')
plt.title('Training and validation accuracy')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.show()
```


<figure class="align-center">
  <img src="/assets/images/imdb (4).png" alt="">
  <figcaption>Training and Validation Accuracy</figcaption>
</figure> 

```python
results = model.evaluate(x_test, y_test)
results
```

    782/782 [==============================] - 2s 2ms/step - loss: 0.3018 - accuracy: 0.8807

    [0.30182182788848877, 0.8807200193405151]



As can be seen above, the validation loss dips toward two or three epochs, then skyrockets after that point. This tells us that the seventeen epochs that came after the third epoch contributed to the model *overfitting* on the training data, which made the model less reliable for unseen data (such as the 10000 samples set aside before training).

That being said, after twenty epochs, the model achieved a somewhat high accuracy of 88.07% at evaluating the sentiment of movie reviews--a classic binary classification problem. 

## Conclusion

In this post, I went over a well-known, basic example of a binary sentiment analysis of movie reviews using neural networks. In the next post, I will continue to go down the rabbit hole towards the world of neural networks, working on the Reuters and the Boston housing price datasets. Thanks for reading. 

