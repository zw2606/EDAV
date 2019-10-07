# (PART) Continuous Variables {-}

# Chart: Histogram {#histo}

![](images/banners/banner_histogram.png)

## Overview

This section covers how to make histograms.

## tl;dr
Gimme a full-fledged example!

Here's an application of histograms that looks at how the beaks of Galapagos finches changed due to external factors:

<img src="histogram_files/figure-html/tldr-finch-example-1.png" width="672" />

And here's the code:

```r
library(Sleuth3) # data
library(ggplot2) # plotting

# load data
finches <- Sleuth3::case0201
# finch histograms by year with overlayed density curves
ggplot(finches, aes(x = Depth, y = ..density..)) + 
  # plotting
  geom_histogram(bins = 20, colour = "#80593D", fill = "#9FC29F", boundary = 0) +
  geom_density(color = "#3D6480") + 
  facet_wrap(~Year) +
  # formatting
  ggtitle("Severe Drought Led to Finches with Bigger Chompers",
          subtitle = "Beak Depth Density of Galapagos Finches by Year") +
  labs(x = "Beak Depth (mm)", caption = "Source: Sleuth3::case0201") +
  theme(plot.title = element_text(face = "bold")) +
  theme(plot.subtitle = element_text(face = "bold", color = "grey35")) +
  theme(plot.caption = element_text(color = "grey68"))
```

For more info on this dataset, type `?Sleuth3::case0201` into the console.

## Simple examples
Whoa whoa whoa! Much simpler please!

Let's use a very simple dataset:

```r
# store data
x <- c(50, 51, 53, 55, 56, 60, 65, 65, 68)
```

### Histogram using base R

```r
# plot data
hist(x, col = "lightblue", main = "Base R Histogram of x")
```

<img src="histogram_files/figure-html/base-r-hist-1.png" width="672" />

For the Base R histogram, it's advantages are in it's ease to setup. In truth, all you need to plot the data `x` in question is `hist(x)`, but we included a little color and a title to make it more presentable. 

Full documentation on `hist()` can be found [here](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/hist){target="_blank"}

### Histogram using ggplot2

```r
# import ggplot
library(ggplot2)
# must store data as dataframe
df <- data.frame(x)

# plot data
ggplot(df, aes(x)) +
  geom_histogram(color = "grey", fill = "lightBlue", 
                 binwidth = 5, center = 52.5) +
  ggtitle("ggplot2 histogram of x")
```

<img src="histogram_files/figure-html/ggplot-hist-1.png" width="672" />

The ggplot version is a little more complicated on the surface, but you get more power and control as a result. **Note**: as shown above, ggplot expects a dataframe, so if you are getting an error where "R doesn't know what to do" like this:

![ggplot dataframe error](images/ggplot_df_error.png)

make sure you are using a dataframe.

## Theory

Generally speaking, the histogram is one of many options for displaying continuous data. 

The histogram is clear and quick to make. Histograms are relatively self-explanatory: they show your data's empirical distribution within a set of intervals. Histograms can be employed on raw data to quickly show the distribution without much manipulation. Use a histogram to get a basic sense of the distribution with minimal processing necessary.

