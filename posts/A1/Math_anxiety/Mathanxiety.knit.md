---
title: "Mathanxiety"
format: html
---

Math, math, math... for some an absolute nightmare for others the holy grail of existence. Let's break it down

## Data Cleaning

This section handles loading the dataset, cleaning missing values, and initial transformations like converting variables to factors.

Load necessary libraries for data manipulation, visualization, and interactive elements.


::: {.cell}

```{.r .cell-code}
library(tidyverse)
```

::: {.cell-output .cell-output-stderr}

```
── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.4     ✔ readr     2.1.5
✔ forcats   1.0.0     ✔ stringr   1.5.2
✔ ggplot2   4.0.0     ✔ tibble    3.3.0
✔ lubridate 1.9.4     ✔ tidyr     1.3.1
✔ purrr     1.1.0     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


:::

```{.r .cell-code}
library(mosaic)
```

::: {.cell-output .cell-output-stderr}

```
Registered S3 method overwritten by 'mosaic':
  method                           from   
  fortify.SpatialPolygonsDataFrame ggplot2

The 'mosaic' package masks several functions from core packages in order to add 
additional features.  The original behavior of these functions should not be affected by this.

Attaching package: 'mosaic'

The following object is masked from 'package:Matrix':

    mean

The following objects are masked from 'package:dplyr':

    count, do, tally

The following object is masked from 'package:purrr':

    cross

The following object is masked from 'package:ggplot2':

    stat

The following objects are masked from 'package:stats':

    binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
    quantile, sd, t.test, var

The following objects are masked from 'package:base':

    max, mean, min, prod, range, sample, sum
```


:::

```{.r .cell-code}
library(skimr)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'skimr'

The following object is masked from 'package:mosaic':

    n_missing
```


:::

```{.r .cell-code}
library(visdat)
library(naniar)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'naniar'

The following object is masked from 'package:skimr':

    n_complete
```


:::

```{.r .cell-code}
library(janitor)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'janitor'

The following objects are masked from 'package:stats':

    chisq.test, fisher.test
```


:::

```{.r .cell-code}
library(tinytable)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'tinytable'

The following object is masked from 'package:ggplot2':

    theme_void
```


:::

```{.r .cell-code}
library(ggformula)
library(DT)
library(ggridges)
library(shiny)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'shiny'

The following objects are masked from 'package:DT':

    dataTableOutput, renderDataTable
```


:::

```{.r .cell-code}
library(shinydashboard)
```

::: {.cell-output .cell-output-stderr}

```

Attaching package: 'shinydashboard'

The following object is masked from 'package:graphics':

    box
```


:::

```{.r .cell-code}
library(ggridges)
library(corrplot)
```

::: {.cell-output .cell-output-stderr}

```
corrplot 0.95 loaded
```


:::

```{.r .cell-code}
library(TeachHist)
```
:::

Read the CSV file into a data frame, clean column names, and display the initial data.

::: {.cell}

```{.r .cell-code}
meth <- readr::read_delim("MathAnxiety.csv") %>% 
janitor::clean_names(case="snake")
```

::: {.cell-output .cell-output-stderr}

```
Rows: 599 Columns: 6
── Column specification ────────────────────────────────────────────────────────
Delimiter: ";"
chr (2): Gender, Grade
dbl (3): AMAS, RCMAS, Arith
num (1): Age

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


:::

```{.r .cell-code}
meth
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 599 × 6
     age gender grade      amas rcmas arith
   <dbl> <chr>  <chr>     <dbl> <dbl> <dbl>
 1  1378 Boy    Secondary     9    20     6
 2  1407 Boy    Secondary    18     8     6
 3  1379 Girl   Secondary    23    26     5
 4  1428 Girl   Secondary    19    18     7
 5  1356 Boy    Secondary    23    20     1
 6  1350 Girl   Secondary    27    33     1
 7  1336 Boy    Secondary    22    23     4
 8  1393 Boy    Secondary    17    11     7
 9  1317 Girl   Secondary    28    32     2
10  1348 Boy    Secondary    20    30     6
# ℹ 589 more rows
```


:::
:::


::: {.cell}

```{.r .cell-code}
head(meth)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 6
    age gender grade      amas rcmas arith
  <dbl> <chr>  <chr>     <dbl> <dbl> <dbl>
1  1378 Boy    Secondary     9    20     6
2  1407 Boy    Secondary    18     8     6
3  1379 Girl   Secondary    23    26     5
4  1428 Girl   Secondary    19    18     7
5  1356 Boy    Secondary    23    20     1
6  1350 Girl   Secondary    27    33     1
```


:::
:::

<div>

*Data dictionary*

Age: The age of the child in months.

Gender: The gender of the child (Boy or Girl).

Grade: The educational level of the child (Primary or Secondary).

AMAS: The score on the Abbreviated Math Anxiety Scale, where a higher score indicates greater math anxiety.

RCMAS: The score on the Revised Children's Manifest Anxiety Scale, measuring general anxiety.

Arith: The score on an arithmetic test.

</div>

## Inspecting

This section inspects the dataset structure, summaries, counts, and missing values through various diagnostic functions.


::: {.cell}

```{.r .cell-code}
summary(meth)
```

::: {.cell-output .cell-output-stdout}

```
      age          gender             grade                amas      
 Min.   :  37   Length:599         Length:599         Min.   : 4.00  
 1st Qu.:1062   Class :character   Class :character   1st Qu.:18.00  
 Median :1208   Mode  :character   Mode  :character   Median :22.00  
 Mean   :1246                                         Mean   :21.98  
 3rd Qu.:1418                                         3rd Qu.:26.50  
 Max.   :1875                                         Max.   :45.00  
     rcmas           arith      
 Min.   : 1.00   Min.   :0.000  
 1st Qu.:14.00   1st Qu.:4.000  
 Median :19.00   Median :6.000  
 Mean   :19.24   Mean   :5.302  
 3rd Qu.:25.00   3rd Qu.:7.000  
 Max.   :41.00   Max.   :8.000  
```


:::
:::



::: {.cell}

```{.r .cell-code}
skimr::skim(meth)
```

::: {.cell-output-display}

Table: Data summary

|                         |     |
|:------------------------|:----|
|Name                     |meth |
|Number of rows           |599  |
|Number of columns        |6    |
|_______________________  |     |
|Column type frequency:   |     |
|character                |2    |
|numeric                  |4    |
|________________________ |     |
|Group variables          |None |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|gender        |         0|             1|   3|   4|     0|        2|          0|
|grade         |         0|             1|   7|   9|     0|        2|          0|


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|    mean|     sd| p0|    p25|  p50|    p75| p100|hist  |
|:-------------|---------:|-------------:|-------:|------:|--:|------:|----:|------:|----:|:-----|
|age           |         0|             1| 1246.49| 223.11| 37| 1061.5| 1208| 1418.5| 1875|▁▁▇▇▃ |
|amas          |         0|             1|   21.98|   6.60|  4|   18.0|   22|   26.5|   45|▂▆▇▃▁ |
|rcmas         |         0|             1|   19.24|   7.57|  1|   14.0|   19|   25.0|   41|▂▇▇▅▁ |
|arith         |         0|             1|    5.30|   2.11|  0|    4.0|    6|    7.0|    8|▂▃▃▇▇ |


:::
:::


Show the structure of the dataset including data types.


::: {.cell}

```{.r .cell-code}
str(meth)
```

::: {.cell-output .cell-output-stdout}

```
spc_tbl_ [599 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ age   : num [1:599] 1378 1407 1379 1428 1356 ...
 $ gender: chr [1:599] "Boy" "Boy" "Girl" "Girl" ...
 $ grade : chr [1:599] "Secondary" "Secondary" "Secondary" "Secondary" ...
 $ amas  : num [1:599] 9 18 23 19 23 27 22 17 28 20 ...
 $ rcmas : num [1:599] 20 8 26 18 20 33 23 11 32 30 ...
 $ arith : num [1:599] 6 6 5 7 1 1 4 7 2 6 ...
 - attr(*, "spec")=
  .. cols(
  ..   Age = col_number(),
  ..   Gender = col_character(),
  ..   Grade = col_character(),
  ..   AMAS = col_double(),
  ..   RCMAS = col_double(),
  ..   Arith = col_double()
  .. )
 - attr(*, "problems")=<externalptr> 
```


:::
:::

Count occurrences by gender.

::: {.cell}

```{.r .cell-code}
count(meth, gender)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 2
  gender     n
  <chr>  <int>
1 Boy      323
2 Girl     276
```


:::
:::

Count occurrences by grade.

::: {.cell}

```{.r .cell-code}
count(meth, grade)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 2
  grade         n
  <chr>     <int>
1 Primary     401
2 Secondary   198
```


:::
:::

Return the dimensions of the dataset.

::: {.cell}

```{.r .cell-code}
dim(meth)
```

::: {.cell-output .cell-output-stdout}

```
[1] 599   6
```


:::
:::

List the column names of the dataset.


::: {.cell}

```{.r .cell-code}
base::names(meth)
```

::: {.cell-output .cell-output-stdout}

```
[1] "age"    "gender" "grade"  "amas"   "rcmas"  "arith" 
```


:::
:::



Provide a glimpse of the dataset showing types and sample values.


::: {.cell}

```{.r .cell-code}
dplyr::glimpse(meth)
```

::: {.cell-output .cell-output-stdout}

```
Rows: 599
Columns: 6
$ age    <dbl> 1378, 1407, 1379, 1428, 1356, 1350, 1336, 1393, 1317, 1348, 141…
$ gender <chr> "Boy", "Boy", "Girl", "Girl", "Boy", "Girl", "Boy", "Boy", "Gir…
$ grade  <chr> "Secondary", "Secondary", "Secondary", "Secondary", "Secondary"…
$ amas   <dbl> 9, 18, 23, 19, 23, 27, 22, 17, 28, 20, 16, 20, 21, 36, 16, 27, …
$ rcmas  <dbl> 20, 8, 26, 18, 20, 33, 23, 11, 32, 30, 10, 4, 23, 26, 24, 21, 3…
$ arith  <dbl> 6, 6, 5, 7, 1, 1, 4, 7, 2, 6, 2, 5, 2, 6, 2, 7, 2, 4, 7, 3, 8, …
```


