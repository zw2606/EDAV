# Chart: Boxplot {#box}




![](images/banners/banner_boxplot.png)

## tl;dr
I want a nice example and I want it NOW!

Here's a look at the weights of newborn chicks split by the feed supplement they received:

<img src="boxplot_files/figure-html/tldr-show-plot-1.png" width="75%" style="display: block; margin: auto;" />

And here's the code:

```r
library(ggplot2)

# boxplot by feed supplement 
ggplot(chickwts, aes(x = reorder(feed, -weight, median), y = weight)) + 
  # plotting
  geom_boxplot(fill = "#cc9a38", color = "#473e2c") + 
  # formatting
  ggtitle("Casein Makes You Fat?!",
          subtitle = "Boxplots of Chick Weights by Feed Supplement") +
  labs(x = "Feed Supplement", y = "Chick Weight (g)", caption = "Source: datasets::chickwts") +
  theme_grey(16) +
  theme(plot.title = element_text(face = "bold")) +
  theme(plot.subtitle = element_text(face = "bold", color = "grey35")) +
  theme(plot.caption = element_text(color = "grey68"))
```

For more info on this dataset, type `?datasets::chickwts` into the console.

## Simple examples
Okay...*much* simpler please.

### Single boxplots

Base R will give you a quick boxplot of a vector or a single column of a data frame with very little typing:


```r
# vector
boxplot(rivers) 
```

<img src="boxplot_files/figure-html/base-r-1.png" width="60%" style="display: block; margin: auto;" />

Or, the horizontal version:


```r
# single column of a data frame
boxplot(chickwts$weight, horizontal = TRUE) 
```

<img src="boxplot_files/figure-html/unnamed-chunk-3-1.png" width="60%" style="display: block; margin: auto;" />

Creating a single boxplot in **ggplot2** is somewhat problematic. (The joke is that it's the package author's way of saying that if you only have one group, make a histogram instead!)  

If you only include one aesthetic mapping, it will be assumed to the `x` (group) variable and you will get an error:


```r
ggplot(chickwts, aes(weight)) + geom_boxplot()
```

```
Error: stat_boxplot requires the following missing aesthetics: y
```

This can be remedied by adding `y = ` to indicate that `weight` is the numeric variable, but you'll still get a meaningless x-axis:


```r
ggplot(chickwts, aes(y = weight)) + 
  geom_boxplot() +
  theme_grey(16) # make all font sizes larger (default is 11)
```

<img src="boxplot_files/figure-html/unnamed-chunk-5-1.png" width="60%" style="display: block; margin: auto;" />

Another, cleaner approach is to create a name for the single group as the `x` aesthetic and remove the x-axis label:


```r
ggplot(chickwts, aes(x = "all 71 chickens", y = weight)) + 
  geom_boxplot() + xlab("") + theme_grey(16)
```

<img src="boxplot_files/figure-html/unnamed-chunk-6-1.png" width="60%" style="display: block; margin: auto;" />

### Multiple boxplots using ggplot2

To create multiple boxplots with **ggplot2**, your data frame needs to be tidy, that is you need to have a column with levels of the grouping variable. It can be be factor, character, or integer class.


```r
str(chickwts)
```

```
## 'data.frame':	71 obs. of  2 variables:
##  $ weight: num  179 160 136 227 217 168 108 124 143 140 ...
##  $ feed  : Factor w/ 6 levels "casein","horsebean",..: 2 2 2 2 2 2 2 2 2 2 ...
```

We see that `chickwts` is in the right form: we have a `feed` column with six factor levels, so we can set the the `x` aesthetic to `feed`. We also order the boxplots by *decreasing median weight*:


```r
ggplot(chickwts, aes(x = reorder(feed, -weight, median), y = weight)) +
  geom_boxplot() +
  xlab("feed type") +
  theme_grey(16)
```

<img src="boxplot_files/figure-html/unnamed-chunk-8-1.png" width="60%" style="display: block; margin: auto;" />

