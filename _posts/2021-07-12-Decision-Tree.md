---
title: "Decision Tree"
date: 2021-07-12
mathjax: true
toc: true
categories:
  - study
tags:
  - machine-learning
---



In this post, I will explain another popular supervised machine learning tool called a decision tree classifier. 

# Definition & Types

A decision tree classifier is a useful algorithm for classifying data. There are mainly two types of decision trees: classification trees and regression trees. The goal of a classification tree is to separate data into discrete classes. (For example, whether a select group of people have heart disease or not.) In a classification tree, a categorical question is used to separate data, and based on a true/false or yes/no answer, the data are classified into two groups. 

Meanwhile, regression trees attempt to predict specific numerical values for provided data. (For example, the efficacy of a specific type of medicine, depending on how much of the medicine was administered (the dosage).) In other words, a regression tree possesses certain numerical threshold values that it uses to separate data into lower nodes, whereas a classification tree uses a series of true/false questions to split data. Regression trees can be particularly effective when a standard linear regression model fails to produce accurate numerical predictions, and especially when the data are grouped in a non-uniform manner depending on a specific variable. 

Normally, when creating a decision tree, the topmost node is called the "root node," and the nodes underneath are either called "internal nodes" or "leaf nodes," If the data within those nodes can be separated into more nodes, then those nodes are called "internal nodes," whereas if they cannot be separated, they are called "leaf nodes." 

In this post, I will focus on classification trees, which I found to be more intuitive and easier to understand, compared to regression trees. 


***

## Classification Trees

As stated above, a classification tree works by classifying data from a higher-tier node to two lower-tier nodes, based on a yes-or-no answer to a categorical question. But as with any classification algorithm, one can optimize the efficiency of the classification tree by asking the right questions first. Here, the "right" questions are the ones that can most efficiently separate the data into the desired categories. 

To give a more concrete example, let's assume that the task at hand is to separate 100 people into two categories: either with or without heart disease. The classification tree will first divide those 100 people into two categories, based on their yes-or-no answers to a root-node question. Although highly unlikely, if the root-node question manages to separate those 100 people perfectly based on whether they have heart disease or not, one could stop there--no need for more classification, after all.

Unfortunately, this ideal scenario doesn't always conform to reality. In fact, it is natural to assume that no single question could perfectly filter out the desired group of people with/without heart disease. For example, suppose the root-node question is: "Do you drink more than five liters of alcohol per week?" Among those who answered positively, there may be a mix of people with and without heart disease. The same may hold true for those who answered negatively. In this case, the two internal nodes that result from the root-node question are called "impure," which means that the nodes did not clearly discriminate based on the presence of heart disease. 


***

## Gini Impurity & Entropy

One can statistically measure the degree of impurity in two ways: Gini impurity index and entropy.  The formula for the Gini impurity index is as follows:

$$ I_G = 1-\sum_{i=1}^{n} p^2 $$ 

When data at a higher-tier node are separated into two lower-tier nodes, the $I_G$ value is calculated for each of the two lower-tier nodes. For this specific case, the square of the probability that a person may have heart disease, added to the square of the probability that a person may *not* have it, is the sum. The  $I_G$ value is simply one minus that sum. Once the two $I_G$ values are calculated for both nodes, one has to find the total Gini impurity value, which is the weighted average of the two values. 

The Gini impurity value measures the degree of impurity, and since we want the nodes to be as "pure" as possible, the most efficient tree can be created by asking the question with the lowest associated Gini impurity value. Once we reach the point where the total Gini impurity value no longer decreases, we finish the classification tree and make the lowest-tier nodes into leaf nodes. 

There is another way to calculate impurity, which is by using the entropy equation. In chemistry, entropy is generally described as a measure of the state of disorderliness. In information theory, entropy is a measure of uncertainty. When we classify data into categories, uncertainty shrinks as we gain more information. In the case of heart disease, if we can perfectly separate people based on whether they have heart disease or not, the information gain would be one. Consequently, the resulting entropy would be zero. Therefore, the most efficient classification trees first use the questions that can most quickly reduce the entropy to zero. 

The equation for entropy in information theory is as follows: 

$$ I_H = -\sum_{j=1}^{c} p_j \cdot log_2(p_j) $$ 

Here, similar to how we calculated the Gini impurity value, the $p$ value either describes the probability of "yes" or "no" to the classification problem at hand. The only difference lies in the slightly different computation method, which for the entropy equation, involves calculating $p_j \cdot log_2(p_j)$ first. 



