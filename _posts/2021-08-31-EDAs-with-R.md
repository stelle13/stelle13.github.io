---
title: "EDA with R"
date: 2021-08-31
categories:
  - study
tags:
  - R
---



In this post, I will go over the exploratory data analysis part of the R book that I am studying--*R for Data Science* by Hadley Wickham and Garrett Grolemund. 

## Exploratory Data Analysis

EDA, although not strictly defined, consists of three parts:

* Visualization
* Transformation
* Modeling


In this post, a toy dataset called `diamonds` supplied by `tidyverse` will be used for EDA. The diamonds dataset contains information on approximately 54,000 diamonds, such as the quality of the cut and the price. 

To use the dataset, first load `tidyverse` and `diamonds`. 


```R
library(tidyverse)
```

    Registered S3 methods overwritten by 'ggplot2':
      method         from 
      [.quosures     rlang
      c.quosures     rlang
      print.quosures rlang
    Registered S3 method overwritten by 'rvest':
      method            from
      read_xml.response xml2
    -- Attaching packages --------------------------------------- tidyverse 1.2.1 --
    √ ggplot2 3.1.1       √ purrr   0.3.2  
    √ tibble  2.1.1       √ dplyr   0.8.0.1
    √ tidyr   0.8.3       √ stringr 1.4.0  
    √ readr   1.3.1       √ forcats 0.4.0  
    -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    x dplyr::filter() masks stats::filter()
    x dplyr::lag()    masks stats::lag()


Since the `cut` variable is categorical, use a bar chart (`geom_bar`) to visualize the diamonds' distribution. 

* Variables:

    * Categorical:
        * cut
        * color
        * clarity
    * Continuous:
        * price
        * carat
        * length, width, depth (x,y,z)




```R
ggplot(data = diamonds) +
    theme(plot.title = element_text(hjust = 0.5)) +
    ggtitle("Distribution based on Cut") + 
    geom_bar(mapping = aes(x = cut, fill = clarity)) # use `fill` to see cross-section for categorical variable
```


​    
![png](EDAs_files/EDAs_6_0.png)
​    



```R
diamonds %>%
    count(cut) # count() shows the number of diamonds in each category
```


<table>
<thead><tr><th scope=col>cut</th><th scope=col>n</th></tr></thead>
<tbody>
	<tr><td>Fair     </td><td> 1610    </td></tr>
	<tr><td>Good     </td><td> 4906    </td></tr>
	<tr><td>Very Good</td><td>12082    </td></tr>
	<tr><td>Premium  </td><td>13791    </td></tr>
	<tr><td>Ideal    </td><td>21551    </td></tr>
</tbody>
</table>



Meanwhile, since `carat` is a continuous variable, use a histogram (`geom_histogram`) to view the distribution. 


```R
ggplot(data = diamonds) +
    theme(plot.title = element_text(hjust = 0.5)) +
    ggtitle("Distribution based on Carat") + 
    geom_histogram(mapping = aes(x = carat, fill = color), binwidth = 0.5)
```


​    
![png](EDAs_files/EDAs_9_0.png)
​    



```R
diamonds %>%
    count(cut_width(carat, 0.5))
```


<table>
<thead><tr><th scope=col>cut_width(carat, 0.5)</th><th scope=col>n</th></tr></thead>
<tbody>
	<tr><td>[-0.25,0.25]</td><td>  785       </td></tr>
	<tr><td>(0.25,0.75] </td><td>29498       </td></tr>
	<tr><td>(0.75,1.25] </td><td>15977       </td></tr>
	<tr><td>(1.25,1.75] </td><td> 5313       </td></tr>
	<tr><td>(1.75,2.25] </td><td> 2002       </td></tr>
	<tr><td>(2.25,2.75] </td><td>  322       </td></tr>
	<tr><td>(2.75,3.25] </td><td>   32       </td></tr>
	<tr><td>(3.25,3.75] </td><td>    5       </td></tr>
	<tr><td>(3.75,4.25] </td><td>    4       </td></tr>
	<tr><td>(4.25,4.75] </td><td>    1       </td></tr>
	<tr><td>(4.75,5.25] </td><td>    1       </td></tr>
</tbody>
</table>



To view a smaller subset of the data satisfying a certain condition or several conditions, use `filter()`. 


```R
smaller <- diamonds %>%
    filter(carat < 2 && color == "E")

ggplot(data = smaller, mapping = aes(x = carat, color = clarity)) +
    geom_histogram(binwidth = 0.1)
```


​    
![png](EDAs_files/EDAs_12_0.png)
​    


To view multiple distributions over a single variable, one must put multiple histograms over one another, which would obviously be hard to visualize. This is where `geom_freqpoly` comes in--by replacing histograms with lines, visualization is made much easier. 


```R
library(tidyverse)

ggplot(data = smaller, mapping = aes(x = carat, color = cut)) +
    theme(plot.title = element_text(hjust = 0.5)) +
    ggtitle("Distribution of Cut per Carat") + 
    geom_freqpoly(binwidth = 0.1)
```


​    
![png](EDAs_files/EDAs_14_0.png)
​    


## Replacing Values

When preprocessing data, there will almost always be *strange* data--data whose values do not make any sense. When convinced enough that those pieces of data are invalid, two options remain: one, simply drop the rows containing the strange data, or two, replace those values with `NA` (not available). The book recommends that the second option, for the reason that in some cases, there may not be enough data to simply delete those rows. 


```R
tibbleW <- tribble(
    ~w, ~x, ~y, ~z, 
    "a", 2, 1, "q",
    "b", 3, 100, "w",
    "c", 4, 1, "x",
    "d", 5, 110, "y",
) 

head(tibbleW)
```


<table>
<thead><tr><th scope=col>w</th><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr></thead>
<tbody>
	<tr><td>a  </td><td>2  </td><td>  1</td><td>q  </td></tr>
	<tr><td>b  </td><td>3  </td><td>100</td><td>w  </td></tr>
	<tr><td>c  </td><td>4  </td><td>  1</td><td>x  </td></tr>
	<tr><td>d  </td><td>5  </td><td>110</td><td>y  </td></tr>
</tbody>
</table>




```R
tibbleW1 <- tibbleW %>%
    mutate(y = ifelse(y < 10, NA, y)) # look to the `y` column, replace any value in the y column less than 10 with "NA"

head(tibbleW1)
```


<table>
<thead><tr><th scope=col>w</th><th scope=col>x</th><th scope=col>y</th><th scope=col>z</th></tr></thead>
<tbody>
	<tr><td>a  </td><td>2  </td><td> NA</td><td>q  </td></tr>
	<tr><td>b  </td><td>3  </td><td>100</td><td>w  </td></tr>
	<tr><td>c  </td><td>4  </td><td> NA</td><td>x  </td></tr>
	<tr><td>d  </td><td>5  </td><td>110</td><td>y  </td></tr>
</tbody>
</table>



## Covariation 

To visualize the relationship between two variables--one categorical and the other continuous--use `geom_freqpoly()`to graph the distribution of observations based on the continuous variable, for each categorical variable. 

The following example shows the diamonds' distribution of price (continuous), categorized by the cut quality (categorical). 



```R
 ggplot(data = diamonds) +
    theme(plot.title = element_text(hjust = 0.5)) +
    ggtitle("Price Distribution per Cut") + 
    geom_freqpoly(mapping = aes(x = price, color = cut), binwidth = 500)
```


​    
![png](EDAs_files/EDAs_19_0.png)
​    


In determining the relationship between price and quality, however, one must observe the fact that there are a different number of diamonds per cut category. 



```R
ggplot(diamonds) +
    geom_bar(mapping = aes(x = cut))
```


​    
![png](EDAs_files/EDAs_21_0.png)
​    


In the above `geom_freqpoly` graph, the y axis is set to be "count", while the x axis is "price". The most visibly striking aspect of the graph is the tall yellow peak at about 1,200 dollars for price. This yellow peak is taller than the other distributions' peaks at about the same price--far taller, in fact. This gives the illusion that the *percentage* of diamonds that occupy that 1,200 peak in price is also far greater for the `ideal` category than it is for the `good` category. 

This assumption is invalid though, and for two reasons. First, the y axis is "count," not "percentage" or "density," and second, there are a lot more diamonds in the `ideal` category than the `good` category. In short, one cannot compare the difference in *distribution* by comparing the difference in *count*.

Another problem with the graph is that one cannot easily compare the mean diamond price of each cut quality. The graph may deceive people into believing that the 1,200 dollar yellow peak implies a mean price closer to 1,200 dollars for ideal-cut diamonds than other mean prices. However, this is not necessarily true, given the difference in the area under each graph corresponding to the diamond count for each cut quality. 


```R
ggplot(
    data = diamonds,
        mapping = aes(x = price, y = ..density..) # notice the argument "y = ..density"
    ) +

    theme(plot.title = element_text(hjust = 0.5)) +
    ggtitle("Price Density Distribution per Cut") +

    geom_freqpoly(mapping = aes(color = cut), binwidth = 500)
```


​    
![png](EDAs_files/EDAs_23_0.png)
​    


The graph above, in which the area under each line is the same, suggests that fair-cut diamonds have an average price closer to 2,500 dollars, rather than 1,200 dollars. In fact, it appears as though fair-cut diamonds have the highest mean price among the diamonds of each cut quality.   


```R
s1 <- subset(diamonds, cut=='Fair')
# head(s1[7])
s1[7] %>%
   summarize(mean = mean(price)) # average price of fairly cut diamonds = $4358

idealD1 <- diamonds %>%
    filter(cut == "Good") %>%
    summarize(mean = mean(price))
idealD1 # average price of good cut diamonds = $3457 

idealD3 <- diamonds %>%
    filter(cut == "Premium") %>%
    summarize(mean = mean(price))
idealD3 # average price of premium cut diamonds = $3457 

idealD4 <- diamonds %>%
    filter(cut == "Ideal") %>%
    summarize(mean = mean(price))
idealD4 # average price of ideally cut diamonds = $3457 

```


<table>
<thead><tr><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>4358.758</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>3928.864</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>4584.258</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>3457.542</td></tr>
</tbody>
</table>




```R
ggplot(data = diamonds, mapping = aes(x = cut, y = price)) +
    geom_boxplot()
```


​    
![png](EDAs_files/EDAs_26_0.png)
​    


To order the boxplots in increasing median values, add a `x = reorder()` argument to `mapping = aes()`. 


```R
# Ordered by increasing median values 

ggplot(data = diamonds) +
    geom_boxplot(mapping = aes(
                    x = reorder(cut, price, FUN = median),  
                    y = price))
```


​    
![png](EDAs_files/EDAs_28_0.png)
​    


To get a more visually striking picture of the distribution of diamonds in terms of cut and color, use a heatmap-like `geom` object called "geom_tile."


```R
diamonds %>%
    count(color, cut) %>%
    ggplot(mapping = aes(x = color, y = cut)) + 
        geom_tile(mapping = aes(fill = n)) 

```


​    
![png](EDAs_files/EDAs_30_0.png)
​    


From the `geom_tile` diagram above, it appears as though ideal-cut diamonds with color G are the majority, followed by the diamonds in the adjacent cut categories, and finally the fair-cut diamonds. 