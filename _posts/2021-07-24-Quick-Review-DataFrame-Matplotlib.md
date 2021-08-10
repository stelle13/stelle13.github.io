---
title: "Quick Review of DataFrame & Matplotlib"
date: 2021-07-24
toc: true
toc_sticky: true
categories:
  - study
tags:
  - Reference
---



In this post, I created my own reference guide to `dataframe` and `matplotlib`.  

## Motivation

In the previous posts, I have used the `dataframe` module of the `pandas` library quite extensively, especially when building simple Scikit-learn based ML models or analyzing data. In particular, while importing the data from Excel or CSV file formats, preprocessing the data to remove rows with missing or invalid values, and one-hot-encoding columns, I have used various functions of the `dataframe` module. That being said, it occurred to me that having a quick reference for the functions may be useful in the future, should I use those functions for a data analysis project or something. Even though the Pandas website already has a complete compendium of all the `dataframe` module functions, I thought that using such a personal reference guide would be more convenient than searching the web every time. (Quick side note, the official compilation of `dataframe` functions can be found at this [website](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)). Of course, I plan to update this rather short list as I learn more about `dataframe` functions. 

For similar reasons, I decided to do the same with `matplotlib`, explaining in my own language, the various visualization methods of the library. Since I am planning to publish a similar future post on R, I believe that having a subsection in this post about `matplotlib` will facilitate differentiating between those two. So without any further due, let's get straight to `dataframe`. 


# Review of `DataFrame` Functions

## Importing File & Multiple Headers

### Import Excel File: `.read_excel()`


```python
import pandas as pd
```


```python
file = pd.read_excel('C:/Users/firef/Downloads/creditCard.xlsx')
file.head()
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
      <th>Unnamed: 0</th>
      <th>X1</th>
      <th>X2</th>
      <th>X3</th>
      <th>X4</th>
      <th>X5</th>
      <th>X6</th>
      <th>X7</th>
      <th>X8</th>
      <th>X9</th>
      <th>...</th>
      <th>X15</th>
      <th>X16</th>
      <th>X17</th>
      <th>X18</th>
      <th>X19</th>
      <th>X20</th>
      <th>X21</th>
      <th>X22</th>
      <th>X23</th>
      <th>Y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ID</td>
      <td>LIMIT_BAL</td>
      <td>SEX</td>
      <td>EDUCATION</td>
      <td>MARRIAGE</td>
      <td>AGE</td>
      <td>PAY_0</td>
      <td>PAY_2</td>
      <td>PAY_3</td>
      <td>PAY_4</td>
      <td>...</td>
      <td>BILL_AMT4</td>
      <td>BILL_AMT5</td>
      <td>BILL_AMT6</td>
      <td>PAY_AMT1</td>
      <td>PAY_AMT2</td>
      <td>PAY_AMT3</td>
      <td>PAY_AMT4</td>
      <td>PAY_AMT5</td>
      <td>PAY_AMT6</td>
      <td>default payment next month</td>
    </tr>
    <tr>
      <th>1</th>
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
      <th>2</th>
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
      <th>3</th>
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
      <th>4</th>
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
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



Specifying the header parameter `header=1` in `.read_excel()` removes the header row. 


```python
file1 = pd.read_excel('C:/Users/firef/Downloads/creditCard.xlsx', header=1) 
file1.head()
# print(type(file1)) # file1 is a pandas.core.frame.DataFrame object
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
      <th>29995</th>
      <td>29996</td>
      <td>220000</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>88004</td>
      <td>31237</td>
      <td>15980</td>
      <td>8500</td>
      <td>20000</td>
      <td>5003</td>
      <td>3047</td>
      <td>5000</td>
      <td>1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29996</th>
      <td>29997</td>
      <td>150000</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>43</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>8979</td>
      <td>5190</td>
      <td>0</td>
      <td>1837</td>
      <td>3526</td>
      <td>8998</td>
      <td>129</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29997</th>
      <td>29998</td>
      <td>30000</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>37</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>...</td>
      <td>20878</td>
      <td>20582</td>
      <td>19357</td>
      <td>0</td>
      <td>0</td>
      <td>22000</td>
      <td>4200</td>
      <td>2000</td>
      <td>3100</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29998</th>
      <td>29999</td>
      <td>80000</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>41</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>52774</td>
      <td>11855</td>
      <td>48944</td>
      <td>85900</td>
      <td>3409</td>
      <td>1178</td>
      <td>1926</td>
      <td>52964</td>
      <td>1804</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29999</th>
      <td>30000</td>
      <td>50000</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>46</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>36535</td>
      <td>32428</td>
      <td>15313</td>
      <td>2078</td>
      <td>1800</td>
      <td>1430</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