***

# Scikit-learn Implementation 

The following code blocks are from one of StatQuests' tutorials on [decision trees](https://www.youtube.com/watch?v=q90UDEgYqeI&t=949s). The used dataset is from the University of California Irvine's [Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/heart+disease), and was originally provided by the Cleveland Clinic Foundation in 1988.  

***

## Importing Packages & Data


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.tree import DecisionTreeClassifier # imported to build a classification tree
from sklearn.tree import plot_tree
from sklearn.model_selection import train_test_split
from sklearn.model_selection import cross_val_score # imported to perform cross validation
from sklearn.metrics import confusion_matrix # imported to *create* a confusion matrix
from sklearn.metrics import plot_confusion_matrix # imported to *draw* a confusion matrix
```


```python
df = pd.read_csv('processed.cleveland.csv', header=None)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
      <th>11</th>
      <th>12</th>
      <th>13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>2</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>2</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>2</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>2</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>2</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.columns = ['age', 'sex', 'cp', 'restbp', 'chol', 'fbs', 'restecg', 'thalach', 'exang', 'oldpeak', 'slope', 'ca', 'thal', 'hd']
# df.columns = ['parameter1', 'parameter2' [...] ] sets the column names to matching names.
df.head() # .head() prints the first 5 rows, including the column name
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>hd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>2</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>2</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>2</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>2</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>2</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



***

## Cleaning Data: Finding/Replacing Missing Values


```python
df.dtypes # Identify the data type for the fourteen columns
```




    age          int64
    sex          int64
    cp           int64
    restbp       int64
    chol         int64
    fbs          int64
    restecg      int64
    thalach      int64
    exang        int64
    oldpeak    float64
    slope        int64
    ca          object
    thal        object
    hd           int64
    dtype: object



The columns `ca` and `thal`, which are the number of blood vessels counted via fluoroscopy and the parameter corresponding to a thallium heart scan, respectively, have the `object` data type, which implies the existence of a mix of data types. 


```python
df['ca'].unique() 
```




    array(['0', '3', '2', '1', '?'], dtype=object)




```python
df['thal'].unique()
```




    array(['6', '3', '7', '?'], dtype=object)



The question mark, which most likely signifies missing data values, is present in both the `ca` and `thal` columns. To see exactly on which rows the question mark appears, the `df.loc()` method is used, which pinpoints the location in which the parameter value is `true`. 


