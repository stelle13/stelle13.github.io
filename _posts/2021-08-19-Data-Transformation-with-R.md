---
title: "Data Transformation with R"
date: 2021-08-19
toc: true
toc_sticky: true
categories:
  - study
tags:
  - R
---

This post is a continuation of my recent blog posts on learning R. This post, along with the others, references Hadley Wickham and Garret Grolemund's *R for Data Science*, which authorizes small scale, non-profit documentations of the example code from the book. 

## Dplyr Functions: `filter()`, `arrange()`, `mutate()`, `select()`, `summarize()`

### Dataset: `nycflights13`


```R
library(tidyverse)
# install.packages("nycflights13")
library(nycflights13)
```


```R
head(flights) # flights departing NYC in 2013
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>517                </td><td>515                </td><td> 2                 </td><td> 830               </td><td> 819               </td><td> 11                </td><td>UA                 </td><td>1545               </td><td>N14228             </td><td>EWR                </td><td>IAH                </td><td>227                </td><td>1400               </td><td>5                  </td><td>15                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>533                </td><td>529                </td><td> 4                 </td><td> 850               </td><td> 830               </td><td> 20                </td><td>UA                 </td><td>1714               </td><td>N24211             </td><td>LGA                </td><td>IAH                </td><td>227                </td><td>1416               </td><td>5                  </td><td>29                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>542                </td><td>540                </td><td> 2                 </td><td> 923               </td><td> 850               </td><td> 33                </td><td>AA                 </td><td>1141               </td><td>N619AA             </td><td>JFK                </td><td>MIA                </td><td>160                </td><td>1089               </td><td>5                  </td><td>40                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>544                </td><td>545                </td><td>-1                 </td><td>1004               </td><td>1022               </td><td>-18                </td><td>B6                 </td><td> 725               </td><td>N804JB             </td><td>JFK                </td><td>BQN                </td><td>183                </td><td>1576               </td><td>5                  </td><td>45                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>600                </td><td>-6                 </td><td> 812               </td><td> 837               </td><td>-25                </td><td>DL                 </td><td> 461               </td><td>N668DN             </td><td>LGA                </td><td>ATL                </td><td>116                </td><td> 762               </td><td>6                  </td><td> 0                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>558                </td><td>-4                 </td><td> 740               </td><td> 728               </td><td> 12                </td><td>UA                 </td><td>1696               </td><td>N39463             </td><td>EWR                </td><td>ORD                </td><td>150                </td><td> 719               </td><td>5                  </td><td>58                 </td><td>2013-01-01 05:00:00</td></tr>
</tbody>
</table>



### `filter()`: Filter Rows 


```R
head(filter(flights, arr_delay > 120)) # flight arrivals delayed by more than two hours(120min)
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td> 811               </td><td> 630               </td><td>101                </td><td>1047               </td><td> 830               </td><td>137                </td><td>MQ                 </td><td>4576               </td><td>N531MQ             </td><td>LGA                </td><td>CLT                </td><td>118                </td><td> 544               </td><td> 6                 </td><td>30                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td> 848               </td><td>1835               </td><td>853                </td><td>1001               </td><td>1950               </td><td>851                </td><td>MQ                 </td><td>3944               </td><td>N942MQ             </td><td>JFK                </td><td>BWI                </td><td> 41                </td><td> 184               </td><td>18                 </td><td>35                 </td><td>2013-01-01 18:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td> 957               </td><td> 733               </td><td>144                </td><td>1056               </td><td> 853               </td><td>123                </td><td>UA                 </td><td> 856               </td><td>N534UA             </td><td>EWR                </td><td>BOS                </td><td> 37                </td><td> 200               </td><td> 7                 </td><td>33                 </td><td>2013-01-01 07:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>1114               </td><td> 900               </td><td>134                </td><td>1447               </td><td>1222               </td><td>145                </td><td>UA                 </td><td>1086               </td><td>N76502             </td><td>LGA                </td><td>IAH                </td><td>248                </td><td>1416               </td><td> 9                 </td><td> 0                 </td><td>2013-01-01 09:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>1505               </td><td>1310               </td><td>115                </td><td>1638               </td><td>1431               </td><td>127                </td><td>EV                 </td><td>4497               </td><td>N17984             </td><td>EWR                </td><td>RIC                </td><td> 63                </td><td> 277               </td><td>13                 </td><td>10                 </td><td>2013-01-01 13:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>1525               </td><td>1340               </td><td>105                </td><td>1831               </td><td>1626               </td><td>125                </td><td>B6                 </td><td> 525               </td><td>N231JB             </td><td>EWR                </td><td>MCO                </td><td>152                </td><td> 937               </td><td>13                 </td><td>40                 </td><td>2013-01-01 13:00:00</td></tr>
</tbody>
</table>