Since we passed in the Excel file into `pd.read_excel()` function, the resulting data type of `file1` is `pandas.core.frame.DataFrame`.



```python
type(file1)
```




    pandas.core.frame.DataFrame



***

## Modifying `DataFrame` Objects

***

### Removing Columns: `.drop('name', axis=1)`


```python
file1.drop('ID', axis=1, inplace=False).head() # inplace=False : 
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
      <th>default payment next month</th>
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



The `inplace=False` returns a copy of the original `DataFrame` object, with the desired columns removed. 


```python
file1.head()
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



As seen above, the original `DataFrame` object has not been changed. 

***

### Removing Rows: `axis=0`


```python
file1.drop(0, axis=0, inplace=False).head()
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
    <tr>
      <th>5</th>
      <td>6</td>
      <td>50000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19394</td>
      <td>19619</td>
      <td>20024</td>
      <td>2500</td>
      <td>1815</td>
      <td>657</td>
      <td>1000</td>
      <td>1000</td>
      <td>800</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



***

### Retrieving Part of DataFrame Object: `.Series.to_frame()`


```python
# First Step: Getting a part of the DataFrame object as a series
onlyMarriage = file1['MARRIAGE'] # gets only the MARRIAGE column
onlyMarriage.head() # type of onlyMarriage = pandas.core.series.Series

```




    0    1
    1    2
    2    2
    3    1
    4    1
    Name: MARRIAGE, dtype: int64




```python
# Converting the series into a dataframe object

dfMarriage = pd.Series.to_frame(onlyMarriage, name="MARRIAGE")
dfMarriage # parameter `name` is the name of the new column
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
      <th>MARRIAGE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>29995</th>
      <td>1</td>
    </tr>
    <tr>
      <th>29996</th>
      <td>2</td>
    </tr>
    <tr>
      <th>29997</th>
      <td>2</td>
    </tr>
    <tr>
      <th>29998</th>
      <td>1</td>
    </tr>
    <tr>
      <th>29999</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>30000 rows × 1 columns</p>
</div>



***

### One-Hot Encoding Columns: `.get_dummies()`


```python
# First, check the unique values of the about-to-split column

file1['EDUCATION'].unique() 
```




    array([2, 1, 3, 5, 4, 6, 0], dtype=int64)




```python
# Specify the columns to be split using one-hot encoding in columns = [] parameter
pdFile = pd.get_dummies(file1, columns=['EDUCATION', 'SEX'])
pdFile.head()
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
      <th>MARRIAGE</th>
      <th>AGE</th>
      <th>PAY_0</th>
      <th>PAY_2</th>
      <th>PAY_3</th>
      <th>PAY_4</th>
      <th>PAY_5</th>
      <th>PAY_6</th>
      <th>...</th>
      <th>default payment next month</th>
      <th>EDUCATION_0</th>
      <th>EDUCATION_1</th>
      <th>EDUCATION_2</th>
      <th>EDUCATION_3</th>
      <th>EDUCATION_4</th>
      <th>EDUCATION_5</th>
      <th>EDUCATION_6</th>
      <th>SEX_1</th>
      <th>SEX_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>20000</td>
      <td>1</td>
      <td>24</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>-1</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
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
      <td>26</td>
      <td>-1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>...</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>90000</td>
      <td>2</td>
      <td>34</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
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
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>50000</td>
      <td>1</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
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
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>50000</td>
      <td>1</td>
      <td>57</td>
      <td>-1</td>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 32 columns</p>
</div>



***

### Replacing Values In Column: `.replace()`


```python
file1.head()
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




