# (PART) Categorical Variables {-}

# Chart: Bar Chart {#bar}

![](images/banners/banner_bargraph.png)

## Overview

This section covers how to make bar charts

## tl;dr
<!-- Gimme example -->
I want a nice example. Not tomorrow, not *after breakfast*. NOW! 

<!-- Explanation: -->
Here's a bar chart showing the survival rates of passengers aboard the *RMS Titanic*:

<img src="bargraph_files/figure-html/tldr-show-plot-1.png" width="864" />

And here's the code:

```r
library(datasets) # data
library(ggplot2) # plotting
library(dplyr) # manipulation

# Combine Children and Adult stats together
ship_grouped <- as.data.frame(Titanic) %>%
  group_by(Class, Sex, Survived) %>%
  summarise(Total = sum(Freq))

ggplot(ship_grouped, aes(x = Survived, y = Total, fill = Sex)) +
  geom_bar(position = "dodge", stat = "identity") +
  geom_text(aes(label = Total), position = position_dodge(width = 0.9), 
            vjust = -0.4, color = "grey68") +
  facet_wrap(~Class) +
  # formatting
  ylim(0, 750) +
  ggtitle("Don't Be A Crew Member On The Titanic",
          subtitle = "Survival Rates of Titanic Passengers by Class and Gender") +
  scale_fill_manual(values = c("#b2df8a", "#a6cee3")) +
  labs(y = "Passenger Count", caption = "Source: titanic::titanic_train") +
  theme(plot.title = element_text(face = "bold")) +
  theme(plot.subtitle = element_text(face = "bold", color = "grey35")) +
  theme(plot.caption = element_text(color = "grey68"))
```

For more info on this dataset, type `?datasets::Titanic` into the console.

## Simple examples
<!-- Simplify Note -->
My eyes were bigger than my stomach. Much simpler please!

<!-- Simple Explanation of Data: -->
Let's use the `HairEyeColor` dataset. To start, we will just look at the different categories of hair color among females:

```r
colors <- as.data.frame(HairEyeColor)

# just female hair color, using dplyr
colors_female_hair <- colors %>%
  filter(Sex == "Female") %>%
  group_by(Hair) %>%
  summarise(Total = sum(Freq))

# take a look at data
head(colors_female_hair)
```

```
## # A tibble: 4 x 2
##   Hair  Total
##   <fct> <dbl>
## 1 Black    52
## 2 Brown   143
## 3 Red      37
## 4 Blond    81
```

Now let's make some graphs with this data.

### Bar graph using base R

```r
barplot(colors_female_hair[["Total"]], 
        names.arg = colors_female_hair[["Hair"]],
        main = "Bar Graph Using Base R")
```

<img src="bargraph_files/figure-html/base-r-1.png" width="672" />

<!-- Base R Plot Explanation -->
We recommend using Base R only for simple bar graphs for yourself. Like all of Base R, it is simple to setup. **Note**: Base R expects a vector or matrix, hence the double brackets in the barplot call (gets columns as lists).

### Bar graph using ggplot2

```r
library(ggplot2) # plotting

ggplot(colors_female_hair, aes(x = Hair, y = Total)) +
  geom_bar(stat = "identity") +
  ggtitle("Bar Graph Using ggplot2")
```

<img src="bargraph_files/figure-html/ggplot-1.png" width="672" />

<!-- ggplot2 explanation -->
Bar plots are very easy in `ggplot2`. You pass in a dataframe and let it know which parts you want to map to different aesthetics. **Note**: In this case, we have a table of values and want to plot them as explicit bar heights. Because of this, we specify the y aesthetic as the `Total` column, but we also have to specify `stat = "identity"` in `geom_bar()` so it knows to plot them correctly. Often you will have datasets where each row is one observation and you want to group them into bars. In that case, the y aesthetic and `stat = "identity"` do not have to be specified.

## Theory
<!-- *Link to textbook -->
*   For more info about plotting categorical data, check out [Chapter 4](http://www.gradaanwr.net/content/04-displaying-categorial-data/){target="_blank"} of the textbook.

## When to use
<!-- Quick Note on When to use this plot -->
Bar Charts are best for *categorical data*. Often you will have a collection of factors that you want to split into different groups. 

## Considerations

<!-- *   List of things to pay attention to with examples -->
### Not for continuous data
If you are finding that your bar graphs aren't looking right, make sure your data is categorical and not continuous. If you want to plot continuous data using bars, that is what [histograms](histo.html) are for!

## Modifications
<!-- Things to add on -->
<!--
- Flip bars
- Facet Wrap
-->
These modifications assume you are using `ggplot2`.

### Flip Bars
To flip the orientation, just tack on `coord_flip()`:

```r
ggplot(colors_female_hair, aes(x = Hair, y = Total)) +
  geom_bar(stat = "identity") +
  ggtitle("Bar Graph Using ggplot2") +
  coord_flip()
```

<img src="bargraph_files/figure-html/coord-flip-1.png" width="672" />

### Reorder the bars
With both base R and **ggplot2** bars are drawn in alphabetical order for character data and in the order of factor levels for factor data. However, since the default order of levels for factor data is alphabetical, the bars will be alphabetical in both cases. Please see [this tutorial](https://github.com/jtr13/codehelp/blob/master/R/reorder.md){target="_blank"} for a detailed explanation on how bars should be ordered in a bar chart, and how the **forcats** package can help you accomplish the reordering.

### Facet Wrap
You can split the graph into small multiples using `facet_wrap()` (don't forget the tilde, ~):

```r
ggplot(colors, aes(x = Sex, y = Freq)) +
  geom_bar(stat = "identity") +
  facet_wrap(~Hair)
```

<img src="bargraph_files/figure-html/small-multiples-1.png" width="672" />

## External resources
<!-- - [](){target="_blank"}: Links to resources with quick blurb -->
- [Cookbook for R](http://www.cookbook-r.com/Manipulating_data/Changing_the_order_of_levels_of_a_factor/){target="_blank"}: Discussion on reordering the levels of a factor.
- [DataCamp Exercise](https://campus.datacamp.com/courses/data-visualization-with-ggplot2-2/chapter-4-best-practices?ex=4#skiponboarding){target="_blank"}: Simple exercise on making bar graphs with `ggplot2`.
- [ggplot2 cheatsheet](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf){target="_blank"}: Always good to have close by.