```R
head(filter(flights, month==12, day==25))
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>456                </td><td>500                </td><td>-4                 </td><td> 649               </td><td> 651               </td><td> -2                </td><td>US                 </td><td>1895               </td><td>N156UW             </td><td>EWR                </td><td>CLT                </td><td> 98                </td><td> 529               </td><td>5                  </td><td> 0                 </td><td>2013-12-25 05:00:00</td></tr>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>524                </td><td>515                </td><td> 9                 </td><td> 805               </td><td> 814               </td><td> -9                </td><td>UA                 </td><td>1016               </td><td>N32404             </td><td>EWR                </td><td>IAH                </td><td>203                </td><td>1400               </td><td>5                  </td><td>15                 </td><td>2013-12-25 05:00:00</td></tr>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>542                </td><td>540                </td><td> 2                 </td><td> 832               </td><td> 850               </td><td>-18                </td><td>AA                 </td><td>2243               </td><td>N5EBAA             </td><td>JFK                </td><td>MIA                </td><td>146                </td><td>1089               </td><td>5                  </td><td>40                 </td><td>2013-12-25 05:00:00</td></tr>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>546                </td><td>550                </td><td>-4                 </td><td>1022               </td><td>1027               </td><td> -5                </td><td>B6                 </td><td> 939               </td><td>N665JB             </td><td>JFK                </td><td>BQN                </td><td>191                </td><td>1576               </td><td>5                  </td><td>50                 </td><td>2013-12-25 05:00:00</td></tr>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>556                </td><td>600                </td><td>-4                 </td><td> 730               </td><td> 745               </td><td>-15                </td><td>AA                 </td><td> 301               </td><td>N3JLAA             </td><td>LGA                </td><td>ORD                </td><td>123                </td><td> 733               </td><td>6                  </td><td> 0                 </td><td>2013-12-25 06:00:00</td></tr>
	<tr><td>2013               </td><td>12                 </td><td>25                 </td><td>557                </td><td>600                </td><td>-3                 </td><td> 743               </td><td> 752               </td><td> -9                </td><td>DL                 </td><td> 731               </td><td>N369NB             </td><td>LGA                </td><td>DTW                </td><td> 88                </td><td> 502               </td><td>6                  </td><td> 0                 </td><td>2013-12-25 06:00:00</td></tr>
</tbody>
</table>



### `arrange()`: Change the Order of Rows



```R
df <- tibble(x = c(5,4,3,2,1, NA))
arrange(df, x) # arrange by ascending order
arrange(df, desc(x))
```


<table>
<thead><tr><th scope=col>x</th></tr></thead>
<tbody>
	<tr><td> 1</td></tr>
	<tr><td> 2</td></tr>
	<tr><td> 3</td></tr>
	<tr><td> 4</td></tr>
	<tr><td> 5</td></tr>
	<tr><td>NA</td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>x</th></tr></thead>
<tbody>
	<tr><td> 5</td></tr>
	<tr><td> 4</td></tr>
	<tr><td> 3</td></tr>
	<tr><td> 2</td></tr>
	<tr><td> 1</td></tr>
	<tr><td>NA</td></tr>
</tbody>
</table>




```R
head(arrange(flights, year, month, day)) # arranges the rows by ASCENDING ORDER with respect to the year, month, and day 
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>517                </td><td>515                </td><td> 2                 </td><td> 830               </td><td> 819               </td><td> 11                </td><td>UA                 </td><td>1545               </td><td>N14228             </td><td>EWR                </td><td>IAH                </td><td>227                </td><td>1400               </td><td>5                  </td><td>15                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>533                </td><td>529                </td><td> 4                 </td><td> 850               </td><td> 830               </td><td> 20                </td><td>UA                 </td><td>1714               </td><td>N24211             </td><td>LGA                </td><td>IAH                </td><td>227                </td><td>1416               </td><td>5                  </td><td>29                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>542                </td><td>540                </td><td> 2                 </td><td> 923               </td><td> 850               </td><td> 33                </td><td>AA                 </td><td>1141               </td><td>N619AA             </td><td>JFK                </td><td>MIA                </td><td>160                </td><td>1089               </td><td>5                  </td><td>40                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>544                </td><td>545                </td><td>-1                 </td><td>1004               </td><td>1022               </td><td>-18                </td><td>B6                 </td><td> 725               </td><td>N804JB             </td><td>JFK                </td><td>BQN                </td><td>183                </td><td>1576               </td><td>5                  </td><td>45                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>600                </td><td>-6                 </td><td> 812               </td><td> 837               </td><td>-25                </td><td>DL                 </td><td> 461               </td><td>N668DN             </td><td>LGA                </td><td>ATL                </td><td>116                </td><td> 762               </td><td>6                  </td><td> 0                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>558                </td><td>-4                 </td><td> 740               </td><td> 728               </td><td> 12                </td><td>UA                 </td><td>1696               </td><td>N39463             </td><td>EWR                </td><td>ORD                </td><td>150                </td><td> 719               </td><td>5                  </td><td>58                 </td><td>2013-01-01 05:00:00</td></tr>
</tbody>
</table>