```python
file1['EDUCATION'].replace(to_replace=2, value=1, inplace=True)
file1.head() # Values in Education column changed to one
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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



***

### Trimming Dataset (Downsampling): `resample()`, `.concat()`


```python
# CAUTION: Split the DataFrame object beforehand to ensure the proportion of different values 
# before the sampling equals the proportion of different values after the sampling. 

no_default = file1[file1['default payment next month'] == 0]
default = file1[file1['default payment next month'] == 1]

no_default.head() # default payment next month column is all zero
# default.head()
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
      <th>2</th>
      <td>3</td>
      <td>90000</td>
      <td>2</td>
      <td>1</td>
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
      <td>1</td>
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
      <td>1</td>
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
    <tr>
      <th>5</th>
      <td>6</td>
      <td>50000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>37</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19394</td>
      <td>19619</td>
      <td>20024</td>
      <td>2500</td>
      <td>1815</td>
      <td>657</td>
      <td>1000</td>
      <td>1000</td>
      <td>800</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>500000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>29</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>542653</td>
      <td>483003</td>
      <td>473944</td>
      <td>55000</td>
      <td>40000</td>
      <td>38000</td>
      <td>20239</td>
      <td>13750</td>
      <td>13770</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>




```python
from sklearn.utils import resample

file1_no_default_downsampled = resample(no_default, replace=False, n_samples = 100, random_state=42)
file1_default_downsampled = resample(default, replace=False, n_samples = 100, random_state=42)

print("Length of the new downsampled dataframe object: ", len(file1_no_default_downsampled))

file1_no_default_downsampled.head() # resampled file1_no_default_downsampled
```

    Length of the new downsampled dataframe object:  100





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
      <th>7510</th>
      <td>7511</td>
      <td>380000</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>31</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>11147</td>
      <td>12483</td>
      <td>13680</td>
      <td>9240</td>
      <td>15233</td>
      <td>11202</td>
      <td>12493</td>
      <td>13748</td>
      <td>18061</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15325</th>
      <td>15326</td>
      <td>240000</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>35</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
      <td>...</td>
      <td>5638</td>
      <td>2582</td>
      <td>4127</td>
      <td>7375</td>
      <td>4908</td>
      <td>5638</td>
      <td>2587</td>
      <td>4127</td>
      <td>4942</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18666</th>
      <td>18667</td>
      <td>50000</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>-1</td>
      <td>-1</td>
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
      <th>7494</th>
      <td>7495</td>
      <td>330000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>141453</td>
      <td>112633</td>
      <td>121242</td>
      <td>5500</td>
      <td>4723</td>
      <td>5500</td>
      <td>4000</td>
      <td>10700</td>
      <td>4500</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1239</th>
      <td>1240</td>
      <td>80000</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>35</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>396</td>
      <td>396</td>
      <td>396</td>
      <td>9796</td>
      <td>13443</td>
      <td>396</td>
      <td>396</td>
      <td>0</td>
      <td>396</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>




```python
# Concatenate the two parts:

file1_downsampled = pd.concat([file1_no_default_downsampled, file1_default_downsampled])
len(file1_downsampled) # concatenate two pre-separated parts consisting of 100 rows each
```




    200



***

### Appending Rows: `append()`


