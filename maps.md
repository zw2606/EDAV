# (PART) Special Data Types {-}

# Spatial Data {#maps}

<!-- Under Construction Section
----------------------------------------------------------------------------- -->
![Maps](images/banners/banner_maps.png)
*This page is a work in progress. We appreciate any input you may have. If you would like to help improve this page, consider [contributing to our repo](contribute.html).*
<!-- ------------------------------------------------------------------------ -->


## Choropleth maps

Choropleth maps use color to indicate the value of a variable within a defined region, generally political boundaries. ["Mapping in R just got a whole lot easier"](https://www.computerworld.com/article/3175623/mapping-in-r-just-got-a-whole-lot-easier.html){target="_blank"} by Sharon Machlis (2017-03-03) offers a tutorial on using the **tmap**, **tmaptools**, and **tigris** packages to create choropleth maps. Note that with this approach, you will need to merge geographic shape files with your data files, and then map. 

["Step-by-Step Choropleth Map in R: A case of mapping Nepal"](https://medium.com/@anjesh/step-by-step-choropleth-map-in-r-a-case-of-mapping-nepal-7f62a84078d9){target="_blank"} walks through the process of creating a choropleth map using **rgdal** and **ggplot2**.  (We have not followed either of these tutorials step-by-step... if you do, please provide feedback by [submitting an issue](https://github.com/jtr13/EDAV/issues){target="_blank"}).


The **choroplethr** package makes it simple to draw choropleth maps of [U.S. states, countries, and census tracts, as well as countries of the world](https://arilamstein.com/documentation/choroplethr/reference/){target="_blank"} without dealing directly with shape files. The companion package, **choroplethrZip**, provides data for [zip code level choropleths](https://arilamstein.com/creating-zip-code-choropleths-choroplethrzip/){target="_blank"}; **choroplethrAdmin1** draws choropleths for administrative regions of [world countries](https://rdrr.io/cran/choroplethrAdmin1/man/get_admin1_countries.html){target="_blank"}. The following is a brief tutorial on using these packages.

Note: You must install also install **choroplethrMaps** for **choroplethr** to work.  In addition, **choroplethr** requires a number of other dependencies which should be installed automatically, but if they aren't, you can manually install the missing packages that you are notified about when you call `library(choroplethr)`: **maptools**, and **rgdal**, **sp**.

We'll use the `state.x77` dataset for this example:


```r
library(tidyverse)
library(choroplethr)

# data frame must contain "region" and "value" columns

df_illiteracy <- state.x77 %>% as.data.frame() %>% 
  rownames_to_column("state") %>% 
  transmute(region = tolower(`state`), value = Illiteracy)

state_choropleth(df_illiteracy,
                 title = "State Illiteracy Rates, 1977",
                 legend = "Percent Illiterate")
```

<img src="maps_files/figure-html/unnamed-chunk-1-1.png" width="672" />

**Note**: the `choroplethr` "free course" that you may come across arrives one lesson at a time by email over an extended period so not the best option unless you have a few weeks to spare.


## Square bins

Packages such as `statebins` create choropleth style maps with equal size regions that roughly represent the location of the region, but not the size or shape.

**Important**:  Don't install `statebins` from CRAN; use the [dev version](https://github.com/hrbrmstr/statebins){target="_blank"} -- it contains many improvements, which are detailed in ["Statebins Reimagined"](https://rud.is/b/2017/11/18/statebins-reimagined/#comment-19346){target="_blank"}.


```r
# devtools::install_github("hrbrmstr/statebins")
library(statebins)
df_illit <- state.x77 %>% as.data.frame() %>% 
  rownames_to_column("state") %>% 
  select(state, Illiteracy)

# Note: direction = 1 switches the order of the fill scale 
# so darker shades represent higher illiteracy rates
# (The default is -1).

statebins(df_illit, value_col="Illiteracy",
          name = "%", direction = 1) +
  ggtitle("State Illiteracy Rates, 1977") +
  theme_statebins()
```

<img src="maps_files/figure-html/unnamed-chunk-2-1.png" width="672" />

## Longitude / Latitude data 

Note that the options above work with *political boundaries*, based on the names of the regions that you provide. Such maps require packages with geographical boundary information. Longitude / latitude data, on the other hand, can be plotted simply as a scatterplot with x = longitude and y = latitude, without any background maps (just don't mix up x & y!) The first part of ["Data wrangling visualisation and spatial analysis: R Workshop"](http://www.seascapemodels.org/data/data-wrangling-spatial-course.html) by C. Brown, D. Schoeman, A. Richardson, and B. Venables provides a detailed walkthrough of spatial exploratory data analysis with copepod data (a type of zooplankton) using this technique with `ggplot2::geom_point()`. 

If background maps are desired, there are many options.  The [tutorial](http://www.seascapemodels.org/data/data-wrangling-spatial-course.html) mentioned above provides examples using the **maps** or **sf** packages. It is a highly recommended resource as it covers much of the data science pipeline from the context of the problem to obtaining data, cleaning and transforming it, exploring the data, and finally modeling and predicting.

Another good choice for background maps is **ggmap**, which offers several different map source options.  Google Maps API was the go-to, but they now [require you to enable billing through Google Cloud Platorm](https://cloud.google.com/free/){target="_blank"}.  You get $300 in free credit, but if providing a credit card isn't your thing, you may consider using Stamen Maps instead, with the `get_stamenmap()` function. Use the development version of the package; instructions and extensive examples are available on the package's [GitHub page](https://github.com/dkahle/ggmap){target="_blank"} ["Getting started Stamen maps with ggmap"](https://statisticaloddsandends.wordpress.com/2018/10/25/getting-started-stamen-maps-with-ggmap/){target="_blank"} will help you get started with Stamen maps through an example using the Sacramento dataset in the **caret** package.
