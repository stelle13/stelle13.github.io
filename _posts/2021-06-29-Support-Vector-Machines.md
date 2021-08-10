---
title: "Default or No Default?"
toc: true
toc_sticky: true
categories:
  - study
tags:
  - machine-learning
---



In this post, I will briefly go over an example of a Scikit-learn-based implementation of a support vector machine--a popular example of a supervised learning model. The code blocks below came from one of StatQuest's public-domain [tutorials](https://www.youtube.com/watch?v=8A7L0GsBiLQ&t=2298s) on support vector machines, but the line-by-line explanations of the code are in my own words. As usual, the data used in this exercise came from the publicly available [UCI Machine Learning Repository.](https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients)   



# Importing Packages & Data

First, we need to import all the necessary libraries/packages/modules, including `pandas`, `numpy`, `matplotlib`, and etc.   


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.colors as colors
from sklearn.utils import resample
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import scale
from sklearn.svm import SVC
from sklearn.model_selection import GridSearchCV
from sklearn.metrics import confusion_matrix
from sklearn.metrics import plot_confusion_matrix
from sklearn.decomposition import PCA


```


```python
df = pd.read_excel('dccc.xlsx', header=1)
```

Here, we read the excel file we need--"dccc.xlsx"--that I downloaded from the repository. "DCCC" stands for the name of the data--"default of credit card users." 

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
      <th>ID</th>
      <th>LIMIT_BAL</th>
      <th>SEX</th>
      <th>EDUCATION</th>
      <th>MARRIAGE</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>...</th>
      <th>BILL_AMT4</th>
      <th>BILL_AMT5</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>PAY_AMT3</th>
      <th>PAY_AMT4</th>
      <th>PAY_AMT5</th>
      <th>PAY_AMT6</th>
      <th>default payment next month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>20000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>24</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>689</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>120000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>26</td>
      <td>-1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>3272</td>
      <td>3455</td>
      <td>3261</td>
      <td>0</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>0</td>
      <td>2000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>90000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>14331</td>
      <td>14948</td>
      <td>15549</td>
      <td>1518</td>
      <td>1500</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>5000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>50000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>28314</td>
      <td>28959</td>
      <td>29547</td>
      <td>2000</td>
      <td>2019</td>
      <td>1200</td>
      <td>1100</td>
      <td>1069</td>
      <td>1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>50000</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>57</td>
      <td>-1</td>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>...</td>
      <td>20940</td>
      <td>19146</td>
      <td>19131</td>
      <td>2000</td>
      <td>36681</td>
      <td>10000</td>
      <td>9000</td>
      <td>689</td>
      <td>679</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



Here, the columns that read "PAY_AMT" indicate how much was paid over the past six months, with the number following PAY_AMT indicating the month. The numbers in the column for education indicate the level of education that one received. The limit balance tells us how much is left in each person's balance. Finally, default payment next month is the variable that we wish to predict. The following is a brief summary of the meaning of the values.

- Default
    - 0: Did not default
    - 1: Defaulted

- Pay
    - -1: Bill paid on time
    - 1:  Bill paid one month late 
    - n:  Bill paid n months late


```python
df.rename({'default payment next month' : 'Default'}, axis = 'columns', inplace=True)
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
      <th>ID</th>
      <th>LIMIT_BAL</th>
      <th>SEX</th>
      <th>EDUCATION</th>
      <th>MARRIAGE</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>...</th>
      <th>BILL_AMT4</th>
      <th>BILL_AMT5</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>PAY_AMT3</th>
      <th>PAY_AMT4</th>
      <th>PAY_AMT5</th>
      <th>PAY_AMT6</th>
      <th>Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>20000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>24</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>689</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>120000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>26</td>
      <td>-1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>3272</td>
      <td>3455</td>
      <td>3261</td>
      <td>0</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>0</td>
      <td>2000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>90000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>14331</td>
      <td>14948</td>
      <td>15549</td>
      <td>1518</td>
      <td>1500</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>5000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>50000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>28314</td>
      <td>28959</td>
      <td>29547</td>
      <td>2000</td>
      <td>2019</td>
      <td>1200</td>
      <td>1100</td>
      <td>1069</td>
      <td>1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>50000</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>57</td>
      <td>-1</td>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>...</td>
      <td>20940</td>
      <td>19146</td>
      <td>19131</td>
      <td>2000</td>
      <td>36681</td>
      <td>10000</td>
      <td>9000</td>
      <td>689</td>
      <td>679</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