```python
file1.head()
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




```python
# adds the extraRow to the end 
extraRow = [0, 10000, 2, 2, 1, 23, 2, 2,-1,-1,-2,-2,3913,3102,689,0,0,0,0,689,0,0,0,0,1]
file1.loc[len(file1.index)] = extraRow
file1.tail()
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
      <th>29996</th>
      <td>29997</td>
      <td>150000</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>43</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>...</td>
      <td>8979</td>
      <td>5190</td>
      <td>0</td>
      <td>1837</td>
      <td>3526</td>
      <td>8998</td>
      <td>129</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29997</th>
      <td>29998</td>
      <td>30000</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>37</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>-1</td>
      <td>...</td>
      <td>20878</td>
      <td>20582</td>
      <td>19357</td>
      <td>0</td>
      <td>0</td>
      <td>22000</td>
      <td>4200</td>
      <td>2000</td>
      <td>3100</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29998</th>
      <td>29999</td>
      <td>80000</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>41</td>
      <td>1</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>52774</td>
      <td>11855</td>
      <td>48944</td>
      <td>85900</td>
      <td>3409</td>
      <td>1178</td>
      <td>1926</td>
      <td>52964</td>
      <td>1804</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29999</th>
      <td>30000</td>
      <td>50000</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>46</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>36535</td>
      <td>32428</td>
      <td>15313</td>
      <td>2078</td>
      <td>1800</td>
      <td>1430</td>
      <td>1000</td>
      <td>1000</td>
      <td>1000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>30000</th>
      <td>0</td>
      <td>10000</td>
      <td>2</td>
      <td>2</td>
      <td>1</td>
      <td>23</td>
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
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



***

## Identifying Values: Columns, Rows

### Column Names, Values: `.columns`


```python
list(file1.columns) # checks all the columns, puts column names in list
```




    ['ID',
     'LIMIT_BAL',
     'SEX',
     'EDUCATION',
     'MARRIAGE',
     'AGE',
     'PAY_0',
     'PAY_2',
     'PAY_3',
     'PAY_4',
     'PAY_5',
     'PAY_6',
     'BILL_AMT1',
     'BILL_AMT2',
     'BILL_AMT3',
     'BILL_AMT4',
     'BILL_AMT5',
     'BILL_AMT6',
     'PAY_AMT1',
     'PAY_AMT2',
     'PAY_AMT3',
     'PAY_AMT4',
     'PAY_AMT5',
     'PAY_AMT6',
     'default payment next month']




```python
file1['ID'].head() # checks for column: 'ID'
```




    0    1
    1    2
    2    3
    3    4
    4    5
    Name: ID, dtype: int64




```python
file1['EDUCATION'].head() # checks for column: "EDUCATION"
```




    0    2
    1    2
    2    2
    3    2
    4    2
    Name: EDUCATION, dtype: int64



### Row Names, Values: `.index` and `.iloc[]`


```python
list(file1.index)[0:6] # puts the row index numbers into a list, displays first five elements
```




    [0, 1, 2, 3, 4, 5]




```python
file1.iloc[0] # prints the first row with index zero. 
```




    ID                                1
    LIMIT_BAL                     20000
    SEX                               2
    EDUCATION                         2
    MARRIAGE                          1
    AGE                              24
    PAY_0                             2
    PAY_2                             2
    PAY_3                            -1
    PAY_4                            -1
    PAY_5                            -2
    PAY_6                            -2
    BILL_AMT1                      3913
    BILL_AMT2                      3102
    BILL_AMT3                       689
    BILL_AMT4                         0
    BILL_AMT5                         0
    BILL_AMT6                         0
    PAY_AMT1                          0
    PAY_AMT2                        689
    PAY_AMT3                          0
    PAY_AMT4                          0
    PAY_AMT5                          0
    PAY_AMT6                          0
    default payment next month        1
    Name: 0, dtype: int64



### Value At Row X, Column Y: `.at[]`


```python
file1.at[0, 'EDUCATION'] # prints value at index zero, column "EDUCATION"
```




    2



### Checking Data Type: `.dtypes`


```python
file1.dtypes # checks data types of each column
```




    ID                            int64
    LIMIT_BAL                     int64
    SEX                           int64
    EDUCATION                     int64
    MARRIAGE                      int64
    AGE                           int64
    PAY_0                         int64
    PAY_2                         int64
    PAY_3                         int64
    PAY_4                         int64
    PAY_5                         int64
    PAY_6                         int64
    BILL_AMT1                     int64
    BILL_AMT2                     int64
    BILL_AMT3                     int64
    BILL_AMT4                     int64
    BILL_AMT5                     int64
    BILL_AMT6                     int64
    PAY_AMT1                      int64
    PAY_AMT2                      int64
    PAY_AMT3                      int64
    PAY_AMT4                      int64
    PAY_AMT5                      int64
    PAY_AMT6                      int64
    default payment next month    int64
    dtype: object



