Lab 6: More data wrangling and outputs
================
Gaston Sanchez

> ### Learning Objectives:
>
> -   Work with a more complex file structure
> -   Practice exporting tables
> -   Practice exporting R output (as is)
> -   Practice exporting plot images
> -   Learn about `"dplyr"` pipelines
> -   Get to know the pipe operator `%>%`
> -   Chain dplyr operations with the piper

------------------------------------------------------------------------

Manipulating and Visualizing Data Frames
----------------------------------------

In this lab, you will continue manipulating data frames with `"dplyr"`, and plotting graphics with `"ggplot2"`. In addition, you will also use various functions to export (or save) tables, images, and R output to external files.

While you follow this lab, you may want to open these cheat sheets:

-   [dplyr cheatsheet](../cheatsheets/data-transformation-cheatsheet.pdf)
-   [ggplot2 cheatsheet](../cheatsheets/ggplot2-cheatsheet-2.1.pdf)

### Filestructure

To help you better prepare for HW02, we want you to practice working with a more sophisticated file structure. Follow the steps listed below to create the necessary subdirectories like those depicted in this scheme:

        lab06/
           README.md
           data/
           code/
           output/
           images/

-   Open a shell terminal (e.g. command line or GitBash)
-   Change your working directory to a location where you will store all the materials for this lab
-   Use `mkdir` to create a directory `lab06`
-   `cd` to `lab06`
-   Use `mkdir` to create other subdirectories: `data`, `code`, `output`, `images`
-   List the contents of `lab06` to confirm that you have all the subdirectories
-   Use `touch` to create an empty `README.md` text file.
-   Open the `README.md` file with a text editor (e.g. the one in RStudio) and add a brief description of what this lab is about. Save the changes.
-   `cd` to `data/`
-   Download the data file with the command `curl`, and the `-O` option (letter O)

    ``` bash
    curl -O https://raw.githubusercontent.com/ucb-stat133/stat133-spring-2018/master/data/nba2017-players.csv
    ```

-   Use `ls` to confirm that the csv file is in `data/`
-   Use *word count* `wc` to count the lines of the csv file
-   Take a peek at the first rows of the csv file with `head`
-   Take a peek at the last 5 rows of the csv file with `tail`

------------------------------------------------------------------------

R script file
=============

-   Once you have the filestructure for this lab, go to RStudio and open a new `R` script file (do NOT confuse with an `Rmd` file).
-   Save the `R` script file as `lab06-script.R` in the `code/` folder of `lab06/`

R script files are used to write R code only, using R syntax. In other words, you should NOT use Markdown or LaTeX syntax inside an R script file. Why? Because if you run the entire script, R will try to execute all the commands, and won't be able to recognize Markdown, LaTeX, yaml, or other syntaxes.

File Header
-----------

Let's start with some good coding practices by adding a header to the `R` script file in the form of R comments. In general, the header section should contain a title, a description of what the script is about, what are the inputs, and what are the main outputs produced when executing the code in the script. Optionally, you can also include the name of the author, the date, and other details. Something like this:

    # Title: Short title (one sentence)
    # Description: what the script is about (one paragraph or two)
    # Input(s): what are the main inputs (list of inputs)
    # Output(s): what are the main outputs (list of outputs)
    # Author(s): First Last
    # Date: mm-dd-yyyy

Think of the header of a script file as the yaml header used in `Rmd` files. The header should be the very first thing that appears at the top of the script file. Personally, I like to surround the header in my R script files with some delimiting characters that help the reader to visually identify main parts of the script. Here's a hypothetical example of a header:

``` r
# ===================================================================
# Title: Cleaning Data
# Description:
#   This script performs cleaning tasks and transformations on 
#   various columns of the raw data file.
# Input(s): data file 'raw-data.csv'
# Output(s): data file 'clean-data.csv'
# Author: Gaston Sanchez
# Date: 2-27-2018
# ===================================================================
```

Another good coding practice is to avoid writing very long lines of code. Most coding style guides stick to a maximum line width of 80 characters, and this is the [magic number](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) that I also use for my scripts.

**Your turn**: Include a header in your R script file, respecting the width limit of 80 characters. Save this file in the `code/` folder.

Required Packages
-----------------

The next thing that you need to include in your script file are the required packages. Not all script files need packages, but many do. When this is the case, loading the packages should be the first lines of code to be executed.

Include the commands to load the following packages in your script:

``` r
# packages
library(readr)    # importing data
library(dplyr)    # data wrangling
library(ggplot2)  # graphics
```

In addition to loading the packages, sometimes you will also need to load code from other script files. We won't do that today, but you should know that this is very common as the complexity and size of your projects grow.

Exporting some data tables
--------------------------

After the header, and the loading-packages sections, the next part in your lab script involves importing the data. In addition to importing a data table, you are also going to practice exporting tables. That is, writing data tables to external files.