Here, I changed the name of the last column from "default payment next month" to simply "default." To do that, I specified the name of the column before and after the change within curly braces. The `axis='columns'`indicates that I wish to change a column name, and `inplace=True` shows that I am changing the original dataframe object, not a copy of it. 


```python
df.drop('ID', axis=1, inplace=True) # axis=1 parameter for df.drop() indicates a removal of a column.  
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
      <th>LIMIT_BAL</th>
      <th>SEX</th>
      <th>EDUCATION</th>
      <th>MARRIAGE</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>PAY_5</th>
      <th>...</th>
      <th>BILL_AMT4</th>
      <th>BILL_AMT5</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>PAY_AMT3</th>
      <th>PAY_AMT4</th>
      <th>PAY_AMT5</th>
      <th>PAY_AMT6</th>
      <th>Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>24</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-2</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>689</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>120000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>26</td>
      <td>-1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>3272</td>
      <td>3455</td>
      <td>3261</td>
      <td>0</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>0</td>
      <td>2000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>90000</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>14331</td>
      <td>14948</td>
      <td>15549</td>
      <td>1518</td>
      <td>1500</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>5000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>28314</td>
      <td>28959</td>
      <td>29547</td>
      <td>2000</td>
      <td>2019</td>
      <td>1200</td>
      <td>1100</td>
      <td>1069</td>
      <td>1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>50000</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>57</td>
      <td>-1</td>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>20940</td>
      <td>19146</td>
      <td>19131</td>
      <td>2000</td>
      <td>36681</td>
      <td>10000</td>
      <td>9000</td>
      <td>689</td>
      <td>679</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>



# Preprocessing the Data

As with any type of data analysis, the collected data must be preprocessed, and any missing values either need to be found manually, or substituted in via an educated guess. 

## Checking type of data for category

Before we analyze the data, we need to make sure that a single column's data type is uniform for all columns. We do that calling `df.dtypes`, which presents us with a list of the data type for all twenty-four columns.

```python
df.dtypes 
```




    LIMIT_BAL    int64
    SEX          int64
    EDUCATION    int64
    MARRIAGE     int64
    AGE          int64
    PAY_0        int64
    PAY_2        int64
    PAY_3        int64
    PAY_4        int64
    PAY_5        int64
    PAY_6        int64
    BILL_AMT1    int64
    BILL_AMT2    int64
    BILL_AMT3    int64
    BILL_AMT4    int64
    BILL_AMT5    int64
    BILL_AMT6    int64
    PAY_AMT1     int64
    PAY_AMT2     int64
    PAY_AMT3     int64
    PAY_AMT4     int64
    PAY_AMT5     int64
    PAY_AMT6     int64
    Default      int64
    dtype: object




`df.dtypes` shows the uniformity of the data within each column, which tells us two things. 

1. There are no 'mixed' values consisting of letters and numbers.

2. There are no string-type placeholder values for missing data, or 'NA values.'

Unfortunately, when we look at the unique numerical values, we see that zero was used as a placeholder value for missing data. Therefore, we will need to either "impute," or calculate, the value for the placeholder zero, or remove the rows containing the zero. Since the process of cross validation, which we will use later to determine gamma and C (two parameters of SVM models), performs best when there aren't too many rows, we will remove the 68 rows that contain missing values.  

```python
df['EDUCATION'].unique()
```




    array([2, 1, 3, 5, 4, 6, 0], dtype=int64)




```python
df.loc[(df['EDUCATION'] == 0) | (df['MARRIAGE'] == 0)]
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
      <th>LIMIT_BAL</th>
      <th>SEX</th>
      <th>EDUCATION</th>
      <th>MARRIAGE</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>PAY_5</th>
      <th>...</th>
      <th>BILL_AMT4</th>
      <th>BILL_AMT5</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>PAY_AMT3</th>
      <th>PAY_AMT4</th>
      <th>PAY_AMT5</th>
      <th>PAY_AMT6</th>
      <th>Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>218</th>
      <td>110000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>31</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>73315</td>
      <td>63818</td>
      <td>63208</td>
      <td>4000</td>
      <td>5000</td>
      <td>3000</td>
      <td>3000</td>
      <td>3000</td>
      <td>8954</td>
      <td>0</td>
    </tr>
    <tr>
      <th>809</th>
      <td>160000</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>28574</td>
      <td>27268</td>
      <td>28021</td>
      <td>35888</td>
      <td>1325</td>
      <td>891</td>
      <td>1000</td>
      <td>1098</td>
      <td>426</td>
      <td>0</td>
    </tr>
    <tr>
      <th>820</th>
      <td>200000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>51</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>0</td>
      <td>...</td>
      <td>780</td>
      <td>390</td>
      <td>390</td>
      <td>0</td>
      <td>390</td>
      <td>780</td>
      <td>0</td>
      <td>390</td>
      <td>390</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1019</th>
      <td>180000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>45</td>
      <td>-1</td>
      <td>-1</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1443</th>
      <td>200000</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>51</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>0</td>
      <td>...</td>
      <td>2529</td>
      <td>1036</td>
      <td>4430</td>
      <td>5020</td>
      <td>9236</td>
      <td>2529</td>
      <td>0</td>
      <td>4430</td>
      <td>6398</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>28602</th>
      <td>200000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>37</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>4000</td>
      <td>22800</td>
      <td>5716</td>
      <td>35000</td>
      <td>5000</td>
      <td>4000</td>
      <td>22800</td>
      <td>5716</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28603</th>
      <td>110000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>44</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>41476</td>
      <td>42090</td>
      <td>43059</td>
      <td>2000</td>
      <td>2000</td>
      <td>1700</td>
      <td>1600</td>
      <td>1800</td>
      <td>1800</td>
      <td>1</td>
    </tr>
    <tr>
      <th>28766</th>
      <td>80000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>40</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>...</td>
      <td>1375</td>
      <td>779</td>
      <td>5889</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>885</td>
      <td>5889</td>
      <td>4239</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29078</th>
      <td>100000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>56</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>...</td>
      <td>31134</td>
      <td>30444</td>
      <td>32460</td>
      <td>0</td>
      <td>1500</td>
      <td>2700</td>
      <td>0</td>
      <td>2400</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29111</th>
      <td>300000</td>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>53</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>68 rows × 24 columns</p>