### Checking Data Values: `.unique()`


```python
file1['EDUCATION'].unique() # returns unique values in column 'education'
```




    array([2, 1, 3, 5, 4, 6, 0], dtype=int64)



### Location of Specific Values: `.loc[]`


```python
file1.loc[file1['AGE'] == 25].head() # locates and shows all rows where AGE == 25
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
      <th>38</th>
      <td>39</td>
      <td>50000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>25</td>
      <td>1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-2</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>780</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>41</th>
      <td>42</td>
      <td>70000</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>25</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>63699</td>
      <td>64718</td>
      <td>65970</td>
      <td>3000</td>
      <td>4500</td>
      <td>4042</td>
      <td>2500</td>
      <td>2800</td>
      <td>2500</td>
      <td>0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>54</td>
      <td>180000</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>25</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>43510</td>
      <td>44420</td>
      <td>45319</td>
      <td>1300</td>
      <td>2010</td>
      <td>1762</td>
      <td>1762</td>
      <td>1790</td>
      <td>1622</td>
      <td>0</td>
    </tr>
    <tr>
      <th>76</th>
      <td>77</td>
      <td>50000</td>
      <td>1</td>
      <td>3</td>
      <td>2</td>
      <td>25</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>9636</td>
      <td>9590</td>
      <td>10030</td>
      <td>1759</td>
      <td>1779</td>
      <td>320</td>
      <td>500</td>
      <td>1000</td>
      <td>1000</td>
      <td>0</td>
    </tr>
    <tr>
      <th>151</th>
      <td>152</td>
      <td>80000</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>25</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>41087</td>
      <td>41951</td>
      <td>31826</td>
      <td>30000</td>
      <td>3000</td>
      <td>6000</td>
      <td>8000</td>
      <td>2000</td>
      <td>14000</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



## Matplotlib 

***

### Plotting Lists: `.plot()`


```python
import matplotlib.pyplot as plt
import numpy as np

data = [10, 15, 10, 15, 5, 20, 30, 15, 10]

plt.plot(data)
plt.show()

```


<figure class="align-center">
  <img src="/assets/images/df1.png" alt="">
  <figcaption>Matplotlib Graph</figcaption>
</figure> 
 


***
### Plotting on Same Graph: `.plot(x,y1,x,y2 ...)`


```python
import numpy as np

X = np.linspace(-10, 10, num=30, endpoint=True)
y1=X**2
y2=X+15
y3=-X**2 + 50

plt.plot(X,y1, X,y2, X,y3)
plt.grid(True)
```


<figure class="align-center">
  <img src="/assets/images/df2.png" alt="">
  <figcaption>Plotting Multiple Graphs</figcaption>
</figure> 


***

### Subplot: `.subplot()`


```python
X = np.linspace(-np.pi,np.pi, num=50, endpoint=True)

y1 = np.sin(X)
y2 = np.cos(X)
y3 = np.tan(X)
y4 = X

plt.subplot(2,2,1)
plt.plot(X,y1)
plt.grid(True)

plt.subplot(2,2,2)
plt.plot(X,y2)
plt.grid(True)

plt.subplot(2,2,3)
plt.plot(X,y3)
plt.grid(True)

plt.subplot(2,2,4)
plt.plot(X,y4)
plt.grid(True)

plt.show()

```


<figure class="align-center">
  <img src="/assets/images/df3.png" alt="">
  <figcaption>Subplots</figcaption>
</figure> 

***

### Adding Legend: `.legend`


```python
import numpy as np
import matplotlib.pyplot as plt

X = np.linspace(-4*np.pi,4*np.pi, num=320, endpoint=True)

y5 = np.sin(X) * 15
y6 = np.sin(X) * -15