```R
head(arrange(flights, desc(year, month, day)))
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>517                </td><td>515                </td><td> 2                 </td><td> 830               </td><td> 819               </td><td> 11                </td><td>UA                 </td><td>1545               </td><td>N14228             </td><td>EWR                </td><td>IAH                </td><td>227                </td><td>1400               </td><td>5                  </td><td>15                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>533                </td><td>529                </td><td> 4                 </td><td> 850               </td><td> 830               </td><td> 20                </td><td>UA                 </td><td>1714               </td><td>N24211             </td><td>LGA                </td><td>IAH                </td><td>227                </td><td>1416               </td><td>5                  </td><td>29                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>542                </td><td>540                </td><td> 2                 </td><td> 923               </td><td> 850               </td><td> 33                </td><td>AA                 </td><td>1141               </td><td>N619AA             </td><td>JFK                </td><td>MIA                </td><td>160                </td><td>1089               </td><td>5                  </td><td>40                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>544                </td><td>545                </td><td>-1                 </td><td>1004               </td><td>1022               </td><td>-18                </td><td>B6                 </td><td> 725               </td><td>N804JB             </td><td>JFK                </td><td>BQN                </td><td>183                </td><td>1576               </td><td>5                  </td><td>45                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>600                </td><td>-6                 </td><td> 812               </td><td> 837               </td><td>-25                </td><td>DL                 </td><td> 461               </td><td>N668DN             </td><td>LGA                </td><td>ATL                </td><td>116                </td><td> 762               </td><td>6                  </td><td> 0                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>558                </td><td>-4                 </td><td> 740               </td><td> 728               </td><td> 12                </td><td>UA                 </td><td>1696               </td><td>N39463             </td><td>EWR                </td><td>ORD                </td><td>150                </td><td> 719               </td><td>5                  </td><td>58                 </td><td>2013-01-01 05:00:00</td></tr>
</tbody>
</table>



### `select()`: Select specific columns 


```R
head(select(flights, arr_time, arr_delay, flight, tailnum)) # select specific columns: arr_time, arr_delay, flight, tailnum
```


<table>
<thead><tr><th scope=col>arr_time</th><th scope=col>arr_delay</th><th scope=col>flight</th><th scope=col>tailnum</th></tr></thead>
<tbody>
	<tr><td> 830  </td><td> 11   </td><td>1545  </td><td>N14228</td></tr>
	<tr><td> 850  </td><td> 20   </td><td>1714  </td><td>N24211</td></tr>
	<tr><td> 923  </td><td> 33   </td><td>1141  </td><td>N619AA</td></tr>
	<tr><td>1004  </td><td>-18   </td><td> 725  </td><td>N804JB</td></tr>
	<tr><td> 812  </td><td>-25   </td><td> 461  </td><td>N668DN</td></tr>
	<tr><td> 740  </td><td> 12   </td><td>1696  </td><td>N39463</td></tr>
</tbody>
</table>




```R
head(select(flights, year:day)) # To view the end of the data, run: `tail(select(flights, year:day))`
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th></tr></thead>
<tbody>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td></tr>
</tbody>
</table>