:::
:::



Replace common NA representations with actual NA values in the dataset.

::: {.cell}

```{.r .cell-code}
meth_modified <- meth %>%
  naniar::replace_with_na_all(data = ., condition = ~ .x %in% common_na_numbers) %>%
  naniar::replace_with_na_all(data = ., condition = ~ .x %in% common_na_strings)
glimpse(meth_modified)
```

::: {.cell-output .cell-output-stdout}

```
Rows: 599
Columns: 6
$ age    <dbl> 1378, 1407, 1379, 1428, 1356, 1350, 1336, 1393, 1317, 1348, 141…
$ gender <chr> "Boy", "Boy", "Girl", "Girl", "Boy", "Girl", "Boy", "Boy", "Gir…
$ grade  <chr> "Secondary", "Secondary", "Secondary", "Secondary", "Secondary"…
$ amas   <dbl> 9, 18, 23, 19, 23, 27, 22, 17, 28, 20, 16, 20, 21, 36, 16, 27, …
$ rcmas  <dbl> 20, 8, 26, 18, 20, 33, 23, 11, 32, 30, 10, 4, 23, 26, 24, 21, 3…
$ arith  <dbl> 6, 6, 5, 7, 1, 1, 4, 7, 2, 6, 2, 5, 2, 6, 2, 7, 2, 4, 7, 3, 8, …
```


:::
:::


Visualize missing values in the modified dataset: Luckily there are none!

::: {.cell}

:::



## Munging

This section performs further data wrangling, such as binning age and factoring variables.

Convert gender and grade to factors and relocate them before age for better organization.


::: {.cell}

```{.r .cell-code}
meth_modified <- meth_modified %>%
  mutate(
    gender = as.factor(gender),
    grade = as.factor(grade),
  ) %>%
  dplyr::relocate(where(is.factor), .before = age)
glimpse(meth_modified)
```

::: {.cell-output .cell-output-stdout}

```
Rows: 599
Columns: 6
$ gender <fct> Boy, Boy, Girl, Girl, Boy, Girl, Boy, Boy, Girl, Boy, Boy, Boy,…
$ grade  <fct> Secondary, Secondary, Secondary, Secondary, Secondary, Secondar…
$ age    <dbl> 1378, 1407, 1379, 1428, 1356, 1350, 1336, 1393, 1317, 1348, 141…
$ amas   <dbl> 9, 18, 23, 19, 23, 27, 22, 17, 28, 20, 16, 20, 21, 36, 16, 27, …
$ rcmas  <dbl> 20, 8, 26, 18, 20, 33, 23, 11, 32, 30, 10, 4, 23, 26, 24, 21, 3…
$ arith  <dbl> 6, 6, 5, 7, 1, 1, 4, 7, 2, 6, 2, 5, 2, 6, 2, 7, 2, 4, 7, 3, 8, …
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  head(10) %>%
  dplyr::rename(
    "Gender" = gender,
    "Grade" = grade,
    "Age (months)" = age,
    "AMAS (Math Anxiety)" = amas,
    "RCMAS (General Anxiety)" = rcmas,
    "Arithmetic Score" = arith
  ) %>%
  tt(
    caption = "Math is fun!"
  ) %>%
  theme_html(class = "table table-hover table-striped table-condensed") %>%
  style_tt(fontsize = 0.8)
```

::: {.cell-output-display}

```{=html}
<!-- preamble start -->

    <script>

      function styleCell_c3zyi13cukobd674tpyo(i, j, css_id) {
          var table = document.getElementById("tinytable_c3zyi13cukobd674tpyo");
          var cell = table.querySelector(`[data-row="${i}"][data-col="${j}"]`);
          if (cell) {
              console.log(`Styling cell at (${i}, ${j}) with class ${css_id}`);
              cell.classList.add(css_id);
          } else {
              console.warn(`Cell at (${i}, ${j}) not found.`);
          }
      }
      function spanCell_c3zyi13cukobd674tpyo(i, j, rowspan, colspan) {
        var table = document.getElementById("tinytable_c3zyi13cukobd674tpyo");
        const targetCell = table.querySelector(`[data-row="${i}"][data-col="${j}"]`);
        if (!targetCell) {
          console.warn(`Cell at (${i}, ${j}) not found.`);
        }

        // Get all cells that need to be removed
        const cellsToRemove = [];
        for (let r = 0; r < rowspan; r++) {
          for (let c = 0; c < colspan; c++) {
            if (r === 0 && c === 0) continue; // Skip the target cell
            const cell = table.querySelector(`[data-row="${i + r}"][data-col="${j + c}"]`);
            if (cell) {
              cellsToRemove.push(cell);
            }
          }
        }

        // Remove all cells
        cellsToRemove.forEach(cell => cell.remove());

        // Set rowspan and colspan of the target cell if it exists
        if (targetCell) {
          targetCell.rowSpan = rowspan;
          targetCell.colSpan = colspan;
        }
      }
      // tinytable span after
      window.addEventListener('load', function () {
          var cellsToStyle = [
            // tinytable style arrays after
          { positions: [ { i: '10', j: 4 }, { i: '10', j: 2 }, { i: '10', j: 3 }, { i: '10', j: 0 }, { i: '10', j: 1 }, { i: '10', j: 5 },  ], css_id: 'tinytable_css_h20p6iux5xgu37nhdg6b',}, 
          { positions: [ { i: '2', j: 0 }, { i: '4', j: 0 }, { i: '6', j: 0 }, { i: '8', j: 0 }, { i: '1', j: 0 }, { i: '1', j: 2 }, { i: '1', j: 1 }, { i: '2', j: 1 }, { i: '3', j: 1 }, { i: '4', j: 1 }, { i: '3', j: 0 }, { i: '6', j: 1 }, { i: '5', j: 0 }, { i: '8', j: 1 }, { i: '7', j: 0 }, { i: '1', j: 3 }, { i: '9', j: 0 }, { i: '3', j: 3 }, { i: '2', j: 2 }, { i: '3', j: 2 }, { i: '4', j: 2 }, { i: '5', j: 2 }, { i: '6', j: 2 }, { i: '5', j: 1 }, { i: '8', j: 2 }, { i: '7', j: 1 }, { i: '1', j: 4 }, { i: '9', j: 1 }, { i: '3', j: 4 }, { i: '2', j: 3 }, { i: '5', j: 4 }, { i: '4', j: 3 }, { i: '5', j: 3 }, { i: '6', j: 3 }, { i: '7', j: 3 }, { i: '8', j: 3 }, { i: '7', j: 2 }, { i: '1', j: 5 }, { i: '9', j: 2 }, { i: '3', j: 5 }, { i: '2', j: 4 }, { i: '5', j: 5 }, { i: '4', j: 4 }, { i: '7', j: 5 }, { i: '6', j: 4 }, { i: '7', j: 4 }, { i: '8', j: 4 }, { i: '9', j: 4 }, { i: '9', j: 3 }, { i: '9', j: 5 }, { i: '2', j: 5 }, { i: '4', j: 5 }, { i: '6', j: 5 }, { i: '8', j: 5 },  ], css_id: 'tinytable_css_d8w2e5u36vn7mnb7lf68',}, 
          { positions: [ { i: '0', j: 0 }, { i: '0', j: 4 }, { i: '0', j: 5 }, { i: '0', j: 2 }, { i: '0', j: 3 }, { i: '0', j: 1 },  ], css_id: 'tinytable_css_iv4lmbml2vwicub1vbu1',}, 
          ];

          // Loop over the arrays to style the cells
          cellsToStyle.forEach(function (group) {
              group.positions.forEach(function (cell) {
                  styleCell_c3zyi13cukobd674tpyo(cell.i, cell.j, group.css_id);
              });
          });
      });
    </script>

    <style>
      /* tinytable css entries after */
      .table td.tinytable_css_h20p6iux5xgu37nhdg6b, .table th.tinytable_css_h20p6iux5xgu37nhdg6b { font-size: 0.8em; border-bottom: solid #d3d8dc 0.1em; }
      .table td.tinytable_css_d8w2e5u36vn7mnb7lf68, .table th.tinytable_css_d8w2e5u36vn7mnb7lf68 { font-size: 0.8em; }
      .table td.tinytable_css_iv4lmbml2vwicub1vbu1, .table th.tinytable_css_iv4lmbml2vwicub1vbu1 { font-size: 0.8em; border-top: solid #d3d8dc 0.1em; border-bottom: solid #d3d8dc 0.05em; }
    </style>
    <div class="container">
      <table class="table table-hover table-striped table-condensed" id="tinytable_c3zyi13cukobd674tpyo" style="width: auto; margin-left: auto; margin-right: auto;" data-quarto-disable-processing='true'>
        <thead>
        <caption>Math is fun!</caption>
              <tr>
                <th scope="col" data-row="0" data-col="0">Gender</th>
                <th scope="col" data-row="0" data-col="1">Grade</th>
                <th scope="col" data-row="0" data-col="2">Age (months)</th>
                <th scope="col" data-row="0" data-col="3">AMAS (Math Anxiety)</th>
                <th scope="col" data-row="0" data-col="4">RCMAS (General Anxiety)</th>
                <th scope="col" data-row="0" data-col="5">Arithmetic Score</th>
              </tr>
        </thead>
        
        <tbody>
                <tr>
                  <td data-row="1" data-col="0">Boy</td>
                  <td data-row="1" data-col="1">Secondary</td>
                  <td data-row="1" data-col="2">1378</td>
                  <td data-row="1" data-col="3">9</td>
                  <td data-row="1" data-col="4">20</td>
                  <td data-row="1" data-col="5">6</td>
                </tr>
                <tr>
                  <td data-row="2" data-col="0">Boy</td>
                  <td data-row="2" data-col="1">Secondary</td>
                  <td data-row="2" data-col="2">1407</td>
                  <td data-row="2" data-col="3">18</td>
                  <td data-row="2" data-col="4">8</td>
                  <td data-row="2" data-col="5">6</td>
                </tr>
                <tr>
                  <td data-row="3" data-col="0">Girl</td>
                  <td data-row="3" data-col="1">Secondary</td>
                  <td data-row="3" data-col="2">1379</td>
                  <td data-row="3" data-col="3">23</td>
                  <td data-row="3" data-col="4">26</td>
                  <td data-row="3" data-col="5">5</td>
                </tr>
                <tr>
                  <td data-row="4" data-col="0">Girl</td>
                  <td data-row="4" data-col="1">Secondary</td>
                  <td data-row="4" data-col="2">1428</td>
                  <td data-row="4" data-col="3">19</td>
                  <td data-row="4" data-col="4">18</td>
                  <td data-row="4" data-col="5">7</td>
                </tr>
                <tr>
                  <td data-row="5" data-col="0">Boy</td>
                  <td data-row="5" data-col="1">Secondary</td>
                  <td data-row="5" data-col="2">1356</td>
                  <td data-row="5" data-col="3">23</td>
                  <td data-row="5" data-col="4">20</td>
                  <td data-row="5" data-col="5">1</td>
                </tr>
                <tr>
                  <td data-row="6" data-col="0">Girl</td>
                  <td data-row="6" data-col="1">Secondary</td>
                  <td data-row="6" data-col="2">1350</td>
                  <td data-row="6" data-col="3">27</td>
                  <td data-row="6" data-col="4">33</td>
                  <td data-row="6" data-col="5">1</td>
                </tr>
                <tr>
                  <td data-row="7" data-col="0">Boy</td>
                  <td data-row="7" data-col="1">Secondary</td>
                  <td data-row="7" data-col="2">1336</td>
                  <td data-row="7" data-col="3">22</td>
                  <td data-row="7" data-col="4">23</td>
                  <td data-row="7" data-col="5">4</td>
                </tr>
                <tr>
                  <td data-row="8" data-col="0">Boy</td>
                  <td data-row="8" data-col="1">Secondary</td>
                  <td data-row="8" data-col="2">1393</td>
                  <td data-row="8" data-col="3">17</td>
                  <td data-row="8" data-col="4">11</td>
                  <td data-row="8" data-col="5">7</td>
                </tr>
                <tr>
                  <td data-row="9" data-col="0">Girl</td>
                  <td data-row="9" data-col="1">Secondary</td>
                  <td data-row="9" data-col="2">1317</td>
                  <td data-row="9" data-col="3">28</td>
                  <td data-row="9" data-col="4">32</td>
                  <td data-row="9" data-col="5">2</td>
                </tr>
                <tr>
                  <td data-row="10" data-col="0">Boy</td>
                  <td data-row="10" data-col="1">Secondary</td>
                  <td data-row="10" data-col="2">1348</td>
                  <td data-row="10" data-col="3">20</td>
                  <td data-row="10" data-col="4">30</td>
                  <td data-row="10" data-col="5">6</td>
                </tr>
        </tbody>
      </table>
    </div>
<!-- hack to avoid NA insertion in last line -->
```