plt.plot(X, y5, 'c', X, y6, 'y')
plt.grid(True)
plt.legend(['data1', 'data2'], loc = 'lower right')
plt.title('Trigonometric Functions: Sin() and Cos()')
plt.show() # b: blue, g: green, c: cyan, r:red, etc
```



<figure class="align-center">
  <img src="/assets/images/df4.png" alt="">
  <figcaption>Trig Functions</figcaption>
</figure> 





***

### Style of Lines: `(Use parameters)`


```python
X = np.linspace(-10,10, num=20)

y1 = 1.5*X
y2 = 1.5*X + 6
y3 = 1.5*X + 12
y4 = 1.5*X + 18

plt.plot(X, y1, '--r', X, y2, ':m', X, y3, '-.b', X, y4, 'dg')
plt.legend(['data1', 'data2','data3', 'data4'], loc = 'lower right')
plt.title('Lines With Differing Styles')
plt.show()
```



<figure class="align-center">
  <img src="/assets/images/df5.png" alt="">
  <figcaption>Lines with Different Styles</figcaption>
</figure> 





***

### Radar Charts


```python
import numpy as np
import matplotlib.pyplot as plt


# Fixing random state for reproducibility
np.random.seed(19680801)

# Compute pie slices
N = 20
theta = np.linspace(0.0, 2 * np.pi, N, endpoint=False)
radii = 10 * np.random.rand(N) # adjust radii by setting 10 to a different number. Right now, radii is between 0 and 10
width = np.pi / 4 * np.random.rand(N) # adjust width by setting 6 to a different number
colors = plt.cm.viridis(radii / 10.) # adjust color: viridis, magma, plasma, inferno

ax = plt.subplot(projection='polar')
ax.bar(theta, radii, width=width, bottom=0.0, color=colors, alpha=0.5)

plt.show()
```

    

<figure class="align-center">
  <img src="/assets/images/df6.png" alt="">
  <figcaption>Radar Charts</figcaption>
</figure> 


***

### Heatmap


```python
import pandas as pd
import numpy as np

vegetables = ["cucumber", "tomato", "lettuce", "asparagus",
              "potato", "wheat", "barley"]
farmers = ["Farmer Joe", "Upland Bros.", "Smith Gardening",
           "Agrifun", "Organiculture", "BioGoods Ltd.", "Cornylee Corp."]

harvest = [[0.8, 2.4, 2.5, 3.9, 0.0, 4.0, 0.0],
                    [2.4, 0.0, 4.0, 1.0, 2.7, 0.0, 0.0],
                    [1.1, 2.4, 0.8, 4.3, 1.9, 4.4, 0.0],
                    [0.6, 0.0, 0.3, 0.0, 3.1, 0.0, 0.0],
                    [0.7, 1.7, 0.6, 2.6, 2.2, 6.2, 0.0],
                    [1.3, 1.2, 0.0, 0.0, 0.0, 3.2, 5.1],
                    [0.1, 2.0, 0.0, 1.4, 0.0, 1.9, 6.3]]

df1 = pd.DataFrame(harvest, columns=farmers, index=vegetables)
df1
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
      <th>Farmer Joe</th>
      <th>Upland Bros.</th>
      <th>Smith Gardening</th>
      <th>Agrifun</th>
      <th>Organiculture</th>
      <th>BioGoods Ltd.</th>
      <th>Cornylee Corp.</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>cucumber</th>
      <td>0.8</td>
      <td>2.4</td>
      <td>2.5</td>
      <td>3.9</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>tomato</th>
      <td>2.4</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>2.7</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>lettuce</th>
      <td>1.1</td>
      <td>2.4</td>
      <td>0.8</td>
      <td>4.3</td>
      <td>1.9</td>
      <td>4.4</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>asparagus</th>
      <td>0.6</td>
      <td>0.0</td>
      <td>0.3</td>
      <td>0.0</td>
      <td>3.1</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>potato</th>
      <td>0.7</td>
      <td>1.7</td>
      <td>0.6</td>
      <td>2.6</td>
      <td>2.2</td>
      <td>6.2</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>wheat</th>
      <td>1.3</td>
      <td>1.2</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.2</td>
      <td>5.1</td>
    </tr>
    <tr>
      <th>barley</th>
      <td>0.1</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>1.4</td>
      <td>0.0</td>
      <td>1.9</td>
      <td>6.3</td>
    </tr>
  </tbody>