</div>




```python
len(df.loc[(df['EDUCATION'] == 0) | (df['MARRIAGE'] == 0)])
```




    68



Here, based on the fact that the zero values in either the education or the marriage columns imply missing values, we will remove the rows containing the zeros. In fact, as shown above, there are a total of 68 rows that contain zero for either the education or the marriage columns. These rows will be removed, and then the data will be trimmed to a 1,000 rows, since support vector machines work best for moderate sized datasets. 

## Removal of rows containing missing data & Trimming of the dataset


To create a new dataset containing less numbers of rows and no zeros for missing values, we will create a new dataframe object called `df_no_missing`


```python
df_no_missing = df.loc[(df['EDUCATION'] != 0) & (df['MARRIAGE'] != 0)]
```


```python
print('length of new dataframe : ', len(df_no_missing))
print(df_no_missing['EDUCATION'].unique())
print(df_no_missing['MARRIAGE'].unique())
```

    length of new dataframe :  29932
    [2 1 3 5 4 6]
    [1 2 3]


`.unique()` shows the unique values within the column. As we can see, there are no zeros in any of the two columns, 'education' and 'marriage.'

Support vector machine requires optimization for the right value of parameter X, which in turn necessitates a process called 'cross validation.' Unfortunately, cross validation works efficiently with relatively moderate sized datasets, so we need to downsample the size of the dataset to a 1,000. 

Here, we split the default column into two different dataframe objects--one containing zero's and the other containing one's. 


```python
df_no_default = df_no_missing[df_no_missing['Default'] == 0]
df_default = df_no_missing[df_no_missing['Default'] == 1]

```