:::
:::



## Table

This section creates tabular displays of the data, including styled tables and interactive datatables.

::: {.cell}

```{.r .cell-code}
meth_modified %>%
  DT::datatable(
    style = "default",
    caption = htmltools::tags$caption(
      style = "caption-side: top; text-align: left; color: black; font-size: 100%;", "Math Anxiety Dataset (Clean)"
    ),
    options = list(pageLength = 10, autoWidth = TRUE)
  ) %>%
  DT::formatStyle(
    columns = names(meth_modified),
    fontFamily = "Roboto Condensed",
    fontSize = "12px"
  )
```

::: {.cell-output-display}

```{=html}
<div class="datatables html-widget html-fill-item" id="htmlwidget-157286c8b2b8c748f211" style="width:100%;height:auto;"></div>
<script type="application/json" data-for="htmlwidget-157286c8b2b8c748f211">{"x":{"filter":"none","vertical":false,"caption":"<caption style=\"caption-side: top; text-align: left; color: black; font-size: 100%;\">Math Anxiety Dataset (Clean)<\/caption>","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150","151","152","153","154","155","156","157","158","159","160","161","162","163","164","165","166","167","168","169","170","171","172","173","174","175","176","177","178","179","180","181","182","183","184","185","186","187","188","189","190","191","192","193","194","195","196","197","198","199","200","201","202","203","204","205","206","207","208","209","210","211","212","213","214","215","216","217","218","219","220","221","222","223","224","225","226","227","228","229","230","231","232","233","234","235","236","237","238","239","240","241","242","243","244","245","246","247","248","249","250","251","252","253","254","255","256","257","258","259","260","261","262","263","264","265","266","267","268","269","270","271","272","273","274","275","276","277","278","279","280","281","282","283","284","285","286","287","288","289","290","291","292","293","294","295","296","297","298","299","300","301","302","303","304","305","306","307","308","309","310","311","312","313","314","315","316","317","318","319","320","321","322","323","324","325","326","327","328","329","330","331","332","333","334","335","336","337","338","339","340","341","342","343","344","345","346","347","348","349","350","351","352","353","354","355","356","357","358","359","360","361","362","363","364","365","366","367","368","369","370","371","372","373","374","375","376","377","378","379","380","381","382","383","384","385","386","387","388","389","390","391","392","393","394","395","396","397","398","399","400","401","402","403","404","405","406","407","408","409","410","411","412","413","414","415","416","417","418","419","420","421","422","423","424","425","426","427","428","429","430","431","432","433","434","435","436","437","438","439","440","441","442","443","444","445","446","447","448","449","450","451","452","453","454","455","456","457","458","459","460","461","462","463","464","465","466","467","468","469","470","471","472","473","474","475","476","477","478","479","480","481","482","483","484","485","486","487","488","489","490","491","492","493","494","495","496","497","498","499","500","501","502","503","504","505","506","507","508","509","510","511","512","513","514","515","516","517","518","519","520","521","522","523","524","525","526","527","528","529","530","531","532","533","534","535","536","537","538","539","540","541","542","543","544","545","546","547","548","549","550","551","552","553","554","555","556","557","558","559","560","561","562","563","564","565","566","567","568","569","570","571","572","573","574","575","576","577","578","579","580","581","582","583","584","585","586","587","588","589","590","591","592","593","594","595","596","597","598","599"],["Boy","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Girl","Boy","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Girl","Girl","Girl","Girl","Girl","Girl","Boy","Boy","Girl","Girl","Girl","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Girl","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Girl","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Girl","Girl","Girl","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Girl","Girl","Girl","Girl","Girl","Boy","Girl","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Boy","Girl","Girl","Girl","Girl","Girl","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Girl","Girl","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Girl","Boy","Girl","Boy","Boy","Girl","Girl","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Girl","Girl","Girl","Girl","Girl","Boy","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Girl","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Girl","Girl","Boy","Girl","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Girl","Boy","Girl","Boy","Girl","Boy","Boy","Girl","Boy","Boy","Boy","Boy","Girl","Boy","Boy","Girl","Girl","Girl","Girl","Girl","Boy","Boy","Girl","Girl","Boy","Boy","Boy","Boy","Girl","Girl","Boy","Girl","Boy","Girl","Girl"],["Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Secondary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary","Primary"],[1378,1407,1379,1428,1356,1350,1336,1393,1317,1348,1413,1580,1551,1327,1341,1379,1326,1366,1467,1341,1354,1334,1376,1322,1415,1332,1336,1411,1434,1568,1444,1661,1442,1462,1512,1512,1436,1533,1526,1436,1512,1522,1615,1433,1513,1747,1462,1551,1491,1493,1445,1432,1503,1478,1450,1508,1458,1522,1456,1634,1586,1645,1543,1586,1594,1554,1643,1577,1628,1639,1573,1622,1623,1591,1606,1596,1620,1638,1759,1635,1691,1611,1600,1703,1626,1611,1559,1668,1789,1579,1577,1791,1606,1803,1655,1569,1321,1340,1431,1315,1354,1341,1412,1324,1324,1424,1327,1409,1422,1400,1363,1344,1366,1413,1323,1460,1388,1335,1406,1363,1364,1391,1333,1427,1527,1377,1455,1524,1588,1425,1567,1493,1444,1492,1479,1424,1533,1485,1587,1465,1424,1547,1674,1566,1551,1575,1511,1560,1553,1450,1496,1432,1525,1576,1529,1455,1631,1517,1452,1685,1486,1708,1641,1459,1487,1440,1506,1587,1574,1515,1595,1581,1733,1555,1582,1614,1623,1538,1610,1544,1634,1554,1622,1632,1605,1582,1670,1579,1569,1803,1645,1735,1598,1605,1572,1672,1875,1797,971,1027,964,974,974,947,931,1028,948,837,927,1043,1023,1027,959,925,992,1009,910,1063,1135,1066,1114,1042,1050,1073,1003,1150,1129,1149,1187,1104,1070,1062,1127,1122,1208,1218,1150,1237,1169,1193,1252,1276,1200,1200,1272,1237,1150,1150,1254,1222,1253,1154,1282,1206,1214,1139,1178,1161,940,947,1010,1009,949,1080,1093,1091,1161,1093,1126,1076,1111,1110,1056,1173,1107,1124,1064,1223,1271,1185,1262,1251,1204,1265,1250,1157,1261,1069,973,995,1059,938,1060,1059,980,938,955,955,1061,1059,968,952,973,1017,1015,1059,1027,1023,963,989,959,1013,1004,1017,979,1011,951,1025,1021,1031,1050,1045,1036,1049,1027,1040,977,1061,962,979,1054,975,1017,1045,963,986,986,1050,979,995,984,1061,977,981,952,966,1037,971,991,1005,1058,1037,1018,1120,1101,1136,1186,1179,1075,1183,1124,1139,1085,1141,1157,1131,1172,1112,1180,1144,1155,1187,1156,1081,1113,1178,1121,1128,1072,1113,1067,1077,1065,1176,1156,1153,1077,1090,1170,1165,1152,1108,1169,1174,1183,1141,1122,1144,1176,1130,1109,1090,1259,1269,1247,1221,1234,1201,1244,1309,1301,1277,1261,1250,1217,1252,1266,1203,1221,1277,1189,1200,1200,1213,1351,1174,1233,1299,1269,1198,1279,1288,1198,1232,1251,1294,1249,1273,1220,1170,1284,1197,1309,1212,1154,1284,1210,1299,1309,1223,1235,1196,1185,1286,1231,1193,1283,1259,1308,37,1257,1078,1134,1177,1153,1174,1112,1154,1102,1089,1121,1134,1073,1301,1237,1234,1307,1252,1295,1220,1262,1200,1009,987,1027,957,1025,971,1035,971,993,974,1019,974,987,1035,1045,1027,1031,1021,1012,1058,960,1021,1061,1119,1001,1012,958,961,958,1045,1009,953,1033,991,1021,1021,1032,974,1011,1054,1054,1060,1053,1018,1043,984,963,957,1060,1054,991,1043,1054,960,963,1069,980,1055,1179,1184,1107,1134,1103,1088,1107,1191,1075,1127,1162,1134,1087,1178,1096,1104,1112,1133,1127,1090,1100,1083,1129,1079,1101,1211,1232,1285,1247,1301,1220,1308,1229,1208,1232,1239,1286,1277,1229,1292,1200,1229,1257,1210,1232,1242,1265,1295,1269,1286,1223,1201,1280,1263,1262,1249,1253,1276,1256],[9,18,23,19,23,27,22,17,28,20,16,20,21,36,16,27,28,18,29,31,21,24,17,25,9,17,37,30,21,31,35,24,21,33,27,24,20,9,25,32,19,13,29,21,23,27,20,21,20,33,21,15,25,25,12,12,28,36,13,23,25,23,13,27,16,45,25,14,25,23,28,18,17,26,18,14,14,10,16,14,11,23,22,18,15,27,31,19,20,18,15,37,14,19,9,23,21,14,12,22,25,23,33,26,24,33,22,19,25,18,20,21,29,28,19,40,16,27,34,25,28,34,22,16,14,11,31,18,30,16,34,21,27,24,22,28,23,19,29,22,28,21,35,26,21,36,14,23,20,22,21,17,18,22,12,23,22,23,33,31,22,19,25,22,12,21,19,24,18,21,18,28,20,18,17,26,27,28,21,21,15,32,12,24,12,24,20,14,19,33,26,21,18,10,22,11,26,29,9,21,26,18,23,22,16,19,14,26,10,18,18,29,9,28,28,35,17,31,29,19,10,38,17,17,36,31,13,11,20,13,30,18,27,33,21,21,21,35,24,14,20,22,29,21,25,22,23,23,21,20,15,30,26,31,27,16,23,23,13,12,11,24,11,22,33,28,18,31,26,26,27,32,29,28,32,20,26,20,27,23,22,31,13,21,15,13,20,14,27,13,16,24,19,19,21,35,17,16,14,24,18,15,17,23,31,26,28,33,28,18,33,24,22,11,22,32,29,24,22,27,11,26,25,26,22,25,14,18,13,22,22,30,14,21,16,29,38,18,26,38,30,23,26,27,17,36,29,9,22,9,31,19,19,26,17,27,24,29,22,20,28,18,27,11,18,30,20,11,24,18,17,22,31,16,14,23,11,19,19,19,29,25,20,15,13,25,29,13,20,12,32,18,23,33,28,16,17,24,29,19,31,28,25,20,15,20,21,21,25,21,24,11,29,23,18,21,27,31,16,27,11,34,33,20,12,26,26,11,19,20,34,22,12,19,23,25,21,19,25,26,9,27,17,33,14,24,20,26,22,36,25,23,9,28,14,25,19,25,14,11,22,26,28,24,20,15,18,22,29,31,24,16,29,9,27,23,26,19,23,19,29,30,28,20,24,26,25,23,30,22,28,13,26,17,27,32,25,21,22,23,22,19,18,12,4,20,18,31,27,23,29,29,15,29,20,19,19,20,17,24,9,23,20,19,16,19,9,21,29,9,15,23,19,21,22,9,24,22,9,9,18,21,26,28,31,26,21,35,26,18,26,15,19,29,17,13,24,15,17,29,16,22,25,16,25,13,26,17,23,12,12,25,13,21,21,14,19,18,18,24,16,19,20,9,20,26,29,21,26,17,15,24,34,24,27,23,27,30,24],[20,8,26,18,20,33,23,11,32,30,10,4,23,26,24,21,30,26,18,12,15,12,8,15,3,10,30,19,18,26,21,18,16,34,14,19,7,22,23,26,10,13,30,17,15,18,4,16,21,32,32,17,33,23,14,12,17,41,13,14,17,10,16,25,18,23,23,19,25,6,15,9,8,18,15,5,11,23,18,20,15,21,23,15,13,21,23,7,27,21,11,31,23,6,27,11,23,9,8,8,15,16,29,29,15,27,15,22,34,10,28,8,23,29,14,35,19,23,22,21,14,23,20,25,17,26,24,9,15,27,1,17,25,16,16,18,21,25,28,16,19,19,25,11,17,26,10,13,15,11,22,15,7,13,1,9,20,17,32,18,26,19,26,14,12,18,17,9,6,13,13,35,10,12,15,27,14,12,16,18,12,29,8,21,38,26,19,20,36,33,22,26,10,10,4,9,13,25,9,10,23,30,25,38,20,22,17,23,12,3,5,19,34,21,30,22,13,12,20,18,27,24,15,26,14,28,15,21,27,17,27,20,9,28,21,8,23,22,9,17,16,14,29,20,25,21,14,23,24,10,7,26,11,22,28,11,24,23,15,24,19,20,15,9,25,20,15,22,25,24,23,32,19,20,31,24,29,28,27,7,8,20,5,13,5,4,13,24,27,7,15,20,21,4,26,33,23,31,19,25,23,23,22,24,12,22,17,28,18,10,23,21,22,22,31,31,34,25,22,15,14,22,18,34,19,25,14,20,27,25,22,22,14,18,14,28,28,20,22,17,20,27,3,13,19,22,23,7,12,8,29,9,28,29,25,10,15,32,20,20,19,15,28,8,21,34,21,8,31,10,16,13,26,4,15,24,21,14,10,23,25,30,15,21,28,17,24,6,9,13,28,12,17,24,14,15,15,11,28,13,21,29,25,15,15,20,28,18,33,18,12,18,19,18,11,16,25,38,27,33,15,29,25,16,17,20,6,15,25,14,19,19,26,17,11,19,5,18,14,29,13,26,20,17,25,29,18,19,20,26,17,13,9,20,7,16,13,17,20,5,5,20,25,8,24,17,9,29,35,30,29,25,28,8,19,25,20,21,21,30,31,22,26,8,33,27,20,31,24,23,32,18,17,9,26,28,25,11,22,18,26,14,19,13,18,17,16,35,30,22,29,18,14,24,16,22,13,25,21,13,12,22,25,13,19,17,28,13,27,23,27,18,33,30,12,10,13,24,10,19,15,26,17,17,34,28,27,28,14,15,24,16,23,29,18,8,14,12,6,23,23,22,13,23,21,10,30,19,14,14,16,13,23,8,19,20,19,11,10,16,14,10,26,27,12,8,37,12,20,17,13,17,22,31,11,8,11,20,31],[6,6,5,7,1,1,4,7,2,6,2,5,2,6,2,7,2,4,7,3,8,7,7,6,3,0,3,4,4,3,4,8,2,1,5,6,3,5,0,6,1,4,4,4,2,4,4,3,4,5,1,7,1,7,3,6,4,0,4,4,3,3,4,0,5,3,0,5,4,4,0,3,5,3,3,2,6,8,6,7,7,3,7,2,5,2,3,6,5,3,5,5,8,4,0,6,7,4,6,7,4,4,4,5,8,2,8,3,4,5,5,7,6,6,5,7,5,5,1,4,5,3,5,8,6,5,1,7,2,8,2,3,6,6,7,8,5,6,2,7,2,3,1,5,7,2,1,0,1,4,6,5,8,3,2,4,1,2,3,6,5,0,1,6,5,0,6,4,7,3,7,2,7,4,7,6,3,5,5,6,6,1,7,4,4,5,1,4,5,6,5,3,7,7,3,8,0,6,7,4,1,5,3,1,4,4,7,3,3,6,1,7,1,5,6,3,4,7,7,0,7,7,3,8,7,6,7,0,5,4,8,8,7,7,4,5,7,8,3,4,3,8,2,4,7,8,7,5,7,7,6,3,7,3,0,5,4,5,5,3,5,6,6,8,7,7,8,7,5,7,6,6,8,8,8,8,8,4,5,5,8,1,8,7,4,5,4,6,5,5,6,6,4,6,4,6,6,5,6,6,6,2,5,4,3,5,4,5,5,6,3,6,6,5,5,5,5,6,6,8,8,8,5,8,5,6,7,7,7,4,8,6,7,8,7,7,6,8,7,3,5,6,6,6,4,3,1,6,3,8,5,6,6,7,8,8,4,8,7,7,8,8,8,8,6,7,6,8,8,6,8,8,8,5,7,2,7,5,3,5,8,7,6,8,8,7,5,6,8,8,6,8,6,7,7,4,7,8,8,7,6,6,8,8,7,8,7,3,8,8,8,7,5,7,7,5,3,4,8,7,8,4,6,8,1,3,4,5,5,7,6,6,7,4,8,0,8,7,7,6,8,7,7,8,2,7,8,7,8,7,8,5,8,6,7,7,7,8,8,8,6,7,7,7,7,8,6,8,6,8,8,7,6,7,6,7,8,7,7,6,3,7,5,4,5,5,6,6,5,2,6,6,3,6,4,5,6,6,6,6,5,7,7,8,5,7,3,4,6,4,4,7,7,8,8,8,5,4,4,3,4,5,6,7,3,5,5,6,3,4,6,5,2,3,6,5,4,5,5,5,8,7,6,8,1,2,8,1,4,8,7,5,1,4,3,7,5,7,6,7,6,8,6,6,6,3,4,8,5,6,8,6,6,6,5,7,7,6,5,6,6,8,7,8,8,7,6,7,3,7,8,3,6,5,5,5,5,6,4]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>gender<\/th>\n      <th>grade<\/th>\n      <th>age<\/th>\n      <th>amas<\/th>\n      <th>rcmas<\/th>\n      <th>arith<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"pageLength":10,"autoWidth":true,"columnDefs":[{"className":"dt-right","targets":[3,4,5,6]},{"orderable":false,"targets":0},{"name":" ","targets":0},{"name":"gender","targets":1},{"name":"grade","targets":2},{"name":"age","targets":3},{"name":"amas","targets":4},{"name":"rcmas","targets":5},{"name":"arith","targets":6}],"order":[],"orderClasses":false,"rowCallback":"function(row, data, displayNum, displayIndex, dataIndex) {\nvar value=data[1]; $(this.api().cell(row, 1).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\nvar value=data[2]; $(this.api().cell(row, 2).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\nvar value=data[3]; $(this.api().cell(row, 3).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\nvar value=data[4]; $(this.api().cell(row, 4).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\nvar value=data[5]; $(this.api().cell(row, 5).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\nvar value=data[6]; $(this.api().cell(row, 6).node()).css({'font-family':'Roboto Condensed','font-size':'12px'});\n}"},"selection":{"mode":"multiple","selected":null,"target":"row","selectable":null}},"evals":["options.rowCallback"],"jsHooks":[]}</script>
```

:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  slice_sample(n = 150, weight_by = gender) %>%
  gf_point(amas ~ age,
    colour = ~gender,
    shape = ~gender,
    size = 2, data = .
  ) %>%
  gf_labs(
    title = "MATH ANXIETY & AGE",
    subtitle = "By Gender",
    caption = "From the MathAnxiety dataset",
    x = "Age (years)",
    y = "AMAS (Math Anxiety Score)"
  ) %>%
  gf_refine(
    scale_color_brewer(
      name = "Gender",
      palette = "Set1"
    ),
    scale_shape_manual(
      name = "Gender",
      values = c(15:21)
    )
  )
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-17-1.png){width=672}
:::
:::


## Charts

This section generates various visualizations including points, histograms, boxplots, bars, and more to explore relationships.

::: {.cell}

```{.r .cell-code}
meth_modified %>%
  slice_sample(n = 150, weight_by = grade) %>%
  gf_point(amas ~ age | grade,
    colour = ~gender,
    shape = ~gender,
    size = 2, data = .
  ) %>%
  gf_labs(
    title = "MATH ANXIETY BY GRADE",
    subtitle = "Faceted by Grade, Colored by Gender",
    caption = "From the MathAnxiety dataset",
    x = "Age (years)",
    y = "AMAS (Math Anxiety Score)"
  ) %>%
  gf_refine(
    scale_color_brewer(
      name = "Gender",
      palette = "Set1"
    ),
    scale_shape_manual(
      name = "Gender",
      values = c(15:21)
    )
  )
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-18-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% dplyr::glimpse()
```

