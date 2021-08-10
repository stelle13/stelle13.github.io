---
title: "First Attempt at R"
date: 2021-07-28
toc: true
toc_sticky: true
categories:
  - study
tags:
  - R
---

In this post, I will go over what I learned during my first attempt at R.  

# Motivation

R is one of the two main tools that data scientists and statisticians use in their work. Given its prominence in the industry, I had heard about the language countless numbers of times in books and videos alike, but over the past few days, I have finally had the chance to study it in detail. As usual, this post serves to document my progress with what I have studied, in addition to being a  reference guide for my future self. (Also, just as a side note, this post was originally written in R Markdown, but later converted to `.ipynb` and `.md` files using Jupyter.) With that being said, let's get to some R. 

# RStudio vs Jupyter

As with any newcomer to R, I first started with RStudio, studying basic grammar with the widely-known books *R for Data Science* and *Hands-On Programming with R* by Garrett Grolemund and Hadley Wickham, as well as *Data Analysis with R* by Hoon Park. While taking my first steps down the R journey, I first started working on this post using the syntax editor in RStudio--integrating some R code blocks and commentry in a R Markdown file. Then by using a clever trick provided by this [guide](https://bookdown.org/yihui/rmarkdown/markdown-document.html), I converted (or "knitted") the R Markdown file to a `.md` file, and later test-uploaded the file to my Github repository. This worked by adding `output:md_document:variant: markdown_github` to the `.rmd` file's YAML front matter, the style of which I was not familiar with. 

Although RStudio worked fine, after some trial and error, I decided to use Jupyter to finish the rest of this post--which was because I found RStudio to be somewhat slow with downloading packages, as well as my penchant for Jupyter's simple, elegant interface. To give RStudio some credit though, I also decided to come back to it later--that is, if I need to knit `.rmd` files to HTML or Word, publish R files on the web using [rpubs](https://rpubs.com/), or create applications using [shinyapp](https://www.shinyapps.io/). 

# Some Notes on R Grammar 

## Lists: Appending Lists


```R
listFruit <- c('Apples', 'Apples', 'Bananas', 'Bananas', 'Pineapples', 'Pineapples', 'Oranges', 'Cucumbers')
print(unique(listFruit))
```

    [1] "Apples"     "Bananas"    "Pineapples" "Oranges"    "Cucumbers" 


## Lists: Reversing Logical Elements


```R
logical_var <- c(FALSE, TRUE, FALSE, TRUE, TRUE)
logical_var # prints: FALSE, TRUE, FALSE, TRUE, TRUE
!logical_var # prints: T, F, T, F, F
```


<ol class=list-inline>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>TRUE</li>
</ol>




<ol class=list-inline>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>TRUE</li>
	<li>FALSE</li>
	<li>FALSE</li>
</ol>



## Data Frame: Create & Access 

### Create a Data Frame


```R
id <- c('43', '44', '45', '46')
name <- c('Bush', 'Obama', 'Trump', 'Biden')
age <- c(75,59,75,78)
mStatus <- c(T, T, T, T)

df <- data.frame(id,name,age,mStatus)
df # displays dataframe chart
str(df) # displays type / values
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>43   </td><td>Bush </td><td>75   </td><td>TRUE </td></tr>
	<tr><td>44   </td><td>Obama</td><td>59   </td><td>TRUE </td></tr>
	<tr><td>45   </td><td>Trump</td><td>75   </td><td>TRUE </td></tr>
	<tr><td>46   </td><td>Biden</td><td>78   </td><td>TRUE </td></tr>
</tbody>
</table>



    'data.frame':	4 obs. of  4 variables:
     $ id     : Factor w/ 4 levels "43","44","45",..: 1 2 3 4
     $ name   : Factor w/ 4 levels "Biden","Bush",..: 2 3 4 1
     $ age    : num  75 59 75 78
     $ mStatus: logi  TRUE TRUE TRUE TRUE


### Access Data in Rows, Columns


```R
df[2,3] # 2nd row, 3rd column, value = 56
df[c(2,3), c(2,4)] # print (row, column) values = (2,2), (2,4), (3,2), (3,4)
```


59



<table>
<thead><tr><th></th><th scope=col>name</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><th scope=row>2</th><td>Obama</td><td>TRUE </td></tr>
	<tr><th scope=row>3</th><td>Trump</td><td>TRUE </td></tr>
</tbody>
</table>




```R
df$name # access column data using $
df$name[3:4] # column 'name', third, fourth rows
```


<ol class=list-inline>
	<li>Bush</li>
	<li>Obama</li>
	<li>Trump</li>
	<li>Biden</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'Biden'</li>
		<li>'Bush'</li>
		<li>'Obama'</li>
		<li>'Trump'</li>
	</ol>
</details>



<ol class=list-inline>
	<li>Trump</li>
	<li>Biden</li>
</ol>

<details>
	<summary style=display:list-item;cursor:pointer>
		<strong>Levels</strong>:
	</summary>
	<ol class=list-inline>
		<li>'Biden'</li>
		<li>'Bush'</li>
		<li>'Obama'</li>
		<li>'Trump'</li>
	</ol>
</details>


### Structure Display


```R
str(df) # display information, such as #objects, categories, etc
```

    'data.frame':	4 obs. of  4 variables:
     $ id     : Factor w/ 4 levels "43","44","45",..: 1 2 3 4
     $ name   : Factor w/ 4 levels "Biden","Bush",..: 2 3 4 1
     $ age    : num  75 59 75 78
     $ mStatus: logi  TRUE TRUE TRUE TRUE



```R
id <- c('39', '40', '41', '42')
name <- c('Carter', 'Reagan', 'Bush', 'Clinton')
age <- c(96,93,94,74)
mStatus <- c(T, T, T, T)

df1 <- data.frame(id,name,age,mStatus)
df1
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>39     </td><td>Carter </td><td>96     </td><td>TRUE   </td></tr>
	<tr><td>40     </td><td>Reagan </td><td>93     </td><td>TRUE   </td></tr>
	<tr><td>41     </td><td>Bush   </td><td>94     </td><td>TRUE   </td></tr>
	<tr><td>42     </td><td>Clinton</td><td>74     </td><td>TRUE   </td></tr>
</tbody>
</table>



### Combine Data Frames


```R
bothdf <- rbind(df1, df)
bothdf
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>39     </td><td>Carter </td><td>96     </td><td>TRUE   </td></tr>
	<tr><td>40     </td><td>Reagan </td><td>93     </td><td>TRUE   </td></tr>
	<tr><td>41     </td><td>Bush   </td><td>94     </td><td>TRUE   </td></tr>
	<tr><td>42     </td><td>Clinton</td><td>74     </td><td>TRUE   </td></tr>
	<tr><td>43     </td><td>Bush   </td><td>75     </td><td>TRUE   </td></tr>
	<tr><td>44     </td><td>Obama  </td><td>59     </td><td>TRUE   </td></tr>
	<tr><td>45     </td><td>Trump  </td><td>75     </td><td>TRUE   </td></tr>
	<tr><td>46     </td><td>Biden  </td><td>78     </td><td>TRUE   </td></tr>
</tbody>
</table>



### Retrieve: `head`, `tail`, `min`, `max`, `median`, `quantile`


```R
head(bothdf,3)
tail(bothdf, 3)

min(bothdf$age) # min
max(bothdf$age) # max
median(bothdf$age) # median
quantile(bothdf$age) # quartile 

df3 <- bothdf
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>39    </td><td>Carter</td><td>96    </td><td>TRUE  </td></tr>
	<tr><td>40    </td><td>Reagan</td><td>93    </td><td>TRUE  </td></tr>
	<tr><td>41    </td><td>Bush  </td><td>94    </td><td>TRUE  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th></th><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><th scope=row>6</th><td>44   </td><td>Obama</td><td>59   </td><td>TRUE </td></tr>
	<tr><th scope=row>7</th><td>45   </td><td>Trump</td><td>75   </td><td>TRUE </td></tr>
	<tr><th scope=row>8</th><td>46   </td><td>Biden</td><td>78   </td><td>TRUE </td></tr>
</tbody>
</table>




59



96



76.5



<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>59</dd>
	<dt>25%</dt>
		<dd>74.75</dd>
	<dt>50%</dt>
		<dd>76.5</dd>
	<dt>75%</dt>
		<dd>93.25</dd>
	<dt>100%</dt>
		<dd>96</dd>
</dl>



### Retrieve: Sections of the Data Frame


```R
subset(df3, age > 80) # only those with age above 80. 
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>39    </td><td>Carter</td><td>96    </td><td>TRUE  </td></tr>
	<tr><td>40    </td><td>Reagan</td><td>93    </td><td>TRUE  </td></tr>
	<tr><td>41    </td><td>Bush  </td><td>94    </td><td>TRUE  </td></tr>
</tbody>
</table>



### Add: New Column


```R
Nationality <- c("American1", "American2", "American3", "American4")

df3$new_column <- Nationality # adds a new column, "Nationality", with alternating values
df3
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th><th scope=col>new_column</th></tr></thead>
<tbody>
	<tr><td>39       </td><td>Carter   </td><td>96       </td><td>TRUE     </td><td>American1</td></tr>
	<tr><td>40       </td><td>Reagan   </td><td>93       </td><td>TRUE     </td><td>American2</td></tr>
	<tr><td>41       </td><td>Bush     </td><td>94       </td><td>TRUE     </td><td>American3</td></tr>
	<tr><td>42       </td><td>Clinton  </td><td>74       </td><td>TRUE     </td><td>American4</td></tr>
	<tr><td>43       </td><td>Bush     </td><td>75       </td><td>TRUE     </td><td>American1</td></tr>
	<tr><td>44       </td><td>Obama    </td><td>59       </td><td>TRUE     </td><td>American2</td></tr>
	<tr><td>45       </td><td>Trump    </td><td>75       </td><td>TRUE     </td><td>American3</td></tr>
	<tr><td>46       </td><td>Biden    </td><td>78       </td><td>TRUE     </td><td>American4</td></tr>
</tbody>
</table>



### Delete: Columns


```R
new_df3 <- df3[ , -c(3,4)] # creates a copy of the df3 data frame, deletes columns "age", "mStatus"
new_df3
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>new_column</th></tr></thead>
<tbody>
	<tr><td>39       </td><td>Carter   </td><td>American1</td></tr>
	<tr><td>40       </td><td>Reagan   </td><td>American2</td></tr>
	<tr><td>41       </td><td>Bush     </td><td>American3</td></tr>
	<tr><td>42       </td><td>Clinton  </td><td>American4</td></tr>
	<tr><td>43       </td><td>Bush     </td><td>American1</td></tr>
	<tr><td>44       </td><td>Obama    </td><td>American2</td></tr>
	<tr><td>45       </td><td>Trump    </td><td>American3</td></tr>
	<tr><td>46       </td><td>Biden    </td><td>American4</td></tr>
</tbody>
</table>




```R
df3[ , c(5)] <- list(NULL) # delete the column "Nationality" from the original data frame, df3
head(df3)
```


<table>
<thead><tr><th scope=col>id</th><th scope=col>name</th><th scope=col>age</th><th scope=col>mStatus</th></tr></thead>
<tbody>
	<tr><td>39     </td><td>Carter </td><td>96     </td><td>TRUE   </td></tr>
	<tr><td>40     </td><td>Reagan </td><td>93     </td><td>TRUE   </td></tr>
	<tr><td>41     </td><td>Bush   </td><td>94     </td><td>TRUE   </td></tr>
	<tr><td>42     </td><td>Clinton</td><td>74     </td><td>TRUE   </td></tr>
	<tr><td>43     </td><td>Bush   </td><td>75     </td><td>TRUE   </td></tr>
	<tr><td>44     </td><td>Obama  </td><td>59     </td><td>TRUE   </td></tr>
</tbody>
</table>



### Change: Column Names


```R
colnames(df3)
```


<ol class=list-inline>
	<li>'id'</li>
	<li>'name'</li>
	<li>'age'</li>
	<li>'mStatus'</li>
</ol>




```R
colnames(df3) <- c("C1", "C2", "C3", "C4")
head(df3)

colnames(df3) <- c("ID", "Name", "Age", "Marital Status")
head(df3)
```


<table>
<thead><tr><th scope=col>C1</th><th scope=col>C2</th><th scope=col>C3</th><th scope=col>C4</th></tr></thead>
<tbody>
	<tr><td>39     </td><td>Carter </td><td>96     </td><td>TRUE   </td></tr>
	<tr><td>40     </td><td>Reagan </td><td>93     </td><td>TRUE   </td></tr>
	<tr><td>41     </td><td>Bush   </td><td>94     </td><td>TRUE   </td></tr>
	<tr><td>42     </td><td>Clinton</td><td>74     </td><td>TRUE   </td></tr>
	<tr><td>43     </td><td>Bush   </td><td>75     </td><td>TRUE   </td></tr>
	<tr><td>44     </td><td>Obama  </td><td>59     </td><td>TRUE   </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>ID</th><th scope=col>Name</th><th scope=col>Age</th><th scope=col>Marital Status</th></tr></thead>
<tbody>
	<tr><td>39     </td><td>Carter </td><td>96     </td><td>TRUE   </td></tr>
	<tr><td>40     </td><td>Reagan </td><td>93     </td><td>TRUE   </td></tr>
	<tr><td>41     </td><td>Bush   </td><td>94     </td><td>TRUE   </td></tr>
	<tr><td>42     </td><td>Clinton</td><td>74     </td><td>TRUE   </td></tr>
	<tr><td>43     </td><td>Bush   </td><td>75     </td><td>TRUE   </td></tr>
	<tr><td>44     </td><td>Obama  </td><td>59     </td><td>TRUE   </td></tr>
</tbody>
</table>



The rest of this post dealing with visualization with R has been relocated to the next post, due to the length of this document. 