Then we resample from the pool of around 30,000 rows to select 1,000 rows. The `resample` method takes `replace=False` to mean that the selection pool is not refilled with already selected choices, and `n_samples=1000` specifies the number of rows (samples) to select. 


```python
df_no_default_downsampled = resample(df_default, replace=False, n_samples=1000, random_state=42)
df_default_downsampled = resample(df_no_default, replace=False, n_samples=1000, random_state=42)
```

Then we join the two samples together to make a joint dataframe object of 2,000 rows that were selected via `resample()`. 


```python
df_downsample = pd.concat([df_no_default_downsampled, df_default_downsampled])
len(df_downsample)
```




    2000



## One-Hot Encoding: Changing categorical data to columns of binary values

Above, the category corresponding to marriage contained numbers 1, 2, and 3. If we take these counts to be continuous, then the support vector machine is more likely to view people with closer numbers--say, 2 and 3--as similar when creating the decision boundary. 

However, since the integers in that category are categorical, two people with values closer together (2,3) are equally likely to be similar to each other (in terms of default status), than pairs with values not closer together, such as (1,2).  

Due to this problem, we need to convert the column containing 1's, 2's, and 3's for marital status into three separate columns, containing only 0's and 1's. 


```python
X = df_downsample.drop('Default', axis=1).copy()
y = df_downsample['Default'].copy()

pd.get_dummies(X, columns=['MARRIAGE']).head()
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
      <th>LIMIT_BAL</th>
      <th>SEX</th>
      <th>EDUCATION</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>PAY_5</th>
      <th>PAY_6</th>
      <th>...</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>PAY_AMT3</th>
      <th>PAY_AMT4</th>
      <th>PAY_AMT5</th>
      <th>PAY_AMT6</th>
      <th>MARRIAGE_1</th>
      <th>MARRIAGE_2</th>
      <th>MARRIAGE_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19982</th>
      <td>300000</td>
      <td>2</td>
      <td>1</td>
      <td>47</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19350</th>
      <td>80000</td>
      <td>2</td>
      <td>2</td>
      <td>36</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>0</td>
      <td>1700</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17057</th>
      <td>30000</td>
      <td>2</td>
      <td>3</td>
      <td>22</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>11711</td>
      <td>0</td>
      <td>1687</td>
      <td>1147</td>
      <td>524</td>
      <td>400</td>
      <td>666</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26996</th>
      <td>80000</td>
      <td>1</td>
      <td>1</td>
      <td>34</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>...</td>
      <td>67007</td>
      <td>2800</td>
      <td>3000</td>
      <td>2500</td>
      <td>2600</td>
      <td>2600</td>
      <td>2600</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23621</th>
      <td>210000</td>
      <td>2</td>
      <td>3</td>
      <td>44</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>14793</td>
      <td>13462</td>
      <td>17706</td>
      <td>0</td>
      <td>5646</td>
      <td>14793</td>
      <td>7376</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>




```python
X_encoded = pd.get_dummies(X, columns=['SEX', 'EDUCATION', 'MARRIAGE', 'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6'])
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
      <th>LIMIT_BAL</th>
      <th>AGE</th>
      <th>BILL_AMT1</th>
      <th>BILL_AMT2</th>
      <th>BILL_AMT3</th>
      <th>BILL_AMT4</th>
      <th>BILL_AMT5</th>
      <th>BILL_AMT6</th>
      <th>PAY_AMT1</th>
      <th>PAY_AMT2</th>
      <th>...</th>
      <th>PAY_5_7</th>
      <th>PAY_6_-2</th>
      <th>PAY_6_-1</th>
      <th>PAY_6_0</th>
      <th>PAY_6_2</th>
      <th>PAY_6_3</th>
      <th>PAY_6_4</th>
      <th>PAY_6_5</th>
      <th>PAY_6_6</th>
      <th>PAY_6_7</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19982</th>
      <td>300000</td>
      <td>47</td>
      <td>5000</td>
      <td>5000</td>
      <td>5000</td>
      <td>5000</td>
      <td>5000</td>
      <td>5000</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19350</th>
      <td>80000</td>
      <td>36</td>
      <td>19671</td>
      <td>20650</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1700</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17057</th>
      <td>30000</td>
      <td>22</td>
      <td>29793</td>
      <td>29008</td>
      <td>29047</td>
      <td>29507</td>
      <td>11609</td>
      <td>11711</td>
      <td>0</td>
      <td>1687</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26996</th>
      <td>80000</td>
      <td>34</td>
      <td>61231</td>
      <td>62423</td>
      <td>63827</td>
      <td>64682</td>
      <td>65614</td>
      <td>67007</td>
      <td>2800</td>
      <td>3000</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23621</th>
      <td>210000</td>
      <td>44</td>
      <td>11771</td>
      <td>13462</td>
      <td>17706</td>
      <td>0</td>
      <td>5646</td>
      <td>14793</td>
      <td>13462</td>
      <td>17706</td>
      <td>...</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 81 columns</p>