::: {.cell-output .cell-output-stdout}

```
Rows: 599
Columns: 6
$ gender <fct> Boy, Boy, Girl, Girl, Boy, Girl, Boy, Boy, Girl, Boy, Boy, Boy,…
$ grade  <fct> Secondary, Secondary, Secondary, Secondary, Secondary, Secondar…
$ age    <dbl> 1378, 1407, 1379, 1428, 1356, 1350, 1336, 1393, 1317, 1348, 141…
$ amas   <dbl> 9, 18, 23, 19, 23, 27, 22, 17, 28, 20, 16, 20, 21, 36, 16, 27, …
$ rcmas  <dbl> 20, 8, 26, 18, 20, 33, 23, 11, 32, 30, 10, 4, 23, 26, 24, 21, 3…
$ arith  <dbl> 6, 6, 5, 7, 1, 1, 4, 7, 2, 6, 2, 5, 2, 6, 2, 7, 2, 4, 7, 3, 8, …
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% mosaic::inspect()
```

::: {.cell-output .cell-output-stdout}

```

categorical variables:  
    name  class levels   n missing
1 gender factor      2 599       0
2  grade factor      2 599       0
                                   distribution
1 Boy (53.9%), Girl (46.1%)                    
2 Primary (66.9%), Secondary (33.1%)           

quantitative variables:  
   name   class min     Q1 median     Q3  max       mean         sd   n missing
1   age numeric  37 1061.5   1208 1418.5 1875 1246.49249 223.112183 599       0
2  amas numeric   4   18.0     22   26.5   45   21.98164   6.597962 599       0
3 rcmas numeric   1   14.0     19   25.0   41   19.24040   7.566802 599       0
4 arith numeric   0    4.0      6    7.0    8    5.30217   2.105220 599       0
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% skimr::skim()
```

::: {.cell-output-display}

Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |599        |
|Number of columns        |6          |
|_______________________  |           |
|Column type frequency:   |           |
|factor                   |2          |
|numeric                  |4          |
|________________________ |           |
|Group variables          |None       |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts         |
|:-------------|---------:|-------------:|:-------|--------:|:------------------|
|gender        |         0|             1|FALSE   |        2|Boy: 323, Gir: 276 |
|grade         |         0|             1|FALSE   |        2|Pri: 401, Sec: 198 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|    mean|     sd| p0|    p25|  p50|    p75| p100|hist  |
|:-------------|---------:|-------------:|-------:|------:|--:|------:|----:|------:|----:|:-----|
|age           |         0|             1| 1246.49| 223.11| 37| 1061.5| 1208| 1418.5| 1875|▁▁▇▇▃ |
|amas          |         0|             1|   21.98|   6.60|  4|   18.0|   22|   26.5|   45|▂▆▇▃▁ |
|rcmas         |         0|             1|   19.24|   7.57|  1|   14.0|   19|   25.0|   41|▂▇▇▅▁ |
|arith         |         0|             1|    5.30|   2.11|  0|    4.0|    6|    7.0|    8|▂▃▃▇▇ |


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%  dplyr::count(gender)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 2
  gender     n
  <fct>  <int>
1 Boy      323
2 Girl     276
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%  dplyr::count(across(.cols = c(gender, grade)))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 3
  gender grade         n
  <fct>  <fct>     <int>
1 Boy    Primary     199
2 Boy    Secondary   124
3 Girl   Primary     202
4 Girl   Secondary    74
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%  dplyr::count(across(where(is.factor)))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 3
  gender grade         n
  <fct>  <fct>     <int>
1 Boy    Primary     199
2 Boy    Secondary   124
3 Girl   Primary     202
4 Girl   Secondary    74
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% mosaic::inspect()
```

::: {.cell-output .cell-output-stdout}

```

categorical variables:  
    name  class levels   n missing
1 gender factor      2 599       0
2  grade factor      2 599       0
                                   distribution
1 Boy (53.9%), Girl (46.1%)                    
2 Primary (66.9%), Secondary (33.1%)           

quantitative variables:  
   name   class min     Q1 median     Q3  max       mean         sd   n missing
1   age numeric  37 1061.5   1208 1418.5 1875 1246.49249 223.112183 599       0
2  amas numeric   4   18.0     22   26.5   45   21.98164   6.597962 599       0
3 rcmas numeric   1   14.0     19   25.0   41   19.24040   7.566802 599       0
4 arith numeric   0    4.0      6    7.0    8    5.30217   2.105220 599       0
```


:::

```{.r .cell-code}
meth_modified %>% skimr::skim()
```

::: {.cell-output-display}

Table: Data summary

|                         |           |
|:------------------------|:----------|
|Name                     |Piped data |
|Number of rows           |599        |
|Number of columns        |6          |
|_______________________  |           |
|Column type frequency:   |           |
|factor                   |2          |
|numeric                  |4          |
|________________________ |           |
|Group variables          |None       |


**Variable type: factor**

|skim_variable | n_missing| complete_rate|ordered | n_unique|top_counts         |
|:-------------|---------:|-------------:|:-------|--------:|:------------------|
|gender        |         0|             1|FALSE   |        2|Boy: 323, Gir: 276 |
|grade         |         0|             1|FALSE   |        2|Pri: 401, Sec: 198 |


**Variable type: numeric**

|skim_variable | n_missing| complete_rate|    mean|     sd| p0|    p25|  p50|    p75| p100|hist  |
|:-------------|---------:|-------------:|-------:|------:|--:|------:|----:|------:|----:|:-----|
|age           |         0|             1| 1246.49| 223.11| 37| 1061.5| 1208| 1418.5| 1875|▁▁▇▇▃ |
|amas          |         0|             1|   21.98|   6.60|  4|   18.0|   22|   26.5|   45|▂▆▇▃▁ |
|rcmas         |         0|             1|   19.24|   7.57|  1|   14.0|   19|   25.0|   41|▂▇▇▅▁ |
|arith         |         0|             1|    5.30|   2.11|  0|    4.0|    6|    7.0|    8|▂▃▃▇▇ |


:::

```{.r .cell-code}
meth_modified %>%  dplyr::count(gender)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 2
  gender     n
  <fct>  <int>
1 Boy      323
2 Girl     276
```


:::

```{.r .cell-code}
meth_modified %>%  dplyr::count(across(.cols = c(gender, grade)))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 3
  gender grade         n
  <fct>  <fct>     <int>
1 Boy    Primary     199
2 Boy    Secondary   124
3 Girl   Primary     202
4 Girl   Secondary    74
```


:::

```{.r .cell-code}
meth_modified %>%  dplyr::count(across(where(is.factor)))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 3
  gender grade         n
  <fct>  <fct>     <int>