```R
head(select(flights, -(dep_delay:time_hour)))
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th></tr></thead>
<tbody>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>517 </td><td>515 </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>533 </td><td>529 </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>542 </td><td>540 </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>544 </td><td>545 </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>554 </td><td>600 </td></tr>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>554 </td><td>558 </td></tr>
</tbody>
</table>



### `mutate()`: Add new columns


```R
new_tibble <- select(flights, arr_time, sched_arr_time) # create new tibble consisting of two columns: arr_time, sched_arr_time

head(mutate(new_tibble, arrival_delay = arr_time - sched_arr_time)) # mutate the new tibble by adding a new column: arr_time - sched_Arr_time
```


<table>
<thead><tr><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arrival_delay</th></tr></thead>
<tbody>
	<tr><td> 830</td><td> 819</td><td> 11 </td></tr>
	<tr><td> 850</td><td> 830</td><td> 20 </td></tr>
	<tr><td> 923</td><td> 850</td><td> 73 </td></tr>
	<tr><td>1004</td><td>1022</td><td>-18 </td></tr>
	<tr><td> 812</td><td> 837</td><td>-25 </td></tr>
	<tr><td> 740</td><td> 728</td><td> 12 </td></tr>
</tbody>
</table>





### `summarize()`: Collapse/summarize data 

The below code "summarizes" the `flights` data frame by providing the average delay of all the planes departing New York City in 2013. 


```R
summarize(flights, delay = mean(dep_delay, na.rm = TRUE))
```


<table>
<thead><tr><th scope=col>delay</th></tr></thead>
<tbody>
	<tr><td>12.63907</td></tr>
</tbody>
</table>



The function `group_by()` allows one to summarize data by groups. In the above code, the data frame is grouped by `year`, `month`, and `day`, which ensures that when the `summarize()` function is applied with `dep_delay` as a parameter, the resulting summary is that of average delay *per day*. 


```R
by_day <- group_by(flights, year, month, day) # group the flights data set by year, month, and day
head(summarize(by_day, delay=mean(dep_delay, na.rm = TRUE)))  # create new column called "delay" as a new statistical summary 
                                                              # "delay" is the mean of the "dep_delay" column grouped by day  
                                                              # ra.rm refers to removing missing values ("not available") when calculating the mean
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>delay</th></tr></thead>
<tbody>
	<tr><td>2013     </td><td>1        </td><td>1        </td><td>11.548926</td></tr>
	<tr><td>2013     </td><td>1        </td><td>2        </td><td>13.858824</td></tr>
	<tr><td>2013     </td><td>1        </td><td>3        </td><td>10.987832</td></tr>
	<tr><td>2013     </td><td>1        </td><td>4        </td><td> 8.951595</td></tr>
	<tr><td>2013     </td><td>1        </td><td>5        </td><td> 5.732218</td></tr>
	<tr><td>2013     </td><td>1        </td><td>6        </td><td> 7.148014</td></tr>