-   We want you to work with relative paths. To execute the commands that read input(s) and write output(s), you will need to set the working directory. On the menu bar go to **Session**, then select the option **Set Working Directory**, and choose **To Source File Location**.
-   Use `read_csv()` from the package `"readr"` to import the data `nba2017-players.csv` in R. Do this by specifying a relative path.
-   Use the imported tibble to create a data frame `warriors` by selecting rows---e.g. `filter()`---of Golden State Warriors, arranging rows by salary in increasing order.
-   Use the function `write.csv()` to export (or save) the data frame `warriors` to a data file `warriors.csv` in the `output/` directory. You will need to use a relative path to specify the `file` argument. Also, see how to use the argument `row.names` to avoid including a first column of numbers.
-   Create another data frame `lakers` by selecting rows of Los Angeles Lakers, this time arranging rows by experience (decreasingly).
-   Now use the function `write_csv()` to export (or save) the data frame `lakers` to a data file `lakers.csv` in the `folder/` directory. You will also need to use a relative path to specify the `file` argument.
-   Inspect the contents of the `data/` folder and confirm that the csv files are there.

Exporting some R output
-----------------------

After exporting the tables to the corresponding csv files, you will produce some summary statistics, and then save the generated output to external text files. To do this, you will have to learn about the `sink()` function, which sends R output to a specified file.

Say you are interested in exporting the summary statistics of `height` and `weight`, exactly in the same way they are displayed by R:

``` r
summary(dat[ ,c('height', 'weight')])
```

    ##      height          weight     
    ##  Min.   :69.00   Min.   :150.0  
    ##  1st Qu.:77.00   1st Qu.:200.0  
    ##  Median :79.00   Median :220.0  
    ##  Mean   :79.15   Mean   :220.2  
    ##  3rd Qu.:82.00   3rd Qu.:240.0  
    ##  Max.   :87.00   Max.   :290.0

One naive option to "export" this output would be to manually copy the text displayed on the console, and then paste it to a text file. While this may work, it is labor intensive, error prone, and highly irreproducible. A better way to achieve this task is with the `sink()` function. Here's how:

``` r
# divert output to the specified file
sink(file = '../output/summary-height-weight.txt')
summary(dat[ ,c('height', 'weight')])
sink()
```

The fist call to `sink()` opens a connection to the specified file, and then all outputs are diverted to that location. The second call to `sink()`, i.e. the one without any arguments, closes the connection.

While you are `sink()`ing output to a specified file, all the results will be sent to such file. In other words, nothing will be printed on the console. Only after the sinking process has finished and the connection is closed, you will be able to execute commands and see results displayed on R's console.

### Why sinking?

Why would you ever want to `sink()` R outputs to a file? Why not simply display them as part of your `Rmd` file? One good reason for diverting output to an external file is for convenience. In practice, the reports and documents (e.g. papers, executive summaries, slides) of a data analysis project won't contain everything that you tried, explored, analyzed, and graphed. There will be many intermediate results that, while relevant for a specific stage of the analysis cycle, are innecessary for the final report. So a good way to keep these intermediate outputs is by exporting them with `sink()`.

**Your turn:**

-   Export the output of `str()` on the data frame with all the players. `sink()` the output, using a relative path, to a text file `data-structure.txt`, in the `output/` folder.
-   Export the `summary()` of the entire data frame `warriors` to a text file `summary-warriors.txt`, in the `output/` folder (also use a relative path).
-   Export another `summary()` of the entire data frame `lakers` to a text file `summary-lakers.txt`, in the `output/` folder (using a relative path).

Exporting some "base" graphs
----------------------------

In the same way that R output, as it appears on the console, can be exported to some files, you can do the same with graphics and plots. Actually, saving plot images is much more common than `sink()`ing output.

Base R provides a wide array of functions to save images in most common formats:

-   `png()`
-   `jpeg()`
-   `tiff()`
-   `bmp()`
-   `svg()`
-   `pdf()`

Similar to the writing table functions such as `write.table()` or `write.csv()`, and the `sink()` function, the graphics device functions require a file name to be provided. Here's how to save a simple scatterpot of `height` and `weight` in png format to the folder `images/`:

``` r
# saving a scatterplot in png format
png(filename = "../images/scatterplot-height-weight.png")
plot(dat$height, dat$weight, pch = 20, 
     xlab = 'Height', ylab = 'Height')
dev.off()
```

-   The function `png()` tells R to save the image in PNG format, using the provided filename.
-   Invoking `png()` will open a graphics device; not the graphics device of RStudio, so you won't be able to see the graphic.
-   The `plot()` function produces the scatterplot.
-   The function `dev.off()` closes the graphics device.

**Your turn**:

-   Open the help documentation of `png()` and related graphic devices.
-   Use `png()` to save a scatterplot of `height` and `weight` with `plot()`. Save the graph in the `images/` folder.
-   Save another version of the scatterplot between `height` and `weight`, but now try to get an image with higher resolution. Save the plot in `images/`.
-   Save a histogram in JPEG format of `age` with dimensions (width x height) 600 x 400 pixels. Save the plot in `images/`.
-   Use `pdf()` to save the previous histogram of `age` in PDF format, with dimensions (width x height) 7 x 5 inches.