1 Boy    Primary     199
2 Boy    Secondary   124
3 Girl   Primary     202
4 Girl   Secondary    74
```


:::
:::


::: {.cell}

```{.r .cell-code}
overall_count <- meth_modified
```
:::



::: {.cell}

```{.r .cell-code}
meth_modified %>% dplyr::count(across(.cols = c(gender, grade)))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 3
  gender grade         n
  <fct>  <fct>     <int>
1 Boy    Primary     199
2 Boy    Secondary   124
3 Girl   Primary     202
4 Girl   Secondary    74
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(
    mean_amas = mean(amas, na.rm = T),
    sd_amas = sd(amas, na.rm = T),
    min_amas = min(amas, na.rm = T),
    max_amas = max(amas, na.rm = T)
  )
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 4
  mean_amas sd_amas min_amas max_amas
      <dbl>   <dbl>    <dbl>    <dbl>
1      22.0    6.60        4       45
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(across(
    .cols = c(amas, rcmas), 

    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 8
  amas_mean amas_sd amas_min amas_max rcmas_mean rcmas_sd rcmas_min rcmas_max
      <dbl>   <dbl>    <dbl>    <dbl>      <dbl>    <dbl>     <dbl>     <dbl>
1      22.0    6.60        4       45       19.2     7.57         1        41
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(mean_rcmas = mean(rcmas, na.rm = T))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 1
  mean_rcmas
       <dbl>
1       19.2
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(
    mean_rcmas = mean(rcmas, na.rm = T),
    sd_rcmas = sd(rcmas, na.rm = T),
    min_rcmas = min(rcmas, na.rm = T),
    max_rcmas = max(rcmas, na.rm = T)
  )
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 4
  mean_rcmas sd_rcmas min_rcmas max_rcmas
       <dbl>    <dbl>     <dbl>     <dbl>
1       19.2     7.57         1        41
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(
    mean_arith = mean(arith, na.rm = T),
    sd_arith = sd(arith, na.rm = T),
    min_arith = min(arith, na.rm = T),
    max_arith = max(arith, na.rm = T)
  )
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 4
  mean_arith sd_arith min_arith max_arith
       <dbl>    <dbl>     <dbl>     <dbl>
1       5.30     2.11         0         8
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(
    mean_age = mean(age, na.rm = T),
    sd_age = sd(age, na.rm = T),
    min_age = min(age, na.rm = T),
    max_age = max(age, na.rm = T)
  )
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 4
  mean_age sd_age min_age max_age
     <dbl>  <dbl>   <dbl>   <dbl>
1    1246.   223.      37    1875
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),

    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 16
  age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max rcmas_mean
     <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>      <dbl>
1    1246.   223.      37    1875      22.0    6.60        4       45       19.2
# ℹ 7 more variables: rcmas_sd <dbl>, rcmas_min <dbl>, rcmas_max <dbl>,
#   arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>, arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  group_by(gender) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),

    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 17
  gender age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max
  <fct>     <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>
1 Boy       1276.   229.     931    1875      21.2    6.51        4       45
2 Girl      1211.   211.      37    1803      22.9    6.59        9       40
# ℹ 8 more variables: rcmas_mean <dbl>, rcmas_sd <dbl>, rcmas_min <dbl>,
#   rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>,
#   arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  group_by(grade) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),

    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 17
  grade     age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max
  <fct>        <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>
1 Primary      1115.   121.      37    1351      21.8    6.53        4       38
2 Secondary    1513.   121.    1315    1875      22.3    6.75        9       45
# ℹ 8 more variables: rcmas_mean <dbl>, rcmas_sd <dbl>, rcmas_min <dbl>,
#   rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>,
#   arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  group_by(gender, grade) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),

    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stderr}

```
`summarise()` has grouped output by 'gender'. You can override using the
`.groups` argument.
```


:::

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 18
# Groups:   gender [2]
  gender grade     age_mean age_sd age_min age_max amas_mean amas_sd amas_min
  <fct>  <fct>        <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>
1 Boy    Primary      1121.   109.     931    1351      20.9    6.30        4
2 Boy    Secondary    1526.   126.    1315    1875      21.5    6.83        9
3 Girl   Primary      1109.   131.      37    1309      22.7    6.63        9
4 Girl   Secondary    1492.   109.    1317    1803      23.5    6.47       11
# ℹ 9 more variables: amas_max <dbl>, rcmas_mean <dbl>, rcmas_sd <dbl>,
#   rcmas_min <dbl>, rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>,
#   arith_min <dbl>, arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),
    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 1 × 16
  age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max rcmas_mean
     <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>      <dbl>
1    1246.   223.      37    1875      22.0    6.60        4       45       19.2
# ℹ 7 more variables: rcmas_sd <dbl>, rcmas_min <dbl>, rcmas_max <dbl>,
#   arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>, arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  group_by(G=grade) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith), 
    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 17
  G         age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max
  <fct>        <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>
1 Primary      1115.   121.      37    1351      21.8    6.53        4       38
2 Secondary    1513.   121.    1315    1875      22.3    6.75        9       45
# ℹ 8 more variables: rcmas_mean <dbl>, rcmas_sd <dbl>, rcmas_min <dbl>,
#   rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>,
#   arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  group_by(gender, grade) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith), 
    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stderr}

```
`summarise()` has grouped output by 'gender'. You can override using the
`.groups` argument.
```


:::

::: {.cell-output .cell-output-stdout}

```
# A tibble: 4 × 18
# Groups:   gender [2]
  gender grade     age_mean age_sd age_min age_max amas_mean amas_sd amas_min
  <fct>  <fct>        <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>
1 Boy    Primary      1121.   109.     931    1351      20.9    6.30        4
2 Boy    Secondary    1526.   126.    1315    1875      21.5    6.83        9
3 Girl   Primary      1109.   131.      37    1309      22.7    6.63        9
4 Girl   Secondary    1492.   109.    1317    1803      23.5    6.47       11
# ℹ 9 more variables: amas_max <dbl>, rcmas_mean <dbl>, rcmas_sd <dbl>,
#   rcmas_min <dbl>, rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>,
#   arith_min <dbl>, arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
gf_histogram(~age, data = meth_modified) %>%
  gf_labs(title = "Histogram of Age")
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-41-1.png){width=672}
:::
:::



::: {.cell}

```{.r .cell-code}
gf_histogram(~amas, data = meth_modified) %>%
  gf_labs(title = "Histogram of AMAS")
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-42-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
gf_histogram(~rcmas, data = meth_modified) %>%
  gf_labs(title = "Histogram of RCMAS")
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-43-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
gf_bar(~grade, data = meth_modified) %>%
  gf_labs(title = "Bar Plot of Grade")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-44-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
gf_bar(~gender, data = meth_modified) %>%
  gf_labs(title = "Bar Plot of Gender")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-45-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = grade, fill = gender), position = "dodge") +
  labs(title = "Dodged Bar Chart") +
  scale_fill_brewer(palette = "Set2")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-46-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = grade, fill = gender), position = "stack") +
  labs(
    title = "Stacked Bar Chart",
    subtitle = "Can we spot per group differences in proportions??"
  ) +
  scale_fill_brewer(palette = "Set2")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-47-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = grade, fill = gender), position = "fill") +
  labs(title = "Filled Bar Chart", subtitle = "Shows Per group differences in Proportions!") +
  scale_fill_brewer(palette = "Set2")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-48-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = grade, fill = gender), position = "fill") +
  labs(
    title = "Filled Bar Chart",
    subtitle = "Shows Per group differences in Proportions!",
    y = "Proportion"
  ) +
  scale_fill_brewer(palette = "Set2")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-49-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot(aes(x = amas)) +
  geom_histogram(binwidth = 5, fill = "steelblue", color = "white") +
  facet_grid(grade ~ gender) +
  labs(title = "Histogram of AMAS Scores",
       subtitle = "Faceted by Grade and Gender",
       x = "AMAS Score",
       y = "Count") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-50-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot(aes(x = grade, y = rcmas)) +
  geom_boxplot(fill = "lightblue") +
  facet_wrap(~ gender) +
  labs(title = "Boxplots of RCMAS Scores by Grade",
       subtitle = "Faceted by Gender",
       x = "Grade",
       y = "RCMAS Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-51-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot(aes(x = age, y = arith, color = grade)) +
  geom_point(alpha = 0.6) +
  facet_grid(. ~ gender) +
  labs(title = "Scatter Plot of Age vs Arithmetic Score",
       subtitle = "Faceted by Gender, Colored by Grade",
       x = "Age",
       y = "Arithmetic Score") +
  scale_color_brewer(palette = "Set2") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-52-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot(aes(x = age, fill = gender)) +
  geom_density(alpha = 0.5) +
  facet_wrap(~ grade) +
  labs(title = "Density Plots of Age",
       subtitle = "Faceted by Grade, Filled by Gender",
       x = "Age",
       y = "Density") +
  scale_fill_brewer(palette = "Set2") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-53-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = factor(arith), fill = gender), position = "dodge") +
  labs(title = "Dodged Bar Chart") +
  theme(axis.text.x = element_text(size = 6, angle = 45, hjust = 1)) +
  scale_fill_brewer(palette = "Set3")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-54-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = factor(arith), fill = gender), position = "stack") +
  labs(
    title = "Stacked Bar Chart",
    subtitle = "Can we spot per group differences in proportions??"
  ) +
  theme(axis.text.x = element_text(size = 6, angle = 45, hjust = 1)) +
  scale_fill_brewer(palette = "Set3")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-55-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = factor(arith), fill = gender), position = "fill") +
  labs(
    title = "Filled Bar Chart",
    subtitle = "Shows Per group differences in Proportions!"
  ) +
  theme(axis.text.x = element_text(size = 6, angle = 45, hjust = 1)) +
  scale_fill_brewer(palette = "Set3")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-56-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_bar(aes(x = factor(arith), fill = gender), position = "fill") +
  labs(
    title = "Filled Bar Chart",
    subtitle = "Shows Per group differences in Proportions!",
    y = "Proportions"
  ) +
  theme(axis.text.x = element_text(size = 6, angle = 45, hjust = 1)) +
  scale_fill_brewer(palette = "Set3")
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-57-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified <- meth_modified %>%
  mutate(
    age_group = cut(
      age,
      breaks = c(0, 80, 100, 120, 140, 160, 180, 200),
      labels = c("0-80", "80-100", "100-120", "120-140", "140-160", "160-180", "180-200"),
      include.lowest = TRUE
    ),
    arith = factor(arith)
  )
