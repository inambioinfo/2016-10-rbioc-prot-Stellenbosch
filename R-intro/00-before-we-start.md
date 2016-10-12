---
layout: topic
title: Before we start
author: Data Carpentry contributors
minutes: 15
---



------------

> ## Learning Objectives
>
> * Articulating motivations for this lesson
> * Introduce participants to the RStudio interface
> * Set up participants to have a working directory with a `data/` folder inside
> * Introduce R syntax
> * Point to relevant information on how to get help, and understand how to ask well formulated questions

------------

# Before we get started

* Start RStudio (presentation of RStudio -below- should happen here)
* Under the `File` menu, click on `New project`, choose `New directory`, then
  `Empty project`
* Enter a name for this new folder, and choose a convenient location for
  it. This will be your **working directory** for the rest of the day
  (e.g., `~/r-intro`)
* Click on "Create project"
* Under the `Files` tab on the right of the screen, click on `New Folder` and
  create a folder named `data` within your newly created working directory.
  (e.g., `~/r-intro/data`)
* Create a new R script (`File > New File > R script`) and save it in your working
  directory (e.g. `r-intro-script.R`)

Your working directory should now look like this:

![How it should look like at the beginning of this lesson](img/r_starting_how_it_should_like.png)

# Presentation of RStudio

Let's start by learning about our tool.

* Console, Scripts, Environments, Plots
* Code and workflow are more reproducible if we can document everything that we
  do.
* Our end goal is not just to "do stuff" but to do it in a way that anyone can
  easily and exactly replicate our workflow and results.

# Interacting with R

There are two main ways of interacting with R: using the **console** or by using
**script files** (plain text files that contain your code).

The **console window** (in RStudio, the bottom left panel) is the place where R is
waiting for you to tell it what to do, and where it will show the results of a
command.  You can type commands directly into the console, but **they will be
forgotten** when you close the session. It is better to enter the commands in the
**script editor**, and **save the script**. This way, you have a complete record of what
you did, you can easily show others how you did it and you can do it again later
on if needed. You can copy-paste into the R console, but the Rstudio script
editor allows you to 'send' the current line or the currently selected text to
the R console using the `Ctrl-Enter` shortcut.

At some point in your analysis you may want to check the content of **variable** or
the structure of an object, without necessarily keep a record of it in your
script. You can type these commands directly in the console. RStudio provides
the `Ctrl-1` and `Ctrl-2` shortcuts allow you to jump between the script and the
console windows.

If R is ready to accept commands, the R console shows a `>` **prompt**. If it
receives a command (by typing, copy-pasting or sent from the script editor using
`Ctrl-Enter`), R will try to execute it, and when ready, show the results and
come back with a new `>` prompt to wait for new commands.

If R is still **waiting** for you to enter more data because it isn't
complete yet, the console will show a `+` **prompt**. It means that
you haven't finished entering a complete command. This is because you
have not 'closed' a parenthesis or quotation. If you're in Rstudio and
this happens, click inside the console window and press `Esc`; this
should help you out of trouble.

# Basics of R

R is a versatile, open source programming/scripting language that's useful both
for statistics but also data science. Inspired by the programming language S.

* **Open source** software under GPL.
* Superior (if not just comparable) to commercial alternatives. R has over 7,000
  user contributed **packages** at this time. It's widely used both in **academia and
  industry**.