Data frames that contain a separate column of values for each desired boxplot must be tidied first. (For more detail on using `tidy::gather()`, see [this tutorial](https://github.com/jtr13/codehelp/blob/master/R/gather.md){target="_blank"}.)


```r
library(tidyverse)
head(attitude)
```

```
##   rating complaints privileges learning raises critical advance
## 1     43         51         30       39     61       92      45
## 2     63         64         51       54     63       73      47
## 3     71         70         68       69     76       86      48
## 4     61         63         45       47     54       84      35
## 5     81         78         56       66     71       83      47
## 6     43         55         49       44     54       49      34
```

```r
tidyattitude <- attitude %>% gather(key = "question", value = "rating")
head(tidyattitude)
```

```
##     question rating
## 1 complaints     51
## 2 complaints     64
## 3 complaints     70
## 4 complaints     63
## 5 complaints     78
## 6 complaints     55
```

Now we're ready to plot:


```r
ggplot(tidyattitude, aes(reorder(question, -rating, median), rating)) + 
  geom_boxplot() +
  xlab("question short name") +
  theme_grey(16)
```

<img src="boxplot_files/figure-html/unnamed-chunk-10-1.png" width="60%" style="display: block; margin: auto;" />


## Theory

Here's a quote by Hadley Wickham  that sums up boxplots nicely:

>The boxplot is a compact distributional summary, displaying less detail than a histogram or kernel density, but also taking up less space. Boxplots use robust summary statistics that are always located at actual data points, are quickly computable (originally by hand), and have no tuning parameters. They are particularly useful for comparing distributions across groups. - Hadley Wickham
>

Another important use of the boxplot is in showing outliers. A boxplot shows how much of an outlier a data point is with quartiles and fences. Use the boxplot when you have data with outliers so that they can be exposed. What it lacks in specificity it makes up with its ability to clearly summarize large data sets.

*   For more info about boxplots and continuous variables, check out [Chapter 3](http://www.gradaanwr.net/content/03-examining-continuous-variables/){target="_blank"} of the textbook. 

## When to use

Boxplots should be used to display *continuous variables*. They are particularly useful for identifying outliers and comparing different groups. 

*Aside*: Boxplots may even help you [convince someone you are their outlier](https://xkcd.com/539/){target="_blank"} (If you like it when people over-explain jokes, [here is why that comic is funny](https://www.explainxkcd.com/wiki/index.php/539:_Boyfriend){target="_blank"}.). 

## Considerations

### Flipping orientation
Often you want boxplots to be horizontal. Super easy to do in **ggplot2**: just tack on `+ coord_flip()` and remove the `-` from the reordering so that the factor level with the highest median will be on top:


```r
ggplot(tidyattitude, aes(reorder(question, rating, median), rating)) + 
  geom_boxplot() +
  coord_flip() +
  xlab("question short name") +
  theme_grey(16)
```

<img src="boxplot_files/figure-html/flip-boxplot-1.png" width="60%" style="display: block; margin: auto;" />

Note that switching `x` and `y` insteading of using `coord_flip()` doesn't work!


```r
ggplot(tidyattitude, aes(rating, reorder(question, rating, median))) + 
  geom_boxplot() +
  ggtitle("This is not what we wanted!") +
  ylab("question short name") +
  theme_grey(16)
```

<img src="boxplot_files/figure-html/unnamed-chunk-11-1.png" width="60%" style="display: block; margin: auto;" />

### NOT for categorical data

Boxplots are great, but they do NOT work with categorical data. Make sure your variable is continuous before using boxplots.

The data in this example are variables from the `pisaitems` dataset in the **likert** package with ratings of 1, 2, 3 or 4:




```r
head(pisa, 4)
```

```
##   ST24Q01 ST24Q02 ST24Q03 ST24Q04 ST24Q05 ST24Q06
## 1       2       4       4       1       4       1
## 2       3       1       1       4       1       3
## 3       4       1       1       3       1       4
## 4       2       2       3       1       2       2
```

Creating a boxplot from this data is a good example of what *not* to do:

<img src="boxplot_files/figure-html/bad-boxplots-1.png" width="75%" style="display: block; margin: auto;" />

## External resources

- Tukey, John W. 1977. [*Exploratory Data Analysis.*](https://clio.columbia.edu/catalog/136422){target="_blank"} Addison-Wesley. (Chapter 2): the primary source in which boxplots are first presented. 
- [Article on boxplots with ggplot2](http://t-redactyl.io/blog/2016/04/creating-plots-in-r-using-ggplot2-part-10-boxplots.html){target="_blank"}: An excellent collection of code examples on how to make boxplots with `ggplot2`. Covers layering, working with legends, faceting, formatting, and more. If you want a boxplot to look a certain way, this article will help.
- [Boxplots with plotly package](https://plot.ly/ggplot2/box-plots/){target="_blank"}: boxplot examples using the `plotly` package. These allow for a little interactivity on hover, which might better explain the underlying statistics of your plot.
- [ggplot2 Boxplot: Quick Start Guide](http://www.sthda.com/english/wiki/ggplot2-box-plot-quick-start-guide-r-software-and-data-visualization){target="_blank"}: Article from [STHDA](http://www.sthda.com/english/){target="_blank"} on making boxplots using ggplot2. Excellent starting point for getting immediate results and custom formatting.
- [ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf){target="_blank"}: Always good to have close by.
- [Hadley Wickhan and Lisa Stryjewski on boxplots](http://vita.had.co.nz/papers/boxplots.pdf){target="_blank"}: good for understanding basics of more complex boxplots and some of the history behind them.





