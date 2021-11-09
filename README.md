## hw3

**Due**: 11/16, upload to GitHub individual repo.

Please read these 2 papers.

1. [Is Most Published Research Really False?](https://github.com/bms5213-F2021/bms5213-F2021.github.io/raw/master/docs/resourcedev/papers/metaanalysis.pdf)
2. [Many Analysts, One Data Set: Making Transparent How Variations in Analytic Choices Affect Results](http://econweb.umd.edu/~pope/crowdsourcing_paper.pdf)

### 1. Create an R Markdown file in RStudio

The first paper attributes the lack of good tools for performing data analyses as a roadblock for reproducible and replicable research.

> One of the key impediments to performing reproducible and replicable research in the past was the
lack of availability of tools that make it easy to share data, share code, and perform correct analyses.

The first paper also mentions R Markdown as a tool that is overcoming this issue. Therefore, for this homework, I would like everyone to use an R Markdown file for their text responses, their code, and graphs.  

I will walk you through some basics in the homework, but [this resource](https://r4ds.had.co.nz/r-markdown.html) has great notes on using R Markdown. Also, the [RStudio website](https://rmarkdown.rstudio.com/lesson-1.html) has some cool info. Please feel free to consult either.

You may need to install two packages in R before you can do this. `rmakrdown` and `knitr`. Try the following in the console area of RStudio (bottom left panel):

```
install.packages(c("rmarkdown", "knitr"))
```

If you have any concerns whether these packages installed correctly, please email the instructor. If they did install correctly, you should see something like `* DONE (rmarkdown)` and `* DONE (knitr)` in the console; and, at least with `rmarkdown`, you'll be able to create an R markdown file with the next steps.

Load the packages.

```
library(rmarkdown)
library(knitr)
```

In RStudio by going to File -> New File -> R Markdown, create an R Markdown file. Title the document as you see appropriate; go ahead and keep html as the output. Hit OK in the bottom right corner.

This will create a new document that has what's called a YAML header surrounded by `---`s, various R code chunks surrounded by three ``` (backticks), and text with headers and body formatting.

##### RMarkdown syntax primer

Headers have `#` and a space before the text of the header. More than one pound sign, like `##`, lead to smaller headers.

`*Word*` will lead to *Word*, where Word or whatever is between the asterisks is italicized.


`**Word**` will lead to **Word** or bold.

Bulleted lists can be made using the following:

```
* Item 1
* Item B
* Item 3
```

and will look like:

* Item 1
* Item B
* Item 3

Numbered lists can be made in a similar way:

```
1. Item 1
2. Item B
3. Item 3
```

which will look like:

1. Item 1
2. Item B
3. Item 3

Use the triple backticks for code blocks, and single backticks for inline code. See [here](https://rmarkdown.rstudio.com/lesson-3.html) and [here](https://rmarkdown.rstudio.com/lesson-4.html) respectively. If you want it to be R code that is run/evaluated, not just displayed, you'll include `{r}` next to the triple backticks, like in the R chunks in the created RMarkdown file.

One really cool thing about R code chunks is that they should have a green arrow in the top right corner of the chunk that allows you to run the R code and it'll display any results as you go.

You can link to websites in R Markdown as well

```
[displayed text](url)
```

and then the "displayed text" will be linked to the "url".

Keep the YAML header and the first R chunk (and it's surrounding backticks).

```
{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Feel free to read everything else but then remove it.

Use this RMarkdown file for the rest of the homework please, including text descriptions/answers.

Save the RMarkdown file and make sure you know which folder it's in (this will be important later).

### 2. Please describe the two papers and your reaction to them

(Please try to use header and text formatting markdown syntax in your responses in this section)

* Summarize what the papers are trying to communicate.
* Do you find Table 1 in the first paper helpful for you, especially with your upcoming projects? Do you think that the recommended strategies for improvement are 1) good or 2) enough given the described problems and what you have encountered in this course so far?
* What lessons did you learn from each paper?
* Do you find it surprising that given the same data and study question, teams comprised of (what might be considered) experts in their fields (pg 341, Stage 2 description) would make different analytic choices and reach different conclusions? Why or why not?

### 3. Draw a graphic comparing reproducibility and replicability.

Export or save a png of the graphic to the same folder that your R Markdown file is saved in. Also upload it to your homework repo.

Then link the file in the RMarkdown file, using the syntax below:

```
![replicability vs reproducibility](name_of_image.png)
```

where you keep the exclamation point, brackets, and parentheses, but switch out "replicability vs reproducibility" for whatever title you want the graphic to have and "name_of_image.png" for whatever the name of the image/png is.

### 4. Read in data using R

Using an R code chunk, read in the following data from the second paper.

```
 hw_data <- read.csv("https://raw.githubusercontent.com/bms5213-F2021/bms5213-F2021.github.io/master/docs/resourcedev/tidy_data/CrowdstormingDataJuly1st.csv")
```

There are a lot of variables/columns in this dataset. Please look at this document which is called a codebook and explains what each variable is. The codebook can be found [here](https://github.com/bms5213-F2021/bms5213-F2021.github.io/blob/master/docs/resourcedev/tidy_data/README_CrowdstormingDataJuly1st.txt)

### 5. Look at missing data in R.

A. First, with the console in RStudio, go ahead and do `View(hw_data)`. Visually do you see any missing data or NAs? Answer and explain which variables you see missing data for. (This doesn't have to be exhaustive, just a few examples). Do you think this data is missing at random? Why or why not?

B. In the RMarkdown file, figure out how many datapoints there are in this dataset. (consider functions like `dim()` or `nrow()`)

C. In the RMarkdown file, pick a column that you know has missing data, and quantify how many individuals do not have data for that column/variable. (consider functions like `is.na()` and `sum()`)

D. Let's visualize the missing data!

We'll use a heatmap with the [base R `heatmap` function](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/heatmap.html) where the rows are the players and the columns are the variable; and the heatmap will have two colors, one representing the data being present and one representing the data being absent/NA. We can set the colors we want using the `col` argument for the function. I chose black and orange (but you can use whatever colors you want) and set the colors like so: `col = c("Black", "Orange")`

The base R `heatmap` function tries to cluster data and reorders it plus adds on dendrograms. We don't need that for this, so we'll turn off that behavior by setting the `Rowv` and `Colv` arguments of the function to `NA`.

Also, the `heatmap` function wants numeric data, not TRUEs and FALSEs, which is what the `is.na` function returns. But we can easily change TRUEs and FALSEs to 0's and 1's using the `as.integer` function.

```
as.integer(FALSE)
[1] 0
as.integer(TRUE)
[1] 1
```

We can also apply this function to a data frame using the [`apply` function](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/apply). So if I had data frame of TRUEs and FALSEs ( `old_df`) and wanted it to instead be a data frame of 0's and 1's (`new_df`), I would do the following. Note the 2 after old_df is just telling R to look at columns. This is the "margin" argument discussed in the linked website about the function.

```
new_df <- apply(old_df, 2, as.integer)
```

So putting this all together, a heatmap could be made with code like the following where `old_df` and `new_df` are made with the `is.na` and `apply` functions as discussed above. Make sure to do all the steps in the RMarkdown

```
heatmap(new_df, col=c("Black", "Orange"), Rowv = NA, Colv = NA)
```

E. How would you like to handle the missing data? Why?

F. Try out your proposed method in R to subset or "correct" the `hw_data` dataframe. (This is difficult, and emails to the professor or posts on a discussion board are very welcome)

G. If your dataset size changed after step F, what is the new size? (make sure to do this in the RMarkdown)

H. Remake the heatmap with the new/corrected/subset dataframe, do you only see one color now?

### 6. knit the RMarkdown file

Submit both the .Rmd file and the .html file to your Github (as well as the graphic from question 3 if you haven't done so yet).
