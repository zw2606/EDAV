---
output: html_document
---
# R Basics {#basics} 

![](images/banners/banner_basics.png)



So...there is soooo much to the world of R. Textbooks, cheatsheets, exercises, and other buzzwords full of resources you could go through. There are over 15000 packages on [CRAN](https://cran.r-project.org){target="_blank"}, the network through which R code and packages are distributed. It can be overwhelming.  However, bear in mind that R is being used for a lot of different things, not all of which are relevant to EDAV. 

In an effort to get everyone on the same page, here is a checklist of essentials so you can get up and running with this course. The best resources are scattered in different places online, so bear with links to various sites depending on the topic.

## Top 10 Essentials Checklist

(*r4ds* = [R for Data Science](https://r4ds.had.co.nz/) by Garrett Grolemund and Hadley Wickham, free online)





1. [Install R](https://r4ds.had.co.nz/introduction.html#r2){target="_blank"} (*r4ds*) -- You need to have this installed but you won't open the application since you'll be working in RStudio. If you already installed R, make sure you're current! The latest version of R (as of 2019-10-07) is R 3.6.1 "Action of the Toes" released on 2019/07/05.

2. [Install RStudio](https://r4ds.had.co.nz/introduction.html#rstudio3){target="_blank"} (*r4ds*) -- Download the free, Desktop version for your OS. Working in this IDE will make working in R much more enjoyable. As with R, stay current. RStudio is constantly adding new features. The latest version (as of 2019-10-07) is 1.2.5001. 

3. [Get comfortable with RStudio](https://b-rodrigues.github.io/modern_R/getting-to-know-rstudio.html){target="_blank"} -- In this chapter of Bruno Rodriguez's *Modern R with the Tidyverse*, you'll learn about panes, options, getting help, keyboard shortcuts, projects, add-ins, and packages. Be sure to try out:

    - Do some math in the console
    - Create an R Markdown file (`.Rmd`) and render it to `.html`
    - Install some packages like `tidyverse` or `MASS`  
    
    
    Another great option for learning the IDE: Watch [Writing Code in RStudio](https://www.rstudio.com/resources/webinars/rstudio-essentials-webinar-series-part-1/){target="_blank"} (*RStudio webinar*)

4. Learn ["R Nuts and Bolts"](https://bookdown.org/rdpeng/rprogdatascience/r-nuts-and-bolts.html){target="_blank"} -- Roger Peng's chapter in *R Programming* will give you a solid foundation in the basic building blocks of R. It's worth making the investing in understanding how R objects work now so they don't cause you problems later. Focus on **vectors** and especially **data frames**; matrices and lists don't come up often in data visualization.  Get familiar with R classes: **integer, numeric, character,** and **logical**. Understand how **factors** work; they are very important for graphing.

5. [Learn some RMarkdown](https://rmarkdown.rstudio.com/articles_intro.html){target="_blank"} -- For this class you will write assignments in R Markdown (stored as `.Rmd` files) and then render them into pdfs for submission. You can jump right in and open a new R Markdown file (*File > New File > R Markdown...*), and leave the `Default Output Format` as `HTML`. You will get a R Markdown template you can tinker with. Click the "knit" button and see what happens. For more detail, watch the RStudio webinar [Getting Started with R Markdown](https://resources.rstudio.com/the-essentials-of-data-science/getting-started-with-r-markdown-60-02) 

6. [Tidy up](https://r4ds.had.co.nz/introduction.html#the-tidyverse){target=_blank} (*r4ds*) -- Install the tidyverse, and get familiar with what it is. *We will discuss differences between base R and the tidyverse in class.* 

7. [Learn ggplot2 basics](https://r4ds.had.co.nz/data-visualisation.html){target="_blank"} (*r4ds*) -- In class we will study the grammar of graphics on which **ggplot2** is based, but it will help to familiarize yourself with the syntax in advance. Avail yourself of the "Data Visualization with ggplot2" cheatsheet by clicking "Help" "Cheatsheets..." within RStudio.

8. [Use RStudio projects](https://r4ds.had.co.nz/workflow-projects.html){target="_blank"} (*r4ds*) -- If you haven't already, drink the Kool-Aid. Make each problem set a separate project. You will never have to worry about `getwd()` or `setwd()` again because everything will just be in the right places.

    Or watch the webinar: ["Projects in RStudio"](https://resources.rstudio.com/wistia-rstudio-essentials-2/rstudioessentialsmanagingpart1-2){target="_blank"}

9. [Learn the basic dplyr verbs](https://r4ds.had.co.nz/transform.html){target="_blank"} for data manipulation (*r4ds*) -- Concentrate on the main verbs: **`filter()`** (rows), **`select()`** (columns), **`mutate()`**, **`arrange()`** (rows), **`group_by()`**, and **`summarize()`**. Learn the pipe **`%>%`** operator.

10. Know how to [tidy your data](https://github.com/jtr13/codehelp/blob/master/R/gather.md){target="_blank"} -- The **`gather()`** function from the **tidyr** package will help you get your data in the right form for plotting.  More on this in class. Check out these [super cool animations](https://github.com/gadenbuie/tidyexplain){target="_blank"}, which follow a data frame as it is transformed by `tidyr` functions.

**General advice**: don't get caught up in the details.  Keep a list of questions and move on. 

## Tips & Tricks

### Knitr

Up your game with chunk options: check out the [official list of options](https://yihui.name/knitr/options/){target="_blank"} -- and bookmark it!

Some favorites are:

`warning=FALSE`  

`message=FALSE` -- especially useful when loading packages  

`cache=TRUE` -- only changed chunks will be evaluated, be careful though since changes in dependencies will not be detected.

`fig.`... options, [see below](#sizing-figures-and-more)

### RStudio keyboard shortcuts

- **option-command-i**  ("insert R chunk")


````
```{r}
```
````

- **shift-command-M**  `%>%`   ("the pipe") 

### Sizing figures (and more)

Always use chunk options to size figures.  You can set a default size in the YAML at the beginning of the .Rmd file as so:

```
output: 
  pdf_document: 
    fig_height: 3
    fig_width: 5
```

Another method is to click the gear ⚙️ next to the Knit button, then  **Output Options...**, and finally the **Figures** tab.

Then as needed override one or more defaults in particular chunks:

`{r, fig.width=4, fig.height=2}`


Figure related chunk options include `fig.width`, `fig.height`, `fig.asp`, and `fig.align`; there are [many more](https://yihui.name/knitr/options/#plots){target="_blank"}.

### Viewing plots in plot window

Would you like your plots to appear in the plot window instead of below each chunk in the `.Rmd` file? Click ⚙️ and then  <i class="fas fa-check"></i> **Chunk Output in Console**. 





## Submitting Assignments

Here's a quick run-down of how to submit your assignments using [R Markdown and Knitr](#r-markdown-knitr).

- **Create R Markdown file with PDF output format**: We will often provide you with a template, and feel free to add on to it directly, but **make sure its output format is set to `pdf_document`**. Write out your explanations and insert code chunks to answer the questions provided. If you want to make a new file, go to *File > New File > R Markdown...* and set the `Default Output Format` to `PDF`. Either way, the header of the `.Rmd` file should look something like this:

![](images/markdown_meta_header_example.png)

- **Add PDF Dependencies**: As stated when you create a new R Markdown file, the PDF output format requires TeX:

![](images/pdf_output_info.png)

- Make sure you download TeX for your machine. Here are some Medium articles on the process of creating PDF reports (the articles cover starting from scratch with no installs at all, but you can skip over to installing TeX only):
    + [**Mac OS**](https://medium.com/@sorenlind/create-pdf-reports-using-r-r-markdown-latex-and-knitr-on-macos-high-sierra-e7b5705c9fd){target="_blank"}
    + [**Windows**](https://medium.com/@sorenlind/create-pdf-reports-using-r-r-markdown-latex-and-knitr-on-windows-10-952b0c48bfa9){target="_blank"}

This can be a little complicated, but it will make that Knit button near the top of the IDE magically generate a PDF for you. 

If you are in a rush and want a shortcut, you can instead set the `Default Output Format` to `HTML`. When you open the file in your browser, you can save it as a PDF. It will not be as nicely formatted, but it will still work.

## Getting help

![](images/halp_me_orly.png)
*via [https://dev.to/rly](https://dev.to/rly){target="_blank"}*

First off...breeeeeeathe. We can fix this. There are a bunch of resources out there that can help you.

### Things to try

* Remember: Always try to help yourself! [This article](https://www.r-project.org/help.html){target="_blank"} has a great list of tools to help you learn about anything you may be confused by. This includes learning about functions and packages as well as searching for info about a function/package/problem/etc. This is the perfect place to learn how to get the info you need.

* The RStudio Help menu (in the top toolbar) is a fantastic place to go for understanding/fixing any problems. There are links to documentation and manuals as well as cheatsheets and a lovely collection of keyboard shortcuts.

* Vignettes are a great way to learn about packages and how they work. Vignettes are like stylized manuals that can do a better job at explaining a package's contents. For example, `ggplot2` has a vignette on aesthetics called `ggplot2-specs` that talks about different ways you can map data to different formats.
    + Typing `browseVignettes()` in the console will show you all the vignettes for all of the packages you have installed.
    + You can also see vignettes by package by typing `vignette(package = "<package_name>")` into the console.
    + To run a specific vignette, use `vignette("<vignette_name>")`. If the vignette can't be resolved, include the package name as well: `vignette("<vignette_name", package = "<package_name>")`
    
* Don't ignore errors. They are telling you so much! If you give up because red text showed up in your console, take the time to see what that red text is *saying*. Learn [how to read errors](http://www.dummies.com/programming/r/how-to-read-errors-and-warnings-in-r/){target="_blank"} and [what they are telling you](https://campus.datacamp.com/courses/working-with-the-rstudio-ide-part-1/programming?ex=18){target="_blank"}. They usually include where the problem happened and what R thinks the problem stems from. 

*More Advanced*: Learn to love debugger mode. Debugging can have a steep learning curve, but huge payoffs. Take a look at these videos about [debugging with R](https://campus.datacamp.com/courses/working-with-the-rstudio-ide-part-1/programming?ex=20){target="_blank"}. Topics include running the debugger, setting breakpoints, customizing preferences, and more. **Note**: R Markdown files have some limitations for debugging, as discussed in [this article](https://support.rstudio.com/hc/en-us/articles/205612627-Debugging-with-RStudio#debugging-in-r-markdown-documents){target="_blank"}. You could also consider working out your code in a `.R` file before including it in your R Markdown homework submission.

### Help me, R community!

Relax. There are a bunch of people using the same tools you are.

* Your fellow classmates are a good place to start! Post questions to [Piazza](https://piazza.com/){target="_blank"} to see how they could help.

* There is a lot of great documentation on R and its functions/packages/etc. Get comfy with [R Documentation](https://www.rdocumentation.org/){target="_blank"} and it will help you immensely.

* There is a vibrant [RStudio Community page](https://community.rstudio.com/){target="_blank"}. Also, R likes twitter. Check out [#rstats](https://twitter.com/search?q=%23rstats){target="_blank"} or maybe [let Hadley Wickham know about a wonky error message](https://twitter.com/hadleywickham/status/952259891342794752){target="_blank"}.











