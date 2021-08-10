In this post, I will continue on from the previous post on learning R, but will focus primarily on data visualization. As stated before, the contents of this blog post heavily rely on Hadley Wickham and Garrett Grolemund's book, *R for Data Science*. 

# Basic Graphs

## Boxplot


```R
boxN <- c(10, 25, 30, 45, 55, 68, 80, 90, 100)

quantile(boxN) # shows min, 1st, 2nd, 3rd Quartiles, max
boxplot(boxN)
```


10



100



55



<dl class=dl-horizontal>
	<dt>0%</dt>
		<dd>10</dd>
	<dt>25%</dt>
		<dd>30</dd>
	<dt>50%</dt>
		<dd>55</dd>
	<dt>75%</dt>
		<dd>80</dd>
	<dt>100%</dt>
		<dd>100</dd>
</dl>





![png](2R_files/2R_3_4.png)
    



```R
seq1 <- c(25, 77, 52, 89, 90, 55, 34, 9, 72, 86, 56, 24, 59, 23, 14, 21, 12, 13, 73)
seq2 <- c(45,  64,  20,  43,  44, 85,  51,  62,  98,  74,  88,  96,  94,  36,  65,  97,  82,  50,  30,  99, 37)

boxplot(seq1, seq2, names=c("Seq A", "Seq B"))
```


​    
![png](2R_files/2R_4_0.png)
​    


## Histogram


```R
hist(seq2, main="List of Random Numbers, 1 to 100", xlab="Intervals", ylab="Frequency")

message("Variance: ", var(seq2))
 
message("Standard Deviation: ", sd(seq2))
```

    Variance: 649.990476190476
    Standard Deviation25.4949107900082




![png](2R_files/2R_6_1.png)
    


## Pie Chart


```R
Grade <- c('A', 'B', 'C', 'D', 'B', 'A', 'D', 'F', 'A', 'B', 'C', 'D', 'B', 'A', 'D', 'F', 'A','B', 'C', 'D', 'B', 'A', 'D', 'F', 'A')
length(Grade)

tableG <- table(Grade)
pie(x=table(Grade))
```


25




![png](2R_files/2R_8_1.png)
    



```R
pie(x=table(Grade), col=c("cyan", "lightcyan", "blue", "skyblue", "cyan"), main="Pie Chart of Grades")
pie(x=table(Grade), col=heat.colors(5))
```


​    
![png](2R_files/2R_9_0.png)
​    




![png](2R_files/2R_9_1.png)
    


## Barplot


```R
barplot(table(Grade)
       , names.arg=c("A", "B", "C", "D", "F")
       , main = "Distribution of Grades"
       , xlab = "Grade"
       , ylab = "Number of People"
       , col = heat.colors(5))
```


​    
![png](2R_files/2R_11_0.png)
​    


# R4DS

## Installing R Packages: `tidyverse`

To install the `tidyverse` package ecosystem, simply type: `install.packages("tidyverse")`. To reload the `tidyverse` ecosystem into the notebook, add the following codeblock: `library(tidyverse)`. 

## Graphing with `ggplot()`


```R
head(mpg,7) # mpg is the dataset containing info on thirty-eight cars, provided by U.S.E.P.A. 
```


<table>
<thead><tr><th scope=col>manufacturer</th><th scope=col>model</th><th scope=col>displ</th><th scope=col>year</th><th scope=col>cyl</th><th scope=col>trans</th><th scope=col>drv</th><th scope=col>cty</th><th scope=col>hwy</th><th scope=col>fl</th><th scope=col>class</th></tr></thead>
<tbody>
	<tr><td>audi      </td><td>a4        </td><td>1.8       </td><td>1999      </td><td>4         </td><td>auto(l5)  </td><td>f         </td><td>18        </td><td>29        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>1.8       </td><td>1999      </td><td>4         </td><td>manual(m5)</td><td>f         </td><td>21        </td><td>29        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.0       </td><td>2008      </td><td>4         </td><td>manual(m6)</td><td>f         </td><td>20        </td><td>31        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.0       </td><td>2008      </td><td>4         </td><td>auto(av)  </td><td>f         </td><td>21        </td><td>30        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.8       </td><td>1999      </td><td>6         </td><td>auto(l5)  </td><td>f         </td><td>16        </td><td>26        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>2.8       </td><td>1999      </td><td>6         </td><td>manual(m5)</td><td>f         </td><td>18        </td><td>26        </td><td>p         </td><td>compact   </td></tr>
	<tr><td>audi      </td><td>a4        </td><td>3.1       </td><td>2008      </td><td>6         </td><td>auto(av)  </td><td>f         </td><td>18        </td><td>27        </td><td>p         </td><td>compact   </td></tr>
</tbody>
</table>



The general form of the ggplot function is as follows: `ggplot(data = <DATA>) + <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))`. The `geom_function` specifies the "geom" of the plot, or the type of geometrical object the plot will use. Here are a few examples of the `geom_function`s:

Plot with...

- Points: `geom_point()` 
- Lines: `geom_smooth()`
- Bars: `geom_bar()`
- Histograms: `geom_histogram()`
- Dotplot: `geom_dotplot()`

## Scatterplot


```R
ggplot(data = mpg) + # ggplot(data = mpg) creates an empty graph 
  geom_point(mapping = aes(x = displ, y = hwy)) # function geom_point() takes the argument "mapping": displ is mapped to x-axis, hwy is mapped to y. 
```


​    
![png](2R_files/2R_19_0.png)
​    


Mapping a VARIABLE (class) to an AESTHETIC (color parameter) shows the cars' classes. 

### Scatterplot With Different Colors