Exporting some ggplots
----------------------

The package `"ggplot2"` comes with a wrapper function `ggsave()` that allows you to save ggplot graphics to a specified file. By default, `ggsave()` saves images in PDF format.

-   Use `ggplot()` to make a scatterplot of `points` and `salary`, and store it in a ggplot object named `gg_pts_salary`. Then use `ggsave()` to save the plot with dimensions (width x height) 7 x 5 inches; in the `images/` folder as `points_salary.pdf`

-   Use `ggplot()` to create a scatterplot of `height` and `weight`, faceting by `position`. Store this in a ggplot object `gg_ht_wt_positions` Then use `ggsave()` to save the plot with dimensions (width x height) 6 x 4 inches; in the `images/` folder as `height_weight_by_position.pdf`

------------------------------------------------------------------------

More `"dplyr"`
==============

The last part of this lab involves working the pipe operator `%>%` which allows you write function calls in a more human-readable way. This becomes extremely useful in `"dplyr"` operations that require many steps.

The behavior of `"dplyr"` is functional in the sense that function calls don't have side-effects. You must always save their results in order to keep them in an object (in memory). This doesn't lead to particularly elegant code, especially if you want to do many operations at once. You either have to do it step-by-step:

``` r
# manipulation step-by-step
dat1 <- group_by(dat, team)
dat2 <- select(dat1, team, height, weight)
dat3 <- summarise(dat2,
  avg_height = mean(height, na.rm = TRUE),
  avg_weight = mean(weight, na.rm = TRUE))
dat4 <- arrange(dat3, avg_height)
dat4
```

    ## # A tibble: 30 x 3
    ##    team  avg_height avg_weight
    ##    <chr>      <dbl>      <dbl>
    ##  1 BOS         78.2        220
    ##  2 HOU         78.3        215
    ##  3 SAC         78.5        217
    ##  4 IND         78.5        226
    ##  5 CHI         78.5        216
    ##  6 PHO         78.5        214
    ##  7 BRK         78.7        222
    ##  8 CHO         78.8        213
    ##  9 LAC         78.8        225
    ## 10 CLE         78.9        226
    ## # ... with 20 more rows

Or if you don't want to name the intermediate results, you need to wrap the function calls inside each other:

``` r
# inside-out style (hard to read)
arrange(
  summarise(
    select(
      group_by(dat, team),
      team, height, weight
    ),
    avg_height = mean(height, na.rm = TRUE),
    avg_weight = mean(weight, na.rm = TRUE)    
  ),
  avg_height
)
```

    ## # A tibble: 30 x 3
    ##    team  avg_height avg_weight
    ##    <chr>      <dbl>      <dbl>
    ##  1 BOS         78.2        220
    ##  2 HOU         78.3        215
    ##  3 SAC         78.5        217
    ##  4 IND         78.5        226
    ##  5 CHI         78.5        216
    ##  6 PHO         78.5        214
    ##  7 BRK         78.7        222
    ##  8 CHO         78.8        213
    ##  9 LAC         78.8        225
    ## 10 CLE         78.9        226
    ## # ... with 20 more rows

This is difficult to read because the order of the operations is from inside to out. Thus, the arguments are a long way away from the function. To get around this problem, `"dplyr"` provides the `%>%` operator from `"magrittr"`.

`x %>% f(y)` turns into `f(x, y)` so you can use it to rewrite multiple operations that you can read left-to-right, top-to-bottom:

``` r
# using %>%
dat %>% 
  group_by(team) %>%
  select(team, height, weight) %>%
  summarise(
    avg_height = mean(height, na.rm = TRUE),
    avg_weight = mean(weight, na.rm = TRUE)) %>%
  arrange(avg_height)
```

    ## # A tibble: 30 x 3
    ##    team  avg_height avg_weight
    ##    <chr>      <dbl>      <dbl>
    ##  1 BOS         78.2        220
    ##  2 HOU         78.3        215
    ##  3 SAC         78.5        217
    ##  4 IND         78.5        226
    ##  5 CHI         78.5        216
    ##  6 PHO         78.5        214
    ##  7 BRK         78.7        222
    ##  8 CHO         78.8        213
    ##  9 LAC         78.8        225
    ## 10 CLE         78.9        226
    ## # ... with 20 more rows

Use the piper operator `"%>%"` to perform the following operations:

-   display the player names of Lakers `'LAL'`.

-   display the name and salary of GSW point guards 'PG'.

-   dislay the name, age, and team, of players with more than 10 years of experience, making 10 million dollars or less.

-   select the name, team, height, and weight, of rookie players, 20 years old, displaying only the first five occurrences (i.e. rows).

-   create a data frame `gsw_mpg` of GSW players, that contains variables for `player` name, `experience`, and `min_per_game` (minutes per game), sorted by `min_per_game` (in descending order).

-   display the average triple points by team, in ascending order, of the bottom-5 teams (worst 3pointer teams).

-   obtain the mean and standard deviation of `age`, for Power Forwards, with 5 and 10 years (including) of experience.