* Available on **all platforms**.
* Not just for **statistics**, but also **general purpose programming**.
* For people who have experience in programmming: R is both an **object-oriented**
  and a so-called [**functional language**](http://adv-r.had.co.nz/Functional-programming.html)
* Large and growing **community** of peers.

## Commenting

Use `#` signs to comment. Comment liberally in your R scripts. Anything to the
right of a `#` is ignored by R.

## Assignment operator

`<-` is the assignment operator. It assigns values on the right to objects on
the left. So, after executing `x <- 3`, the value of `x` is `3`. The arrow can
be read as 3 **goes into** `x`.  You can also use `=` for assignments but not in
all contexts so it is good practice to use `<-` for assignments. `=` should only
be used to specify the values of arguments in functions, see below.

In RStudio, typing `Alt + -` (push `Alt`, the key next to your space bar at the
same time as the `-` key) will write ` <- ` in a single keystroke.

## Functions and their arguments

Functions are "canned scripts" that automate something complicated or convenient
or both.  Many functions are predefined, or become available when using the
function `library()` (more on that later). A function usually gets one or more
inputs called **arguments**. Functions often (but not always) return a **value**. A
typical example would be the function `sqrt()`. The input (the argument) must be
a number, and the return value (in fact, the output) is the square root of that
number. Executing a function ('running it') is called **calling** the function. An
example of a function call is:

`b <- sqrt(a)`

Here, the value of the variable `a` is given to the `sqrt()` function,
the `sqrt()` function calculates the square root, and returns the
value which is then assigned to variable `b`. This function is very
simple, because it takes just one argument.

The return 'value' of a function need not be numerical (like that of `sqrt()`),
and it also does not need to be a single item: it can be a set of things, or
even a data set. We'll see that when we read data files in to R.

Arguments can be anything, not only numbers or filenames, but also other
objects. Exactly what each argument means differs per function, and must be
looked up in the documentation (see below). If an argument alters the way the
function operates, such as whether to ignore 'bad values', such an argument is
sometimes called an **option**.

Most functions can take several arguments, but many have so-called **defaults**.
If you don't specify such an argument when calling the function, the function
itself will fall back on using the **default**. This is a standard value that the
author of the function specified as being "good enough in standard cases". An
example would be what symbol to use in a plot. However, if you want something
specific, simply change the argument yourself with a value of your choice.

Let's try a function that can take multiple arguments `round`.


```r
round(3.14159)
```

```
## [1] 3
```

We can see that we get `3`. That's because the default is to round
to the nearest whole number. If we want more digits we can see
how to do that by getting information about the `round` function.
We can use `args(round)` or look at the
help for this function using `?round`.


```r
args(round)
```

```
## function (x, digits = 0) 
## NULL
```


```r
?round
```


We see that if we want a different number of digits, we can
type `digits=2` or however many we want.


```r
round(3.14159, digits=2)
```

```
## [1] 3.14
```

If you provide the arguments in the exact same **order** as they are defined you don't have to name them:


```r
round(3.14159, 2)
```

```
## [1] 3.14
```

However, it's usually not recommended practice because it's a lot of remembering
to do, and if you share your code with others that includes less known functions
it makes your code difficult to read. (It's however OK to not include the names
of the arguments for basic functions like `mean`, `min`, etc...)

Another advantage of **naming arguments**, is that the **order doesn't matter**.
This is useful when there start to be more arguments.


## Organizing your working directory

You should separate the original data (raw data) from intermediate datasets that
you may create for the need of a particular analysis. For instance, you may want
to create a `data/` directory within your working directory that stores the raw
data, and have a `data_output/` directory for intermediate datasets and a
`figure_output/` directory for the plots you will generate.


# Seeking help

# I know the name of the function I want to use, but I'm not sure how to use it

If you need help with a specific function, let's say `barplot()`, you can type:


```r
?barplot
```

If you just need to remind yourself of the names of the arguments, you can use:


```r
args(lm)
```

If the function is part of a package that is installed on your computer but
don't remember which one, you can type:


```r
??geom_point
```

# I want to use a function that does X, there must be a function for it but I don't know which one...

If you are looking for a function to do a particular task, you can use
`help.search()` (but only looks through the installed packages):


```r
help.search("kruskal")
```

If you can't find what you are looking for, you can use the
[rdocumention.org](http://www.rdocumentation.org) website that search through
the help files across all packages available.

## I am stuck... I get an error message that I don't understand

Start by googling the error message. However, this doesn't always work very well
because often, package developers rely on the error catching provided by R. You
end up with general error messages that might not be very helpful to diagnose a
problem (e.g. "subscript out of bounds").

However, you should check stackoverflow. Search using the `[r]` tag. Most
questions have already been answered, but the challenge is to use the right
words in the search to find the answers:
[http://stackoverflow.com/questions/tagged/r](http://stackoverflow.com/questions/tagged/r)

The [Introduction to R](http://cran.r-project.org/doc/manuals/R-intro.pdf) can
also be dense for people with little programming experience but it is a good
place to understand the underpinnings of the R language.

The [R FAQ](http://cran.r-project.org/doc/FAQ/R-FAQ.html) is dense and technical
but it is full of useful information.

## Asking for help

The key to get help from someone is for them to grasp your problem rapidly. **You
should make it as easy as possible to pinpoint where the issue might be**.

Try to use the correct words to describe your problem. For instance, a `package`
is not the same thing as a `library`. Most people will understand what you meant,
but others have really strong feelings about the difference in meaning. The key
point is that it can make things confusing for people trying to help you. Be as
**precise** as possible when describing your problem

If possible, try to reduce what doesn't work to a **simple reproducible
example**. If you can reproduce the problem using a very small `data.frame`
instead of your 50,000 rows and 10,000 columns one, provide the small one with
the description of your problem. When appropriate, try to generalize what you
are doing so even people who are not in your field can understand the question.

To **share** data (an object) with someone else, if it's relatively small, you can use the
function `dput()`. It will output R code that can be used to recreate the exact same
object as the one in memory:


```r
dput(head(iris)) # iris is an example data.frame that comes with R
```

```
## structure(list(Sepal.Length = c(5.1, 4.9, 4.7, 4.6, 5, 5.4), 
##     Sepal.Width = c(3.5, 3, 3.2, 3.1, 3.6, 3.9), Petal.Length = c(1.4, 
##     1.4, 1.3, 1.5, 1.4, 1.7), Petal.Width = c(0.2, 0.2, 0.2, 
##     0.2, 0.2, 0.4), Species = structure(c(1L, 1L, 1L, 1L, 1L, 
##     1L), .Label = c("setosa", "versicolor", "virginica"), class = "factor")), .Names = c("Sepal.Length", 
## "Sepal.Width", "Petal.Length", "Petal.Width", "Species"), row.names = c(NA, 
## 6L), class = "data.frame")
```

If the object is larger, provide either the raw file (i.e., your CSV file) with
your script up to the point of the error (and after removing everything that is
not relevant to your issue). Alternatively, in particular if your questions is
not related to a `data.frame`, you can save any R object to a file:


```r
saveRDS(iris, file="~/tmp/iris.rds")
```

The content of this file is however not human readable and cannot be posted
directly on stackoverflow. It can however be sent to someone by email who can read
it with this command:


```r
some_data <- readRDS(file="~/tmp/iris.rds")
```

Last, but certainly not least, **always include the output of `sessionInfo()`**
as it provides critical information about your platform, the versions of R and
the packages that you are using, and other information that can be very helpful
to understand your problem.


```r
sessionInfo()
```

```
## R version 3.3.1 Patched (2016-08-02 r71022)
## Platform: x86_64-pc-linux-gnu (64-bit)
## Running under: Ubuntu 14.04.5 LTS
## 
## locale:
##  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_GB.UTF-8        LC_COLLATE=en_GB.UTF-8    
##  [5] LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
##  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  base     
## 
## other attached packages:
## [1] knitr_1.14
## 
## loaded via a namespace (and not attached):
## [1] magrittr_1.5  formatR_1.4   tools_3.3.1   msdata_0.12.3 stringi_1.1.2
## [6] methods_3.3.1 stringr_1.1.0 evaluate_0.9
```

## Where to ask for help?

* Your friendly colleagues: if you know someone with more experience than you,
  they might be able and willing to help you.
* Stackoverlow: if your question hasn't been answered before and is well
  crafted, chances are you will get an answer in less than 5 min.
* The [R-help](https://stat.ethz.ch/mailman/listinfo/r-help): it is read by a
  lot of people (including most of the R core team), a lot of people post to it,
  but the tone can be pretty dry, and it is not always very welcoming to new
  users. If your question is valid, you are likely to get an answer very fast
  but don't expect that it will come with smiley faces. Also, here more than
  everywhere else, be sure to use correct vocabulary (otherwise you might get an
  answer pointing to the misuse of your words rather than answering your
  question). You will also have more success if your question is about a base
  function rather than a specific package.
* If your question is about a specific package, see if there is a mailing list
  for it. Usually it's included in the DESCRIPTION file of the package that can
  be accessed using `packageDescription("name-of-package")`. You may also want
  to try to email the author of the package directly.
* There are also some topic-specific mailing lists (GIS, phylogenetics, etc...),
  the complete list is [here](http://www.r-project.org/mail.html).

# More resources

* My [teaching material](http://lgatto.github.io/TeachingMaterial) repository
* The [Posting Guide](http://www.r-project.org/posting-guide.html) for the R
  mailing lists.
* [How to ask for R help](http://blog.revolutionanalytics.com/2014/01/how-to-ask-for-r-help.html)
  useful guidelin`