</tbody>
</table>



To look at the relationship between distance and delay, we first group the flights by destination. This involves a three-step procedure: group, summarize, and filter.  

> 1. Group
2. Summarize
3. Filter


```R
by_dest <- group_by(flights, dest) # group by "dest" column of the flights data set. "group_by" always before "summarize()"
delay <- summarize(by_dest, count = n(), dist = mean(distance, na.rm = TRUE), delay = mean(arr_delay, na.rm = TRUE))
                                   # by_dest = grouped data set, 
                                   # "count" is the variable (column) assigned to number of observations in the summary data
                                   # "count" is the number of flights assigned to a certain destination
                                   # "dist" is the mean distance of the flights grouped by destination, with n/a data removed.
                                   # "delay" is the mean delay time of flights grouped by destination
head(delay)
```


<table>
<thead><tr><th scope=col>dest</th><th scope=col>count</th><th scope=col>dist</th><th scope=col>delay</th></tr></thead>
<tbody>
	<tr><td>ABQ      </td><td>  254    </td><td>1826.0000</td><td> 4.381890</td></tr>
	<tr><td>ACK      </td><td>  265    </td><td> 199.0000</td><td> 4.852273</td></tr>
	<tr><td>ALB      </td><td>  439    </td><td> 143.0000</td><td>14.397129</td></tr>
	<tr><td>ANC      </td><td>    8    </td><td>3370.0000</td><td>-2.500000</td></tr>
	<tr><td>ATL      </td><td>17215    </td><td> 757.1082</td><td>11.300113</td></tr>
	<tr><td>AUS      </td><td> 2439    </td><td>1514.2530</td><td> 6.019909</td></tr>
</tbody>
</table>




```R
delay <- filter(delay, count > 20, dest != "HNL") # filter() finds the data with "count" exceeding 20, and removes dest = "HNL" 
head(delay) # "ANC," along with others such as "HNL," have been removed. 
```


<table>
<thead><tr><th scope=col>dest</th><th scope=col>count</th><th scope=col>dist</th><th scope=col>delay</th></tr></thead>
<tbody>
	<tr><td>ABQ      </td><td>  254    </td><td>1826.0000</td><td> 4.381890</td></tr>
	<tr><td>ACK      </td><td>  265    </td><td> 199.0000</td><td> 4.852273</td></tr>
	<tr><td>ALB      </td><td>  439    </td><td> 143.0000</td><td>14.397129</td></tr>
	<tr><td>ATL      </td><td>17215    </td><td> 757.1082</td><td>11.300113</td></tr>
	<tr><td>AUS      </td><td> 2439    </td><td>1514.2530</td><td> 6.019909</td></tr>
	<tr><td>AVL      </td><td>  275    </td><td> 583.5818</td><td> 8.003831</td></tr>
</tbody>
</table>