</div>



## Scaling the data for using the radial basis function in SVM

To use the radial basis function as the kernel function in the SVM module of Scikit-learn (`kernal=rbf`), the data in those 81 columns need to be scaled to have mean=0 and std=1. We will scale the data using the scale() function. 

By using `train_test_split`, we will split up the data in the X_encoded and y objects into training data (75% of total data in X_encoded and y) and testing data (25% of total data in X_encoded and y).


```python
X_train, X_test, y_train, y_test = train_test_split(X_encoded, y, random_state=42)
X_train_scaled  = scale(X_train)
X_test_scaled = scale(X_test)
```



# Creating and interpreting the SVM model


## Using GridSearchCV to find C and gamma

To optimize the regularization constant C (higher C means choosing lower margins between hyperplane and data points for accuracy improvement), gamma (higher gamma means prioritizing points closer to the hyperplane), we will use a `GridSearchCV` object called `optimal_params`. For the kernel function, the `GridSearchCV` object could suggest other possible kernel functions aside from `rbf` (observe the cv=5, the cross validation fold number parameter), but for simplicity, we will use `rbf`. 


```python
param_grid = [{'C': [0.5, 1, 10, 100], 'gamma': ['scale', 1, 0.1, 0.01, 0.001, 0.0001], 'kernel': ['rbf']}]

optimal_params = GridSearchCV(SVC(), param_grid, cv=5, scoring='accuracy', verbose=0)

optimal_params.fit(X_train_scaled, y_train)
print(optimal_params.best_params_)
```

    {'C': 100, 'gamma': 0.001, 'kernel': 'rbf'}


## Creating and interpreting the SVM model

Here, we will create an SVM object by specifying the C (regularization constant) as 100, and gamma as 0.001. Then we must "train" the object, or allow it to find the optimum parameters for constructing the decision boundary. We will use `.fit()` to accomplish the task. 


```python
shell_of_SVM = SVC(random_state=42, C=100, gamma=0.001)
shell_of_SVM.fit(X_train_scaled, y_train) # "training the SVM", or finding the optimum parameters for the SVM decision boundary

plot_confusion_matrix(shell_of_SVM, X_test_scaled, y_test, values_format='d', display_labels = ["No Default", "Defaulted"])

```




    <sklearn.metrics._plot.confusion_matrix.ConfusionMatrixDisplay at 0x1f826a77970>



<figure class="align-center">
  <img src="/assets/images/svm.png" alt="">
  <figcaption>Confusion Matrix with Scikit-learn</figcaption>
</figure> 


Here is the result, which shows that 187 people out of the 243 that did not default were correctly predicted to have not defaulted, For the 237 people that did default, 167 people were correctly predicted to have defaulted.  

***
# Conclusion

In this post, I explained how to implement a support vector machine model using Scikit-learn, referencing one of StatQuest's tutorials on support vector machines. Even though the actual `SVC()` object did not come until the very end, I believe that speaks to the difficulty and importance of preprocessing the data, which actually took up the bulk of the work. As for optimizing the parameters of the SVC model, I used cross-validation using `GridSearchCV` function, and furthermore, I used `rbf` (radial basis function) for the kernel. As for the latter, I do not yet fully understand its complete mathematical basis, but in the future, I hope that I can to the fullest extent. I hope you enjoyed reading this post.  