</table>
</div>




```python
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

vegetables = ["cucumber", "tomato", "lettuce", "asparagus",
              "potato", "wheat", "barley"]
farmers = ["Farmer Joe", "Upland Bros.", "Smith Gardening",
           "Agrifun", "Organiculture", "BioGoods Ltd.", "Cornylee Corp."]

harvest = np.array([[0.8, 2.4, 2.5, 3.9, 0.0, 4.0, 0.0],
                    [2.4, 0.0, 4.0, 1.0, 2.7, 0.0, 0.0],
                    [1.1, 2.4, 0.8, 4.3, 1.9, 4.4, 0.0],
                    [0.6, 0.0, 0.3, 0.0, 3.1, 0.0, 0.0],
                    [0.7, 1.7, 0.6, 2.6, 2.2, 6.2, 0.0],
                    [1.3, 1.2, 0.0, 0.0, 0.0, 3.2, 5.1],
                    [0.1, 2.0, 0.0, 1.4, 0.0, 1.9, 6.3]])


fig, ax = plt.subplots()
im = ax.imshow(harvest)

# We want to show all ticks...
ax.set_xticks(np.arange(len(farmers)))
ax.set_yticks(np.arange(len(vegetables)))
# ... and label them with the respective list entries
ax.set_xticklabels(farmers)
ax.set_yticklabels(vegetables)

# Rotate the tick labels and set their alignment.
plt.setp(ax.get_xticklabels(), rotation=45, ha="right",
         rotation_mode="anchor")

# Loop over data dimensions and create text annotations.
for i in range(len(vegetables)):
    for j in range(len(farmers)):
        text = ax.text(j, i, harvest[i, j],
                       ha="center", va="center", color="w")

ax.set_title("Harvest of local farmers (in tons/year)")
fig.tight_layout()
plt.show()
```

    
<figure class="align-center">
  <img src="/assets/images/df7.png" alt="">
  <figcaption>Confusion Matrix</figcaption>
</figure> 

***

### Spidermap


```python
import matplotlib.pyplot as plt
import numpy as np

vegetables = ["cucumber", "tomato", "lettuce", "asparagus",
              "potato", "wheat", "barley"]
vegetables = [*vegetables, vegetables[0]]


FarmerJoe = [3, 2.4, 2.5, 3.9, 3, 4.0, 3]
UplandBros = [2.4, 3.0, 4.0, 2.0, 2.7, 3.0, 2.4]
SmithG = [1.1, 2.4, 0.8, 4.3, 1.9, 4.4, 1.1]
FarmerJoe = [*FarmerJoe, FarmerJoe[0]]
UplandBros = [*UplandBros, UplandBros[0]]
SmithG = [*SmithG, SmithG[0]]

label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(FarmerJoe))

plt.figure(figsize=(8, 8))
plt.subplot(polar=True)
plt.plot(label_loc, FarmerJoe, label='FarmerJoe')
plt.plot(label_loc, UplandBros, label='UplandBros')
plt.plot(label_loc, SmithG, label='SmithG')
plt.title('Comparison of Output by Farmers', size=20, y=1.05)
lines, labels = plt.thetagrids(np.degrees(label_loc), labels=vegetables)
plt.legend()
plt.show()
```


​    
<img src="/assets/images/df8.png">
​    


## Conclusion


As the saying goes, one never know something until one tries it. Even though there are countless tutorials and other informative resources related to python modules on the web, until one actually runs the code and inspect the meaning behind each line of code, one cannot ascertain whether one really knows what is going on. Also, if one does not make a record of what one learned, he or she will inevitably end up forgetting it. In this post, I decided to create my own personal reference guide for the `pandas` and `numpy` library functions. Even though there are better, more informative guides on the web, I believe that reviewing past resources to re-learn, reuse, or reconfigure code is better than borrowing random code on Stack Overflow that one does not understand. In light of the claim, I should probably remind myself occasionally to organize systematically what I learned about python modules. This is the end of the post, and thank for reading. 

