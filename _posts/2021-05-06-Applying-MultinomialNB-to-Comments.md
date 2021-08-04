---
title: "Classifying Comments on Papers"
toc: true
categories:
  - study
tags: 
  - machine-learning
---



Multinomial naïve Bayes--quite a name for a machine-learning algorithm! Let's learn more about what it is.

## Introduction to multinomial naïve Bayes


Multinomial naïve Bayes is an example of a supervised-learning algorithm, and relies on the probability density function of multinomial distributions to find the probability that a certain feature vector would be observed, should the vector be a part of a certain class. When deciding upon which class a vector belongs to, the algorithm calculates the individual probabilities that the vector would belong to certain classes, and then chooses the class with the highest probability. 

Let's look more at why the algorithm is called "multinomial naïve Bayes."

First, the algorithm uses the PDF of multinomial distribution to calculate the probability that a vector would belong to a specific class. The next term--naïve--essentially means "simple," and refers to the idea of independent events in general. In the example down below, the probability that a certain feature would be 1 or 0 is independent from the probability that another feature would be 1 or 0. Finally, the third term, Bayes, refers to Bayes' theorem--which in probability theory, calculates the probability of an event occurring under the assumption that another event has already occurred. 

Since multinomial naïve Bayes is an example of a supervised-learning algorithm, the used data set can be expressed in terms of a set of labeled examples, such as those in the form {(**x**,y)}, where x is the feature vector, and y is the label. For example, if the training set consists of a set of labeled examples, in the form (sentence, class), such as (negative sentence, negative), the (sentence, class) form turns to an array of vectors with elements 0 and 1, referencing a key index possessing all the words in the set of labeled examples with the form (sentence, class). 

To use an example that I used in this post, take this sentence:  "Next time, make sure you use proper grammar." This sentence is turned to this array: 



```python
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0]
```



The words inside this sentence can be expressed into the array,  ['grammar', 'make', 'next', 'proper', 'sure', 'time', 'use', 'you'], where the 1s in the array shown above correspond to the words in this array. For the example in this post, the number of indices in the key array that possess all the words in the labeled examples is 85. In fact, the number of elements (either 0 or 1) in the array above are exactly 85. Using the multinomial distribution PDF, we can calculate the probability that a certain vector would be observed in an assumed class. 

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script> 

A multinomial distribution has the probability mass function formula of	

​										 
$$
p(x_1, x_2, ...,x_k, p_1, p_2, ...p_k) = \frac{n!}{(x_1! x_2! x_3! ...x_k!)} \cdot (p_1)^{x_1}(p_2)^{x_2}...(p_k)^{x_k}
$$


Essentially, the formula describes the probability of observing event 1, event 2, event 3, and so on, until event k, at a frequency of x1, x2, x3, [...] xk times. 