```R
ggplot(data = delay, mapping= aes(x = dist, y = delay)) +   # graph of delay
    geom_point(aes(size=count), alpha = 1/3) + # "geom_point" is a point geometric object.
    geom_smooth(se = FALSE) # "geom_smooth" is a geometric object that is displayed as a smooth conditional regression line. 
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'




![png](r2_files/r2_27_1.png)
    


The graph above shows that flights with air distance greater than approximately 1,000 kilometers tend to have less delay time, compared to those with less distance. Also, delay time appears to increase as the distance approaches 600 kilometers, although the opposite appears to be true as the distance climbs toward 2,000 kilometers. 

Although this was a relatively simple visualization task, the steps above involved individually naming objects in each step, which may get very confusing as the complexity of the task increases. To solve this inefficiency problem, as well as to vastly improve readability, there is a technique called "piping"--which involves a "pipe" operator "%>%." Pronounced "then," the pipe operator simplifies multistep procedures, as shown below.  


```R
delays2 <- flights %>% # "then" group by dest
    group_by(dest) %>% # "then" summarize the data by adding statistical summaries--"count," "dist," and "delay"--as columns. 
    summarize(
        count = n(),
        dist = mean(distance, na.rm = TRUE),
        delay = mean(arr_delay, na.rm = TRUE),
    ) %>%             # "then" filter the data frame, or choose the rows with count > 20, and dest != "HNL"
    filter(count > 20, dest != "HNL")

head(delays2)
```


<table>
<thead><tr><th scope=col>dest</th><th scope=col>count</th><th scope=col>dist</th><th scope=col>delay</th></tr></thead>
<tbody>
	<tr><td>ABQ      </td><td>  254    </td><td>1826.0000</td><td> 4.381890</td></tr>
	<tr><td>ACK      </td><td>  265    </td><td> 199.0000</td><td> 4.852273</td></tr>
	<tr><td>ALB      </td><td>  439    </td><td> 143.0000</td><td>14.397129</td></tr>
	<tr><td>ATL      </td><td>17215    </td><td> 757.1082</td><td>11.300113</td></tr>
	<tr><td>AUS      </td><td> 2439    </td><td>1514.2530</td><td> 6.019909</td></tr>
	<tr><td>AVL      </td><td>  275    </td><td> 583.5818</td><td> 8.003831</td></tr>
</tbody>
</table>



When using `summarize()` to add a new statistical summary as a column to the data set, such as in the example below, if there are any missing values in the data set, the new statistical summary will show up as "NA." For example, for the first six days of January, 2013, there are missing data with the departure delay time (`dep_delay`), which is why the `mean` column is "NA" for those first six days. 


```R
contains_missing = flights %>% 
    group_by(year, month, day) %>%
    summarize(mean = mean(dep_delay)) # no "na.rm = TRUE" argument
                                        
head(contains_missing) 
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>NA  </td></tr>
	<tr><td>2013</td><td>1   </td><td>2   </td><td>NA  </td></tr>
	<tr><td>2013</td><td>1   </td><td>3   </td><td>NA  </td></tr>
	<tr><td>2013</td><td>1   </td><td>4   </td><td>NA  </td></tr>
	<tr><td>2013</td><td>1   </td><td>5   </td><td>NA  </td></tr>
	<tr><td>2013</td><td>1   </td><td>6   </td><td>NA  </td></tr>
</tbody>
</table>




```R
# create new data frame with removed missing values

not_cancelled  <- flights %>% 
    filter(!is.na(dep_delay), !is.na(arr_delay))

not_cancelled2 = not_cancelled %>% 
    group_by(year, month, day) %>%
    summarize(mean = mean(dep_delay)) # no "na.rm = TRUE" argument

head(not_cancelled2)
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>mean</th></tr></thead>
<tbody>
	<tr><td>2013     </td><td>1        </td><td>1        </td><td>11.435620</td></tr>
	<tr><td>2013     </td><td>1        </td><td>2        </td><td>13.677802</td></tr>
	<tr><td>2013     </td><td>1        </td><td>3        </td><td>10.907778</td></tr>
	<tr><td>2013     </td><td>1        </td><td>4        </td><td> 8.965859</td></tr>
	<tr><td>2013     </td><td>1        </td><td>5        </td><td> 5.732218</td></tr>
	<tr><td>2013     </td><td>1        </td><td>6        </td><td> 7.145959</td></tr>
</tbody>
</table>




```R
delays <- not_cancelled %>%
    group_by(tailnum) %>%
    summarize(
        delay = mean(arr_delay, na.rm = TRUE),
        n = n()
    )

