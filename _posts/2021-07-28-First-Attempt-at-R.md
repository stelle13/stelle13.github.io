In this post, I will go over what I learned during my first attempt at
R. This post was written using R Markdown, via RStudio.



# Motivation

R is a language that I had heard a lot about, but never had a chance to
learn in more detail.



## Lists: Printing Unique Values



``` r
listFruit <- c('Apples', 'Apples', 'Bananas', 'Bananas', 'Pineapples', 'Pineapples', 'Oranges', 'Cucumbers')
print(unique(listFruit))
```

    ## [1] "Apples"     "Bananas"    "Pineapples" "Oranges"    "Cucumbers"

The above prints the unique values in `listFruit`. In this post, I will
go over what I learned during my first attempt at R. This post was
written using R Markdown, via RStudio. In this post, I will go over what
I learned during my first attempt at R. This post was written using R
Markdown, via RStudio. In this post, I will go over what I learned
during my first attempt at R. This post was written using R Markdown,
via RStudio. In this post, I will go over what I learned during my first
attempt at R. This post was written using R Markdown, via RStudio. In
this post, I will go over what I learned during my first attempt at R.
This post was written using R Markdown, via RStudio.



## Lists: Appending Lists

``` r
vector_A <- c("A", "B", "C", "F", "G")
vector_B <- c("D", "E")
vector_A <- append(vector_A, vector_B,3) 
vector_A
```

    ## [1] "A" "B" "C" "D" "E" "F" "G"



## Lists: Reversing Logical Elements

``` r
logical_var <- c(FALSE, TRUE, FALSE, TRUE, TRUE)
logical_var # prints: FALSE, TRUE, FALSE, TRUE, TRUE
```

    ## [1] FALSE  TRUE FALSE  TRUE  TRUE

``` r
!logical_var # prints: T, F, T, F, F
```

    ## [1]  TRUE FALSE  TRUE FALSE FALSE



## Dataframe: Creating a `Dataframe` Object

``` r
id <- c('43', '44', '45', '46')
name <- c('Bush', 'Obama', 'Trump', 'Biden')
age <- c(75,59,75,78)
mStatus <- c(T, T, T, T)

df <- data.frame(id,name,age,mStatus)
df # displays dataframe chart
```

    ##   id  name age mStatus
    ## 1 43  Bush  75    TRUE
    ## 2 44 Obama  59    TRUE
    ## 3 45 Trump  75    TRUE
    ## 4 46 Biden  78    TRUE

``` r
str(df) # displays type / values
```

    ## 'data.frame':    4 obs. of  4 variables:
    ##  $ id     : chr  "43" "44" "45" "46"
    ##  $ name   : chr  "Bush" "Obama" "Trump" "Biden"
    ##  $ age    : num  75 59 75 78
    ##  $ mStatus: logi  TRUE TRUE TRUE TRUE



## Dataframe: Retrieving Values

``` r
df[2,3] # 2nd row, 3rd column, value = 56
```

    ## [1] 59

``` r
df[c(2,3), c(2,4)] # print (row, column) values = (2,2), (2,4), (3,2), (3,4)
```

    ##    name mStatus
    ## 2 Obama    TRUE
    ## 3 Trump    TRUE