The graph below shows a visualization of a particular type of multinomial distribution, called trinomial distribution.[Image from MathWorks: https://de.mathworks.com/help/stats/mnpdf.html

<img src="/assets/images/1.png">




In summary, the algorithm takes a look at whether the 85-dimensional vector in the training set is part of the positive or negative class.  Then for each class, the probability that a vector in the testing set is in the class is calculated. If the probability that the vector belongs in say, class C1 is greater than the probability that the vector belongs in class C2, we can reasonably assume that the vector is part of class C1. Since I am a student in the process of learning machine learning, I won't explain these steps in a more detailed way, but if you are interested, you may wish to check out the exact workings of the multinomial naïve Bayes algorithm in the following link: ([sklearn.naive_bayes.MultinomialNB — scikit-learn 0.24.2 documentation](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html)) 


Next, I will briefly explain my choice of the comments that I used in the post. In the exercise of the post, I classified fictitious comments offered by an imaginary English teacher for the papers turned in by her students. Why comments on English papers? The following commentary might be long, but this is essentially why I decided to use them. In high school, my English teacher had a habit of writing only his honest, unfiltered thoughts about papers that I would turn in. Consequently, when I first encountered his comments on my paper, I was surprised and honestly a bit upset. Ultimately, I improved my English enough to satisfy him before graduation, and in hindsight, I thank him for cracking the whip, so to speak. In fact, I don't believe I have taken any comments on my assignments more seriously than those on my English papers. Given how memorable the comments were, I decided to classify comments on English papers--although the ones used in the example are not mine. 



## Application of algorithm  



In this demonstration, I used the `numpy`  and `pandas` libraries, so I first imported those libraries up top with `import numpy as np` and `import pandas as pd`. Next, I imported all the necessary classes, `CountVectorizer`, `MultinomialNB`, and `accuracy_score` from their respective Scikit-learn modules. 



```python
import numpy as np
import pandas as pd
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score
```



Then I created a list called `comment_list` that is made up of ten dictionaries, each with two elements called "comment" and "type." In this list, I added the comments that may have been offered by English teachers to students about their essays. (Personally, in high school, I have read comments written by my English teacher that were quite similar to those comments) I also assigned the category "type" to correspond to the tone of the comment--either negative or positive.  The picture down below shows the `dataframe` object containing the `comment_list`. 

```python
comment_list = [
{'comment': 'next time, make sure you use proper grammar','type':'negative'},
{'comment': 'great essay! I look forward to reading more','type':'positive'},
{'comment': 'missing punctuation makes it difficult for any reader to understand your essay','type':'negative'},
{'comment': 'Observations not discussed in class are present. Absolutely brilliant. This shows me your own unique thinking','type':'positive'},
{'comment': 'terrible grammar. Make sure you review the basics of sentence structuring','type':'negative'},
{'comment': 'no indentations. plus, why didnt you add the citations at the end?','type':'negative'},
{'comment': 'the sentence you wrote on line 85 qualifies as sloppy work. almost constitutes plagiarism','type':'negative'},
{'comment': 'excellent essay! Cant wait to read more.','type':'positive'},
{'comment': 'You impressed me a lot with your control of the language. Keep working!','type':'positive'},
{'comment': 'Nice work! Keen and apt observations encapsulated within concise, pithy sentences.','type':'positive'}    
]
df = pd.DataFrame(comment_list)
df
```



<img src="/assets/images/2.PNG">



Here, we use `sklearn.feature_extraction.text` module and the `sklearn.feature_extraction.text.CountVectorizer` class, which according to the Scikit-learn webpage, "converts a collection of text documents to a matrix of token counts." 

Basically, any object of that class contains a `.fit_transform()` method, which takes a `dataframe` object as the parameter.  

Because we wish to change the array of sentences into an array of 1's and 0's, we use the following code `df['label'] = df['type'].map({"positive": 1, "negative":0})` to change the name of the category column from "type" to "label," and to change "positive" to one and "negative" to zero. Next, we assign each of the categories "comment" and "label" into separate `pandas.core.series.Series` objects called `df_x` and `df_y`. As we wish to visualize the array version of `df_x` with its elements of 1's and 0's, we use `x_traincv.toarray()` to save the array in the object `encoded_input`. 



```python
df['label'] = df['type'].map({"positive": 1, "negative":0})

df_x = df["comment"]
df_y = df["label"]

cv = CountVectorizer() # Create new CountVectorizer object called cv

x_traincv = cv.fit_transform(df_x)
encoded_input = x_traincv.toarray()

encoded_input 
```





After creating a new `MultinomialNB()` object called `mnb`, we use the `.fit()`method of the `MultinomialNB` class to provide the two parameters: `x_traincv` and `y_traincv`, which are `scipy.sparse.csr.csr_matrix` objects. 





```python
mnb= MultinomialNB() # create new MultinomialNB object

y_train = df_y.astype('int') 
mnb.fit(x_traincv, y_train)

```

```python
test_comment_list = [ 

{'comment': 'a fine piece of work. nicely done','type':'positive'},
{'comment': 'your sentences flow smoothly much better than I expected keep going!','type':'positive'},
{'comment': 'your essay always impresses me you have a genius for it', 'type':'positive'},
{'comment': 'cool essay about a cool topic well done!','type':'positive'},
{'comment': 'a magnificent piece of writing','type':'positive'},
{'comment': 'make sure you use correct grammar','type':'negative'},
{'comment': 'off topic and possessing factually incorrect statements make sure you double check your essay','type':'negative'},
{'comment': 'this is a bad piece of writing I am simply disappointed','type':'negative'},
{'comment': 'this essay contains incorrect use of punctuation and is not well written','type':'negative'},
{'comment': 'not a very good piece of writing','type':'negative'}   
]

test_df = pd.DataFrame(test_comment_list)
test_df['label'] = test_df['type'].map({"positive":1, "negative":0})

test_x = test_df["comment"]
test_y = test_df["label"]
```



After providing the testing set, we wish to see the accuracy score of the algorithm, so we run the following code and get the result 0.6. 



```python
x_testcv = cv.transform(test_x)

predictions = mnb.predict(x_testcv)
accuracy_score(test_y, predictions)

```



If we wish to visualize the result, we can run: 

```python
comparison = pd.DataFrame({'predictions': predictions, 'ground_truth':test_y.values.ravel()})
comparison
```

```python
0.6
```

<img src="/assets/images/3.PNG">



Even though the accuracy score of 0.6 indicates that the algorithm's predictions are only a bit better than mere guesses with a probability of 0.5, we expect that if we increase the size of the training set to include the words that were present in the testing set, we would observe a much higher accuracy score. For example, we notice that there were words that were in the testing set, such as "disappointed", "cool", and "impressive," while there were also words present in the training set that were not in the testing set, such as "plagiarism" and "brilliant."

This is the end of the post! See you next time. 