```
:::



::: {.cell}

```{.r .cell-code}
gf_bar(~arith, fill = ~gender, data = meth_modified) %>%
  gf_labs(title = "Counts of Gender by Arithmetic Score") %>%
  gf_refine(scale_fill_brewer(palette = "Set4"))
```

::: {.cell-output .cell-output-stderr}

```
Warning: Unknown palette: "Set4"
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-59-1.png){width=672}
:::
:::



::: {.cell}

```{.r .cell-code}
ggplot(meth_modified) +
  geom_bar(aes(x = age_group, fill = gender)) +
  labs(title = "Counts of Gender by Age Group") +
  scale_fill_brewer(palette = "Set1") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-60-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified) +
  geom_bar(aes(x = grade, fill = gender)) +
  labs(title = "Counts of Gender by Grade") +
  scale_fill_brewer(palette = "Set6")
```

::: {.cell-output .cell-output-stderr}

```
Warning: Unknown palette: "Set6"
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-61-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified) +
  geom_bar(aes(x = grade, fill = gender)) +
  facet_wrap(~age_group) +
  labs(title = "Counts of Gender by Grade and Age Group") +
  scale_fill_brewer(palette = "Set4") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output .cell-output-stderr}

```
Warning: Unknown palette: "Set4"
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-62-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified) +
  geom_bar(aes(x = age_group, fill = gender)) +
  facet_wrap(~arith) +
  labs(
    title = "Counts of Gender by Age Group and Arithmetic Score",
    subtitle = "Is this plot arrangement easy to grasp?"
  ) +
  scale_fill_brewer(palette = "Set1") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-63-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified) +
  geom_bar(aes(x = arith, fill = gender)) +
  facet_wrap(~age_group) +
  labs(
    title = "Counts of Gender by Arithmetic Score and Age Group",
    subtitle = "Swapped the Facets"
  ) +
  scale_fill_brewer(palette = "Set3") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-64-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
gf_histogram(~amas, data = meth_modified) %>%
  gf_labs(
    title = "AMAS Scores",
    caption = "ggformula"
  )
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-65-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
gf_histogram(~amas,
  data = meth_modified,
  bins = 100
) %>%
  gf_labs(
    title = "AMAS Scores",
    caption = "ggformula"
  )
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-66-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_histogram(aes(x = rcmas), bins = 100) +
  labs(
    title = "RCMAS Scores",
    caption = "ggplot"
  )
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-67-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>%
  ggplot() +
  geom_histogram(aes(x = amas, fill = grade),
    colour = "black", alpha = 0.3
  ) +
  labs(title = "AMAS filled by Grade", caption = "ggplot")
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-68-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% ggplot() +
  geom_histogram(aes(amas, fill = grade),
    colour = "black", alpha = 0.3
  ) +
  facet_wrap(facets = vars(grade)) +
  labs(
    title = "AMAS by Filled and Facetted by Grade",
    caption = "ggplot"
  ) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-69-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified %>% ggplot() +
  geom_histogram(aes(amas, fill = grade),
    colour = "black", alpha = 0.3
  ) +
  facet_wrap(facets = vars(grade), scales = "free_y") +
  labs(
    title = "AMAS by Filled and Facetted by Grade",
    subtitle = "Free y-scale",
    caption = "ggplot"
  ) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

::: {.cell-output .cell-output-stderr}

```
`stat_bin()` using `bins = 30`. Pick better value `binwidth`.
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-70-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
# In tried something on my own!!
#i got help from https://www.rdocumentation.org/packages/shinyr/versions/0.4.2
ui <- fluidPage(
  titlePanel("Interactive Histogram of Math Anxiety Data"),
  sidebarLayout(
    sidebarPanel(
      selectInput("variable",
                  "Choose variable:",
                  choices = c("Age" = "age", "AMAS" = "amas", "RCMAS" = "rcmas")),
      sliderInput("bins",
                  "Number of bins:",
                  min = 10,
                  max = 50,
                  value = 30)
    ),
    mainPanel(
      plotOutput("distPlot")
    )
  )
)

server <- function(input, output) {
  output$distPlot <- renderPlot({
    meth_modified %>%
      ggplot(aes(x = .data[[input$variable]])) +
      geom_histogram(bins = input$bins, fill = "steelblue", color = "white", alpha = 0.7) +
      labs(
        title = paste("Histogram of", input$variable),
        x = input$variable,
        y = "Count"
      ) +
      theme_minimal()
  })
}

shinyApp(ui = ui, server = server)
```

::: {.cell-output-display}
`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified, aes(x = gender, y = amas, fill = gender)) +
  geom_boxplot() +
  labs(title = "AMAS Scores by Gender",
       x = "Gender",
       y = "AMAS Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-72-1.png){width=672}
:::
:::



::: {.cell}

```{.r .cell-code}
meth_modified
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 599 × 7
   gender grade       age  amas rcmas arith age_group
   <fct>  <fct>     <dbl> <dbl> <dbl> <fct> <fct>    
 1 Boy    Secondary  1378     9    20 6     <NA>     
 2 Boy    Secondary  1407    18     8 6     <NA>     
 3 Girl   Secondary  1379    23    26 5     <NA>     
 4 Girl   Secondary  1428    19    18 7     <NA>     
 5 Boy    Secondary  1356    23    20 1     <NA>     
 6 Girl   Secondary  1350    27    33 1     <NA>     
 7 Boy    Secondary  1336    22    23 4     <NA>     
 8 Boy    Secondary  1393    17    11 7     <NA>     
 9 Girl   Secondary  1317    28    32 2     <NA>     
10 Boy    Secondary  1348    20    30 6     <NA>     
# ℹ 589 more rows
```


:::
:::



I am not sure why meth_modified isn't loading the other variables

Recreate a cleaned dataset version with NA replacements and factor conversions for consistency.


::: {.cell}

```{.r .cell-code}
meth_modified2 <- meth %>%
  naniar::replace_with_na_all(condition = ~ .x %in% common_na_numbers) %>%
  naniar::replace_with_na_all(condition = ~ .x %in% common_na_strings) %>%
  mutate(
    gender = as.factor(gender),
    grade = as.factor(grade)
  ) %>%
    dplyr::relocate(where(is.factor), .before = age)
    glimpse(meth_modified2)
```

::: {.cell-output .cell-output-stdout}

```
Rows: 599
Columns: 6
$ gender <fct> Boy, Boy, Girl, Girl, Boy, Girl, Boy, Boy, Girl, Boy, Boy, Boy,…
$ grade  <fct> Secondary, Secondary, Secondary, Secondary, Secondary, Secondar…
$ age    <dbl> 1378, 1407, 1379, 1428, 1356, 1350, 1336, 1393, 1317, 1348, 141…
$ amas   <dbl> 9, 18, 23, 19, 23, 27, 22, 17, 28, 20, 16, 20, 21, 36, 16, 27, …
$ rcmas  <dbl> 20, 8, 26, 18, 20, 33, 23, 11, 32, 30, 10, 4, 23, 26, 24, 21, 3…
$ arith  <dbl> 6, 6, 5, 7, 1, 1, 4, 7, 2, 6, 2, 5, 2, 6, 2, 7, 2, 4, 7, 3, 8, …
```


:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = gender, y = amas, fill = gender)) +
  geom_boxplot() +
  labs(title = "AMAS Scores by Gender",
       x = "Gender",
       y = "AMAS Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-75-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
meth_modified2 %>%
  group_by(gender) %>%
  dplyr::summarise(across(
    .cols = c(age, amas, rcmas, arith),
    .fns = list(
      mean = ~ mean(., na.rm = T),
      sd = sd,
      min = min, max = max
    )
  ))
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 2 × 17
  gender age_mean age_sd age_min age_max amas_mean amas_sd amas_min amas_max
  <fct>     <dbl>  <dbl>   <dbl>   <dbl>     <dbl>   <dbl>    <dbl>    <dbl>
1 Boy       1276.   229.     931    1875      21.2    6.51        4       45
2 Girl      1211.   211.      37    1803      22.9    6.59        9       40
# ℹ 8 more variables: rcmas_mean <dbl>, rcmas_sd <dbl>, rcmas_min <dbl>,
#   rcmas_max <dbl>, arith_mean <dbl>, arith_sd <dbl>, arith_min <dbl>,
#   arith_max <dbl>
```


:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = grade, y = arith, fill = grade)) +
  geom_boxplot() +
  labs(title = "Arithmetic Performance by Grade",
       x = "Grade",
       y = "Arith Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-77-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = grade, y = rcmas, fill = grade)) +
  geom_boxplot() +
  labs(title = "RCMAS Scores by Grade",
       x = "Grade",
       y = "RCMAS Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-78-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = grade, y = amas, fill = grade)) +
  geom_violin(trim = FALSE) +
  geom_boxplot(width = 0.1, fill = "white") +  # Optional: overlay boxplot for quartiles
  labs(title = "AMAS Scores Distribution by Grade",
       x = "Grade",
       y = "AMAS Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-79-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = gender, y = arith, fill = gender)) +
  geom_violin(trim = FALSE) +
  geom_boxplot(width = 0.1, fill = "white") +  # Optional: overlay boxplot
  labs(title = "Arithmetic Performance Distribution by Gender",
       x = "Gender",
       y = "Arith Score") +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-80-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = amas, y = arith)) +
  geom_point() +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Arithmetic Performance vs. AMAS",
       x = "AMAS Score",
       y = "Arith Score") +
  theme_minimal()
```

::: {.cell-output .cell-output-stderr}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-81-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = amas, y = rcmas)) +
  geom_point() +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "RCMAS vs. AMAS",
       x = "AMAS Score",
       y = "RCMAS Score") +
  theme_minimal()
```