```R
ggplot(data = mpg) +
    geom_point(mapping = aes(x = displ, y = hwy, color = class)) 

# "displ" : Displacement of the cars' engines
# "hwy" : Fuel consumption efficiency

```


​    
![png](2R_files/2R_22_0.png)
​    


### Scatterplot With Different Shapes


```R
# mapping a VARIABLE (class) to another AESTHETIC (shape) shows the cars' classes. 
# mapping seven categorical variables (car types) to shape produces WARNING, 
# since having more than six variables can make discrimination between points difficult.   

ggplot(data = mpg) +
    geom_point(mapping = aes(x = displ, y = hwy, shape = class)) 

```

    Warning message:
    "The shape palette can deal with a maximum of 6 discrete values because
    more than 6 becomes difficult to discriminate; you have 7. Consider
    specifying shapes manually if you must have them."Warning message:
    "Removed 62 rows containing missing values (geom_point)."


​    
![png](2R_files/2R_24_1.png)
​    


### Scatterplot With Uniform Color


```R
# Manually adjust the color aesthetic by specifying the argument outside of aes()
ggplot(data = mpg) +
    geom_point(mapping = aes(x = displ, y = hwy), color = "red") 
```


​    
![png](2R_files/2R_26_0.png)
​    


### Subplots for Scatterplot


```R
# splitting the plot above into seven subplots based on the cars' class. 
ggplot(data=mpg) +
    geom_point(mapping = aes(x = displ, y = hwy)) +
    facet_wrap(~ class, nrow = 2)
```


​    
![png](2R_files/2R_28_0.png)
​    


## Regression Lines


```R
ggplot(data = mpg) + 
    geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv))
    # the gray area are the confidence bands around each nonlinear regression line. 
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'




![png](2R_files/2R_30_1.png)
    


## Regression Lines + Scatterplot


```R
library(tidyverse) # load the tidyverse collection of packages

ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + # Use global mapping 
    geom_point(mapping = aes(color = class)) + # add another mapping argument using color = class, differentiates class by color 
    geom_smooth( # smooth line
        data = filter(mpg, class == "subcompact"), # "filter," or change/override the global mapping up top, 
                                                   # smooth regression line over "subcompact" class dots.
        se = TRUE                                  # "se" specifies condition for confidence bands. se = FALSE removes the confidence bands
    )
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'




![png](2R_files/2R_32_1.png)
    


## Barplot


```R
library(tidyverse)

ggplot(data = diamonds) +
    geom_bar(mapping = aes(x = cut))
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
    v ggplot2 3.1.1       v purrr   0.3.2  
    v tibble  2.1.1       v dplyr   0.8.0.1
    v tidyr   0.8.3       v stringr 1.4.0  
    v readr   1.3.1       v forcats 0.4.0  
    -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    x dplyr::filter() masks stats::filter()
    x dplyr::lag()    masks stats::lag()




![png](2R_files/2R_34_1.png)
    


The function `ggplot(data = diamonds)` loads the diamond dataset into `ggplot()`, which consists of statistical information of about 54,000 diamonds. The function `geom_bar()` specifies that the chart will be a bar chart, with the x-axis assigned to "cut"--one of the many statistical parameters of the diamond dataset. 


```R
ggplot(data = diamonds) +
    geom_bar(mapping = aes(x = cut, fill = cut))
```


​    
![png](2R_files/2R_36_0.png)
​    


The following chart separates each bar into distinct sections, depending on the clarity--another category of the diamonds dataset. 


```R
ggplot(data = diamonds) +
    geom_bar(mapping = aes(x = cut, fill = clarity))
```


​    
![png](2R_files/2R_38_0.png)
​    


To more easily compare the proportions of diamonds within each bar, one needs to set the bars to the same height by assigning proportion to the y-axis. This is done by specifying the `position` parameter to `fill`, outside the `mapping = aes()` argument.    


```R
ggplot(data = diamonds) +
    geom_bar(
        mapping = aes(x = cut, fill = clarity),
        position = "fill"
    )
```


​    
![png](2R_files/2R_40_0.png)
​    


Notice that the bars are at the same height, and that the y-axis stands for proportion. If comparing proportions within a single bar is visually difficult, one can separate the bar into many bars with respect to the clarity. 


```R
ggplot(data = diamonds) + 
    geom_bar(
        mapping = aes(x=cut, fill=clarity),
        position = "dodge"
    )
```


​    
![png](2R_files/2R_42_0.png)
​    


## Maps: United States, U.K, East Asia

### United States


```R
nz <- map_data("state")

ggplot(nz, aes(long, lat, group = group)) +
    geom_polygon(fill = "white", color = "black") +
    coord_quickmap()
```


​    
![png](2R_files/2R_45_0.png)
​    


### United Kingdom


```R
uk <- map_data("world", region = c("UK"))

ggplot(uk, aes(long, lat, group = group)) +
    geom_polygon(fill = "white", color = "black") +
    coord_quickmap()
```


​    
![png](2R_files/2R_47_0.png)
​    


### East Asia


```R
korea <- map_data("world", region = c("North Korea", "Japan", "South Korea"))

ggplot(korea, aes(long, lat, group = group)) +
    geom_polygon(fill = "white", color = "black") +
    coord_quickmap()
```


​    
![png](2R_files/2R_49_0.png)
​    


### Coxcomb Chart


```R
bar <- ggplot(data = diamonds) +
  geom_bar(
    mapping = aes(x = cut, fill = cut),
    show.legend = FALSE,
    width = 1
  ) +
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)

bar + coord_flip()
bar + coord_polar()
```


​    
![png](2R_files/2R_51_0.png)
​    




![png](2R_files/2R_51_1.png)
    