ggplot(data = delays, mapping=aes(x=n, y=delay)) + 
    geom_point(alpha = 1/10)
```


​    
![png](r2_files/r2_33_0.png)
​    


## `summarize()` functions: `mean()`, `sd()`,` IQR()`, `max()`,`min()`


```R
# average arrival delay time per day

two_arr_delays = not_cancelled %>%
    group_by(year, month, day) %>%
    summarize(
        # mean arrival delay time: 
        avg_delay1 = mean(arr_delay),
        # mean arrival delay time > 0:    
        avg_delay2 = mean(arr_delay[arr_delay > 0]),
    )

head(mean_arr_delay)
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>avg_delay1</th><th scope=col>avg_delay2</th></tr></thead>
<tbody>
	<tr><td>2013     </td><td>1        </td><td>1        </td><td>12.651023</td><td>32.48156 </td></tr>
	<tr><td>2013     </td><td>1        </td><td>2        </td><td>12.692888</td><td>32.02991 </td></tr>
	<tr><td>2013     </td><td>1        </td><td>3        </td><td> 5.733333</td><td>27.66087 </td></tr>
	<tr><td>2013     </td><td>1        </td><td>4        </td><td>-1.932819</td><td>28.30976 </td></tr>
	<tr><td>2013     </td><td>1        </td><td>5        </td><td>-1.525802</td><td>22.55882 </td></tr>
	<tr><td>2013     </td><td>1        </td><td>6        </td><td> 4.236429</td><td>24.37270 </td></tr>
</tbody>
</table>



### `sd()`: standard deviation


```R
# destinations with the highest standard deviation in distance
sd_distance = not_cancelled %>% 
    group_by(dest) %>%
    summarize(distance_sd = sd(distance)) %>%
    arrange(desc(distance_sd))
    
head(sd_distance)
```


<table>
<thead><tr><th scope=col>dest</th><th scope=col>distance_sd</th></tr></thead>
<tbody>
	<tr><td>EGE      </td><td>10.542765</td></tr>
	<tr><td>SAN      </td><td>10.350094</td></tr>
	<tr><td>SFO      </td><td>10.216017</td></tr>
	<tr><td>HNL      </td><td>10.004197</td></tr>
	<tr><td>SEA      </td><td> 9.977993</td></tr>
	<tr><td>LAS      </td><td> 9.907786</td></tr>
</tbody>
</table>



### `min()`, `max()`


```R
# first and last flights on each day

min_max_dep_time = not_cancelled %>%
    group_by(year, month, day) %>%
    summarize(
        first = min(dep_time),
        last = max(dep_time)
    )

head(min_max_dep_time)
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>first</th><th scope=col>last</th></tr></thead>
<tbody>
	<tr><td>2013</td><td>1   </td><td>1   </td><td>517 </td><td>2356</td></tr>
	<tr><td>2013</td><td>1   </td><td>2   </td><td> 42 </td><td>2354</td></tr>
	<tr><td>2013</td><td>1   </td><td>3   </td><td> 32 </td><td>2349</td></tr>
	<tr><td>2013</td><td>1   </td><td>4   </td><td> 25 </td><td>2358</td></tr>
	<tr><td>2013</td><td>1   </td><td>5   </td><td> 14 </td><td>2357</td></tr>
	<tr><td>2013</td><td>1   </td><td>6   </td><td> 16 </td><td>2355</td></tr>
</tbody>
</table>



### `n_distinct()`: number of unique values


```R
n_unique_carriers = not_cancelled %>%
    group_by(dest) %>%
    summarize(carriers = n_distinct(carrier)) %>%
    arrange(desc(carriers))