```python
df.loc[(df['ca'] == '?') | (df['thal'] == '?')] # .loc() shows the location of the rows. 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>hd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>87</th>
      <td>53</td>
      <td>0</td>
      <td>3</td>
      <td>128</td>
      <td>216</td>
      <td>0</td>
      <td>2</td>
      <td>115</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
      <td>0</td>
      <td>?</td>
      <td>0</td>
    </tr>
    <tr>
      <th>166</th>
      <td>52</td>
      <td>1</td>
      <td>3</td>
      <td>138</td>
      <td>223</td>
      <td>0</td>
      <td>0</td>
      <td>169</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
      <td>?</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>43</td>
      <td>1</td>
      <td>4</td>
      <td>132</td>
      <td>247</td>
      <td>1</td>
      <td>2</td>
      <td>143</td>
      <td>1</td>
      <td>0.1</td>
      <td>2</td>
      <td>?</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>266</th>
      <td>52</td>
      <td>1</td>
      <td>4</td>
      <td>128</td>
      <td>204</td>
      <td>1</td>
      <td>0</td>
      <td>156</td>
      <td>1</td>
      <td>1.0</td>
      <td>2</td>
      <td>0</td>
      <td>?</td>
      <td>2</td>
    </tr>
    <tr>
      <th>287</th>
      <td>58</td>
      <td>1</td>
      <td>2</td>
      <td>125</td>
      <td>220</td>
      <td>0</td>
      <td>0</td>
      <td>144</td>
      <td>0</td>
      <td>0.4</td>
      <td>2</td>
      <td>?</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>302</th>
      <td>38</td>
      <td>1</td>
      <td>3</td>
      <td>138</td>
      <td>175</td>
      <td>0</td>
      <td>0</td>
      <td>173</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
      <td>?</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_new = df.loc[(df['ca'] != '?') & (df['thal'] != '?')]
df_new.head() # new DataFrame object with no missing values marked by question mark. 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>hd</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>2</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>2</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>2</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>2</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>2</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



***

## One-Hot Encoding

To minimize the risk of unnecessarily jumbling up the data table, we must separate the table into two parts: the part that contains known classifications (`y`), and the part that contains data used to make predictions (`x`). 

After this is done, we will use one-hot encoding to separate the columns whose values possess categorical meaning into several, independent columns with ones and zeros. The logic behind one-hot encoding is elaborated in more detail on the post about [support vector machines](https://fireformist.github.io/study/Support-Vector-Machines/#one-hot-encoding-changing-categorical-data-to-columns-of-binary-values), but the essence of one-hot encoding lies in treating categorical and quantitative data differently. 

For example, the columns `cp` and `slope` are two of the seven columns that contain categorical data. As for the meaning behind the numerical values, he following information was retrieved alongside the data freely available from the repository. 

> `cp`: chest pain type
> * Value 1: typical angina
> * Value 2: atypical angina
> * Value 3: non-anginal pain
> * Value 4: asymptomatic

> `slope`: the slope of the peak exercise ST segment
> * Value 1: upsloping
> * Value 2: flat
> * Value 3: downsloping



```python
X = df_new.drop('hd', axis=1).copy() # axis=1 implies dropping a column, not a row (axis=0)
y = df_new['hd'].copy()
```


```python
X.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>cp</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>2</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>2</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>4</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>2</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>3</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>2</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>2</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
y.head()
```




    0    0
    1    2
    2    1
    3    0
    4    0
    Name: hd, dtype: int64



To perform one-hot encoding for only one column, we pass in the column name and the dataframe object as parameters to the `get_dummies()` function of the pandas library. For example, should we choose the column `cp`, which stands for 'chest pain,' we can split the column containing four values (1,2,3,4), into four columns containing either zero or one.  


```python
pd.get_dummies(X, columns=['cp']).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>restecg</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>slope</th>
      <th>ca</th>
      <th>thal</th>
      <th>cp_1</th>
      <th>cp_2</th>
      <th>cp_3</th>
      <th>cp_4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>2</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>3</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>2</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>2</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>2</td>
      <td>7</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>3</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>2</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



As we can see, the column `cp` has been split into four independent columns: `cp_1`, `cp_2`, `cp_3`, and `cp_4`. Similarly, by using one-hot encoding, we need to split other columns as well, such as `restecg`, `slope`, `ca`, and `thal`. 


```python
X_encoded = pd.get_dummies(X, columns=['cp', 'restecg', 'slope', 'thal'])
X_encoded.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>sex</th>
      <th>restbp</th>
      <th>chol</th>
      <th>fbs</th>
      <th>thalach</th>
      <th>exang</th>
      <th>oldpeak</th>
      <th>ca</th>
      <th>cp_1</th>
      <th>...</th>
      <th>cp_4</th>
      <th>restecg_0</th>
      <th>restecg_1</th>
      <th>restecg_2</th>
      <th>slope_1</th>
      <th>slope_2</th>
      <th>slope_3</th>
      <th>thal_3</th>
      <th>thal_6</th>
      <th>thal_7</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>1</td>
      <td>145</td>
      <td>233</td>
      <td>1</td>
      <td>150</td>
      <td>0</td>
      <td>2.3</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>67</td>
      <td>1</td>
      <td>160</td>
      <td>286</td>
      <td>0</td>
      <td>108</td>
      <td>1</td>
      <td>1.5</td>
      <td>3</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>67</td>
      <td>1</td>
      <td>120</td>
      <td>229</td>
      <td>0</td>
      <td>129</td>
      <td>1</td>
      <td>2.6</td>
      <td>2</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>37</td>
      <td>1</td>
      <td>130</td>
      <td>250</td>
      <td>0</td>
      <td>187</td>
      <td>0</td>
      <td>3.5</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>41</td>
      <td>0</td>
      <td>130</td>
      <td>204</td>
      <td>0</td>
      <td>172</td>
      <td>0</td>
      <td>1.4</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>



Now that we have one-hot encoded values in the aforementioned four columns, we need to convert values in the `y` object (`pandas.series.Series`) to either zero or one. This is because our goal is binary classification based on whether someone has heart disease or not, using a classification tree. The current `y` object contains five values: `0, 2, 1, 3, 4`, as seen from `y.unique()`, with numbers one, two, three, and four indicating various degrees of heart disease severity. Therefore, we will convert all numbers greater than or equal to one (1,2,3,4) into one, and leave the zero in place. 


```python
y.unique()
# type(y) returns pandas.core.series.Series
```




    array([0, 2, 1, 3, 4], dtype=int64)




```python
y.replace(to_replace=2, value=1, inplace=True)