::: {.cell-output .cell-output-stderr}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-82-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = age, y = arith)) +
  geom_point() +
  geom_smooth(method = "lm", color = "blue") +
  labs(title = "Arithmetic Performance vs. Age (Years)",
       x = "Age (Years)",
       y = "Arith Score") +
  theme_minimal()
```

::: {.cell-output .cell-output-stderr}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-83-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = gender, y = amas, fill = gender)) +
  geom_boxplot() +
  scale_fill_manual(values = c('Boy' = 'skyblue', 'Girl' = 'lightcoral')) +
  labs(title = 'Math Anxiety (AMAS) Distribution by Gender',
       x = 'Gender',
       y = 'AMAS Score (Math Anxiety)') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-84-1.png){width=672}
:::
:::



::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = grade, y = arith, fill = grade)) +
  geom_boxplot() +
  scale_fill_manual(values = c('Primary' = 'lightgreen', 'Secondary' = 'gold')) +
  labs(title = 'Arithmetic Score (Arith) Distribution by Grade',
       x = 'Grade Level',
       y = 'Arithmetic Score (0-8)') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-85-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = amas, y = arith, color = grade)) +
  geom_point(alpha = 0.4) +
  geom_smooth(method = 'lm', se = FALSE) +
  scale_color_manual(values = c('Primary' = 'darkgreen', 'Secondary' = 'darkred')) +
  labs(title = 'Relationship between Math Anxiety and Performance by Grade',
       x = 'AMAS Score (Math Anxiety)',
       y = 'Arithmetic Score (0-8)',
       color = 'Grade Level') +
  theme_minimal()
```

::: {.cell-output .cell-output-stderr}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-86-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = amas)) +
  geom_histogram(bins = 20, fill = 'darkblue', color = 'black') +
  geom_density(aes(y = after_stat(density) * (max(meth_modified2$amas) - min(meth_modified2$amas)) / 20), color = 'black', linewidth = 1) +
  geom_vline(aes(xintercept = mean(amas)), color = 'red', linetype = 'dashed') +
  annotate('text', x = mean(meth_modified2$amas) + 2, y = 50, label = paste('Mean:', round(mean(meth_modified2$amas), 2))) +
  labs(title = 'Distribution of Math Anxiety (AMAS) Score',
       x = 'AMAS Score',
       y = 'Count') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-87-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = rcmas)) +
  geom_histogram(bins = 20, fill = 'darkgreen', color = 'black') +
  geom_density(aes(y = after_stat(density) * (max(meth_modified2$rcmas) - min(meth_modified2$rcmas)) / 20), color = 'black', linewidth = 1) +
  geom_vline(aes(xintercept = mean(rcmas)), color = 'red', linetype = 'dashed') +
  annotate('text', x = mean(meth_modified2$rcmas) + 2, y = 50, label = paste('Mean:', round(mean(meth_modified2$rcmas), 2))) +
  labs(title = 'Distribution of General Anxiety (RCMAS) Score',
       x = 'RCMAS Score',
       y = 'Count') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-88-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
mean_scores <- meth_modified2 %>%
  group_by(grade) %>%
  summarise(amas = mean(amas), rcmas = mean(rcmas)) %>%
  pivot_longer(cols = c(amas, rcmas), names_to = 'Anxiety_Type', values_to = 'Mean_Score')

ggplot(mean_scores, aes(x = grade, y = Mean_Score, fill = Anxiety_Type)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  scale_fill_manual(values = c('amas' = 'darkorange', 'rcmas' = 'grey')) +
  labs(title = 'Mean Math vs. General Anxiety by Grade Level',
       x = 'Grade Level',
       y = 'Mean Anxiety Score',
       fill = 'Anxiety Type') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-89-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = grade, y = rcmas, fill = grade)) +
  geom_boxplot() +
  scale_fill_manual(values = c('Primary' = 'darkgreen', 'Secondary' = 'gold')) +
  labs(title = 'RCMAS Distribution by Grade',
       x = 'Grade Level',
       y = 'RCMAS Score (General Anxiety)') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-90-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
ggplot(meth_modified2, aes(x = gender, y = rcmas, fill = gender)) +
  geom_boxplot() +
  scale_fill_manual(values = c('Boy' = 'skyblue', 'Girl' = 'lightcoral')) +
  labs(title = 'RCMAS Distribution by Gender',
       x = 'Gender',
       y = 'RCMAS Score (General Anxiety)') +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-91-1.png){width=672}
:::
:::


::: {.cell}

```{.r .cell-code}
arith_means <- meth_modified2 %>%
  group_by(grade, gender) %>%
  summarise(arith = mean(arith), .groups = 'drop')

ggplot(arith_means, aes(x = grade, y = arith, fill = gender)) +
  geom_bar(stat = 'identity', position = 'dodge') +
  scale_fill_manual(values = c('Boy' = 'dodgerblue', 'Girl' = 'red')) +
  labs(title = 'Mean Arithmetic Score (Arith) by Grade and Gender',
       x = 'Grade Level',
       y = 'Mean Arithmetic Score (0-8)',
       fill = 'Gender') +
  ylim(0, 8) +
  theme_minimal()
```

::: {.cell-output-display}
![](Mathanxiety_files/figure-html/unnamed-chunk-92-1.png){width=672}
:::
:::


## Hypothesis

This section outlines potential hypotheses from data patterns, ready for future testing.

1.  Math anxiety causes poor arithmetic performance: High AMAS likely leads to low Arith scores, not vice versa. Test this with long-term studies or programs that reduce anxiety and check for performance gains.

2.  Gender moderates anxiety-performance link: The negative AMAS-Arith tie hits girls harder, thanks to stereotype threats boosting anxiety's effects. This fits the AMAS gender gap despite similar Arith scores.

3.  Math anxiety mediates grade performance drop: AMAS partly explains falling Arith from primary to secondary, as tougher lessons raise anxiety and hurt scores, matching slight AMAS rises and Arith falls.

4.  Different paths for math vs general anxiety: AMAS grows with math stressors like tests, while RCMAS stays steady as a personal trait. This explains higher AMAS overall, without grade or gender shifts in RCMAS.

5.  Age nonlinearly predicts math anxiety: No straight-line pattern, but peaks hit during shifts like ages 10-12 entering secondary school, due to puberty or school changes. The steady gender gap shows it's ongoing yet variable.

6.  General and math anxiety interact: High RCMAS worsens AMAS in secondary students amid tougher demands, compounding Arith impacts. This comes from small rises in both anxieties there.

7.  Sociocultural factors drive gender gap: Things like parent hopes or media views of math as male boost girls' AMAS, unrelated to performance. Add factors like income or culture to datasets for deeper looks.

## Insights

This section shares key takeaways and wider implications from the data and visuals.

### Expanded Inferences

1.  Girls face more math anxiety: Box plots show higher medians, wider ranges, and upper outliers for girls' AMAS. Means are 22.93 vs 21.17 for boys, lasting across ages, hinting at early social roots like stereotypes causing self-doubt. Scatter plots add that despite similar Arith trends, girls' higher AMAS gives them a tougher start, possibly worsened by teacher biases.

2.  Secondary students lag in arithmetic: Box plots reveal lower medians and shifts for secondary Arith, around 4-5 vs 5-6 in primary. Scatters confirm clustering at low scores with same AMAS, suggesting buildup from abstract tasks and slight AMAS bumps (22.25 vs 21.85). Bar charts show equal drops for both genders, pointing to system issues like harder curricula or less support, creating an education "math cliff."

3.  General anxiety steady across grades: Box plots have overlapping medians and ranges, matching stats with little change (slight secondary uptick). Unlike AMAS, RCMAS seems innate, tied to home or genes (mean 19.24, SD 7.57), not school stress. Bar charts keep it below AMAS, so interventions should target math triggers separately, as RCMAS isn't the main academic hurdle.

4.  Math anxiety tops general by grade: Bar charts put mean AMAS over RCMAS in both levels (\~21.85 vs lower in primary, \~22.25 vs still lower in secondary). Histograms back higher AMAS mean (21.98, range 4-45) vs RCMAS (19.24, 1-41). This points to AMAS as a task-specific reaction disrupting focus, per attentional theory, especially in pressured secondary settings favoring math focused fixes over general ones.

5.  Anxiety scores skewed right: Histograms peak at means but tail high (up to 45 AMAS, 41 RCMAS), signaling a few very anxious kids. AMAS skews more, implying varied experiences with outliers from failures (SDs 6.60 vs 7.57). Scatters link high tails to low Arith, suggesting screen for risks above thresholds like AMAS 30 to catch issues early.

6.  Anxiety hurts performance negatively: Scatters show downhill trends in Arith vs AMAS across grades, primaries a bit higher. Correlation around -0.3 means each AMAS point drops Arith 0.1-0.2, possibly two-way: anxiety blocks focus or flops feed fear. Secondary slopes seem steeper from demands, tying to grade drops and calling for models to sort causes.

7.  Math anxiety trends developmentally: Line plots keep girls 1-2 points above boys, with flat or wobbly age bins (3.7-187.5 months, mean 124.65). This suggests early formation from experiences over growth, with nonlinear dips or peaks at milestones. Wide range hints extremes like low in young naivety, high in older stress; steady gender calls for kid-friendly programs soon.

8.  General anxiety uniform by gender and grade: Box plots lack gaps or shifts, unlike AMAS. This evenness across groups marks RCMAS as a core trait, not swayed by school or demo changes. While AMAS fluctuates, RCMAS stability (from histograms, bars) ties to temperament, so don't mix anxieties in fixes—high RCMAS may flag other problems but not explain scores.

9.  Arithmetic similar by gender, drops by grade: Bar charts show close boy-girl Arith means per grade, but both fall in secondary (\~5.5-6 to \~4-5). Despite girls' higher AMAS, this hints they compensate (like extra effort) at a hidden cost, risking future avoidance. Uniform drop blames environment like content gaps, with scatters showing subtle high-AMAS underperformance in girls.

10. Summary stats overview: 599 clean entries, 323 boys/276 girls, 401 primary/198 secondary; m