head(n_unique_carriers)
```


<table>
<thead><tr><th scope=col>dest</th><th scope=col>carriers</th></tr></thead>
<tbody>
	<tr><td>ATL</td><td>7  </td></tr>
	<tr><td>BOS</td><td>7  </td></tr>
	<tr><td>CLT</td><td>7  </td></tr>
	<tr><td>ORD</td><td>7  </td></tr>
	<tr><td>TPA</td><td>7  </td></tr>
	<tr><td>AUS</td><td>6  </td></tr>
</tbody>
</table>




```R
tailnum = not_cancelled %>%
    count(tailnum, wt=distance)

head(tailnum)
```


<table>
<thead><tr><th scope=col>tailnum</th><th scope=col>n</th></tr></thead>
<tbody>
	<tr><td>D942DN</td><td>  3418</td></tr>
	<tr><td>N0EGMQ</td><td>239143</td></tr>
	<tr><td>N10156</td><td>109664</td></tr>
	<tr><td>N102UW</td><td> 25722</td></tr>
	<tr><td>N103US</td><td> 24619</td></tr>
	<tr><td>N104UW</td><td> 24616</td></tr>
</tbody>
</table>




```R
popular_dest <- flights %>%
    group_by(dest)
    filter(n() > 365)

head(popular_dest)
```


<table>
<thead><tr><th scope=col>year</th><th scope=col>month</th><th scope=col>day</th><th scope=col>dep_time</th><th scope=col>sched_dep_time</th><th scope=col>dep_delay</th><th scope=col>arr_time</th><th scope=col>sched_arr_time</th><th scope=col>arr_delay</th><th scope=col>carrier</th><th scope=col>flight</th><th scope=col>tailnum</th><th scope=col>origin</th><th scope=col>dest</th><th scope=col>air_time</th><th scope=col>distance</th><th scope=col>hour</th><th scope=col>minute</th><th scope=col>time_hour</th></tr></thead>
<tbody>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>517                </td><td>515                </td><td> 2                 </td><td> 830               </td><td> 819               </td><td> 11                </td><td>UA                 </td><td>1545               </td><td>N14228             </td><td>EWR                </td><td>IAH                </td><td>227                </td><td>1400               </td><td>5                  </td><td>15                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>533                </td><td>529                </td><td> 4                 </td><td> 850               </td><td> 830               </td><td> 20                </td><td>UA                 </td><td>1714               </td><td>N24211             </td><td>LGA                </td><td>IAH                </td><td>227                </td><td>1416               </td><td>5                  </td><td>29                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>542                </td><td>540                </td><td> 2                 </td><td> 923               </td><td> 850               </td><td> 33                </td><td>AA                 </td><td>1141               </td><td>N619AA             </td><td>JFK                </td><td>MIA                </td><td>160                </td><td>1089               </td><td>5                  </td><td>40                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>544                </td><td>545                </td><td>-1                 </td><td>1004               </td><td>1022               </td><td>-18                </td><td>B6                 </td><td> 725               </td><td>N804JB             </td><td>JFK                </td><td>BQN                </td><td>183                </td><td>1576               </td><td>5                  </td><td>45                 </td><td>2013-01-01 05:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>600                </td><td>-6                 </td><td> 812               </td><td> 837               </td><td>-25                </td><td>DL                 </td><td> 461               </td><td>N668DN             </td><td>LGA                </td><td>ATL                </td><td>116                </td><td> 762               </td><td>6                  </td><td> 0                 </td><td>2013-01-01 06:00:00</td></tr>
	<tr><td>2013               </td><td>1                  </td><td>1                  </td><td>554                </td><td>558                </td><td>-4                 </td><td> 740               </td><td> 728               </td><td> 12                </td><td>UA                 </td><td>1696               </td><td>N39463             </td><td>EWR                </td><td>ORD                </td><td>150                </td><td> 719               </td><td>5                  </td><td>58                 </td><td>2013-01-01 05:00:00</td></tr>
</tbody>
</table>