y.replace(to_replace=3, value=1, inplace=True)
y.replace(to_replace=4, value=1, inplace=True)
y.unique()
```




    array([0, 1], dtype=int64)



***

# Building Decision Tree & Visualizing Results


Now that preprocessing the data is over, we need to use the `DecisionTreeClassifier` function to build the actual tree. To do that, we first perform a 70-30 split of data into training and test data. Then we use a 10-fold cross-validation method with `GridSearchCV` to find the best parameters for `max_depth`, `min_samples_leaf` and `min_samples_split`. In other words, when we split the data into ten folds, we randomly choose seven of them to be training data, and three of them to be test data. After repeating multiple trials with different permutations, the parameters that yield the best accuracy are chosen. The following information was taken from [Scikit-learn's website](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html).


> * max_depth: maximum depth of tree
> * min_samples_split: the minimum number of samples to split a node
> * min_samples_leaf: the minimum number of samples on a leaf node 


The problem central to building accurate decision trees, however, still remains: overfitting the data. We say that we 'overfit' the data, when we overemphasize accuracy when training the model with the training data. Since the actual, unseen data may deviate from the training data, by attempting to build the 'perfect' model with training data, we may actually end up with a less accurate model--especially if the test data significantly deviate from the training data. 

There are many ways to solve the problem, and this article from [towards data science](https://towardsdatascience.com/4-useful-techniques-that-can-mitigate-overfitting-in-decision-trees-87380098bd3c) by Rukshan Pramoditha nicely summarizes various ways to resolve this issue. The StatQuest tutorial used the cost-complexity pruning method to find the optimum parameter value `ccp_alpha`, which essentially describes the degree to which the tree is pruned. Higher values for `ccp_alpha` imply more pruning. 

Given the complexity of the process, I decided to instead use `GridSearchCV` to find the best parameter values for the three parameters specified above. The following code block is from the aforementioned website. 


```python
from sklearn.model_selection import GridSearchCV
import seaborn as sns
import matplotlib.pyplot as plt


X_train, X_test, y_train, y_test = train_test_split(X_encoded, y, random_state=42)
tree = DecisionTreeClassifier(random_state=42)
tree = tree.fit(X_train, y_train)

opt = {'max_depth': [2,3,4,6,8,10,12,15,20], 'min_samples_leaf': [1,2,4,6,8,10,20,30], 'min_samples_split': [1,2,3,4,5,6,8,10]}

grid = GridSearchCV(tree, param_grid=opt, scoring='accuracy', n_jobs=-1, cv=10, return_train_score=True)
grid.fit(X_train, y_train)
grid.best_estimator_.fit(X_train, y_train)


y_predict = grid.best_estimator_.predict(X_test)
y_true = y_test


print(grid.best_params_)

```

    {'max_depth': 3, 'min_samples_leaf': 6, 'min_samples_split': 2}


Here, we see that the maximum tree depth is three, the minimum number of samples on a leaf node is six, and the minimum number of samples on a node for splitting is two. 

In order to visualize the results, we use the `sns.heatmap` function. 


```python
matrix = confusion_matrix(y_true, y_predict)

y_labels = ['No HD', 'Has HD']
x_labels = ['No HD', 'Has HD']



sns.heatmap(matrix, annot=True, cmap='Blues', xticklabels=x_labels, yticklabels=y_labels)

plt.xlabel('Predicted')
plt.ylabel('True')

```




    Text(33.0, 0.5, 'True')



<figure class="align-center">
  <img src="/assets/images/dts.png" alt="">
  <figcaption>Confusion Matrix with Seaborn Heatmap</figcaption>
</figure> 


Here, we see that thirty-four out of fourty-two people with heart disease were correctly classified (80.95%), and among the thirty-three people without heart disease, twenty-eight were correctly classified (84.84%). 

***

# Conclusion

In this post, I explained the concepts of a decision tree, with specific focus on classification trees. Because I did not optimize the `ccp_alpha` parameter necessary for cost-complexity pruning, I could not visualize the tree using the `plot_tree` method, although I hope to use `ccp_alpha` optimization to solve the overfitting problem soon. Nonetheless, the result turned out to be fairly accurate, with over 80% of the test data correctly identified for both categories. This is the end of the post, and I hope you enjoyed reading. 