*   For more info about histograms and continuous variables, check out [Chapter 3](http://www.gradaanwr.net/content/03-examining-continuous-variables/){target="_blank"} of the textbook. 

## Types of histograms

Use a histogram to show the distribution of *one continuous variable*. The y-scale can be represented in a variety of ways to express different results:

### Frequency or count

y = number of values that fall in each bin

### Relative frequency historgram

y = number of values that fall in each bin / total number of values

### Cumulative frequency histogram

y = total number of values <= (or <) right boundary of bin

### Density

y = relative frequency / binwidth


## Parameters

### Bin boundaries
Be mindful of the boundaries of the bins and whether a point will fall into the left or right bin if it is on a boundary.

```r
# format layout
op <- par(mfrow = c(1, 2), las = 1)

# right closed
hist(x, col = "lightblue", ylim = c(0, 4),
     xlab = "right closed ex. (55, 60]", font.lab = 2)
# right open
hist(x, col = "lightblue", right = FALSE, ylim = c(0, 4),
     xlab = "right open ex. [55, 60)", font.lab = 2)
```

<img src="histogram_files/figure-html/bin-boundaries-1.png" width="672" />

### Bin number
The default bin number of 30 in ggplot2 is not always ideal, so consider altering it if things are looking strange. You can specify the width explicitly with `binwidth` or provide the desired number of bins with `bins`.

```r
# default...note the pop-up about default bin number
ggplot(finches, aes(x = Depth)) +
  geom_histogram() +
  ggtitle("Default with pop-up about bin number")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="histogram_files/figure-html/unnamed-chunk-1-1.png" width="672" />

Here are examples of changing the bins using the two ways described above:

```r
# using binwidth
p1 <- ggplot(finches, aes(x = Depth)) +
  geom_histogram(binwidth = 0.5, boundary = 6) +
  ggtitle("Changed binwidth value")
# using bins
p2 <- ggplot(finches, aes(x = Depth)) +
  geom_histogram(bins = 48, boundary = 6) +
  ggtitle("Changed bins value")

# format plot layout
library(gridExtra)
grid.arrange(p1, p2, ncol = 2)
```

<img src="histogram_files/figure-html/fixed-histograms-binwidth-1.png" width="672" />


### Bin alignment
Make sure the axes reflect the true boundaries of the histogram. You can use `boundary` to specify the endpoint of any bin or `center` to specify the center of any bin. `ggplot2` will be able to calculate where to place the rest of the bins (Also, notice that when the boundary was changed, the number of bins got smaller by one. This is because by default the bins are centered and go over/under the range of the data.)

```r
df <- data.frame(x)

# default alignment
ggplot(df, aes(x)) +
  geom_histogram(binwidth = 5,
                 fill = "lightBlue", col = "black") +
  ggtitle("Default Bin Alignment")
```

<img src="histogram_files/figure-html/unnamed-chunk-2-1.png" width="672" />


```r
# specify alignment with boundary
p3 <- ggplot(df, aes(x)) +
  geom_histogram(binwidth = 5, boundary = 60,
                 fill = "lightBlue", col = "black") +
  ggtitle("Bin Alignment Using boundary")

# specify alignment with center
p4 <- ggplot(df, aes(x)) +
  geom_histogram(binwidth = 5, center = 67.5,
                 fill = "lightBlue", col = "black") +
  ggtitle("Bin Alignment Using center")

# format layout
library(gridExtra)
grid.arrange(p3, p4, ncol = 2)
```

<img src="histogram_files/figure-html/alignment-fix-1.png" width="672" />

**Note**: Don't use both `boundary` *and* `center` for bin alignment. Just pick one.

## Interactive histograms with `ggvis`

The `ggvis` package is not currently in development, but does certain things very well, such as adjusting parameters of a histogram interactively while coding.

Since images cannot be shared by knitting (as with other packages, such as `plotly`), we present the code here, but not the output. To try them out, copy and paste into an R session.

###  Change binwidth interactively

```r
library(tidyverse)
library(ggvis)
faithful %>% ggvis(~eruptions) %>% 
    layer_histograms(fill := "lightblue", 
        width = input_slider(0.1, 2, value = .1, 
                             step = .1, label = "width"))
```

### GDP example

```r
df <-read.csv("countries2012.csv")
df %>% ggvis(~GDP) %>% 
    layer_histograms(fill := "green", 
        width = input_slider(500, 10000, value = 5000, 
        step = 500, label = "width"))
```

### Change center interactively 

```r
df <- data.frame(x = c(50, 51, 53, 55, 56, 60, 65, 65, 68))
df %>% ggvis(~x) %>% 
    layer_histograms(fill := "red", 
        width = input_slider(1, 10, value = 5, step = 1, label = "width"),
        center = input_slider(50, 55, value = 52.5, step = .5, label = "center"))
```

### Change center (with data values shown)

```r
df <- data.frame(x = c(50, 51, 53, 55, 56, 60, 65, 65, 68), 
                 y = c(.5, .5, .5, .5, .5, .5, .5, 1.5, .5))
df %>% ggvis(~x, ~y) %>% 
    layer_histograms(fill := "lightcyan", width = 5,
                     center = input_slider(45, 55, value = 45, 
                                           step = 1, label = "center")) %>% 
  layer_points(fill := "blue", size := 200) %>% 
  add_axis("x", properties = axis_props(labels = list(fontSize = 20))) %>% 
  scale_numeric("x", domain = c(46, 72)) %>% 
  add_axis("y", values = 0:3, 
           properties = axis_props(labels = list(fontSize = 20)))
```

### Change boundary interactively

```r
df %>% ggvis(~x) %>% 
    layer_histograms(fill := "red", 
        width = input_slider(1, 10, value = 5, 
                             step = 1, label = "width"),
        boundary = input_slider(47.5, 50, value = 50,
                                step = .5, label = "boundary"))
```


## External resources
- [hist documentation](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/hist){target="_blank"}: base R histogram documentation page.
- [ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf){target="_blank"}: Always good to have close by.



