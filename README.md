# Forget About For Loops
Hedia Tnani

## Purrr Package

The **`purrr`** package in R is part of the **`tidyverse`**, a
collection of R packages designed for data science. It provides a
functional programming toolkit to simplify many common data manipulation
tasks. The package aims to make your code more readable, more
maintainable, and easier to reason about by allowing you to use
functions as first-class objects.

Here’s a quick overview of some of the key functionalities:

1.  **Mapping Functions**: **`map()`**, **`map_df()`**, **`map_dbl()`**,
    etc. apply a function to each element of a list or vector and return
    results in various forms (list, data frame, double, etc.).

2.  **Predicate Functions**: Functions like **`keep()`**,
    **`discard()`**, etc., help to filter elements from a list based on
    a condition.

3.  **Functional Programming Tools**: **`compose()`**, **`partial()`**,
    etc., help in function composition and creating partial functions.

4.  **Iteration Functions**: **`walk()`**, **`walk2()`**, etc., allow
    you to iterate over multiple arguments in lists.

5.  **Reduction Functions**: **`reduce()`**, **`accumulate()`**, etc.,
    help to reduce a list to a single value by iteratively applying a
    function.

## **1. The basics of purrr**

To get started, map is the workhorse function of the purrr package. map
and apply basically take on the tasks of a for loop. Suppose we wanted
to accomplish the following task.

``` r
genes <- paste0("Gene", 1:10)
genes
```

     [1] "Gene1"  "Gene2"  "Gene3"  "Gene4"  "Gene5"  "Gene6"  "Gene7"  "Gene8" 
     [9] "Gene9"  "Gene10"

Let’s make a `for` loop through this vector and print the genes.

``` r
for (i in seq_along(genes)){
  
  print(genes[i])
  
}
```

    [1] "Gene1"
    [1] "Gene2"
    [1] "Gene3"
    [1] "Gene4"
    [1] "Gene5"
    [1] "Gene6"
    [1] "Gene7"
    [1] "Gene8"
    [1] "Gene9"
    [1] "Gene10"

**For Loop**:

It takes elements one by one from a vector called **`genees`** and then
applies the **`print()`** function to them. In this case, the loop
iterates over the indices of the **`genes`** .

-   **Step 1**: **`i = 1`** -\> **`print(genes[1])`**

-   **Step 2**: **`i = 2`** -\> **`print(genes[2])`**

-   **...**

-   **Step n**: **`i = n`** -\> **`print(genes[n])`**

Rather than use a loop, we could accomplish the same task using `lapply`
. What we are doing here is applying the function `print` over each of
the elements in `genes` .

``` r
print_genes <-  lapply(genes, print)
```

    [1] "Gene1"
    [1] "Gene2"
    [1] "Gene3"
    [1] "Gene4"
    [1] "Gene5"
    [1] "Gene6"
    [1] "Gene7"
    [1] "Gene8"
    [1] "Gene9"
    [1] "Gene10"

And lastly using `map` from `purrr`

``` r
library(purrr)
genes_map <-  map(genes, print)
```

    [1] "Gene1"
    [1] "Gene2"
    [1] "Gene3"
    [1] "Gene4"
    [1] "Gene5"
    [1] "Gene6"
    [1] "Gene7"
    [1] "Gene8"
    [1] "Gene9"
    [1] "Gene10"

**Purrr’s map**: The **`genes_map <- map(genes, print)`** could be
represented as a single operation that applies **`print()`** to each
element of **`genes`** and returns a new list **`genes_map`**.

**Single Operation**: **`map(genes, print)`** -\> Returns
**`genes_map`**

``` r
class(genes_map)
```

    [1] "list"

### **1.1 Mapping Functions**

#### **`map()`**

Now that we have an idea of what `map` does, let’s dig into it a bit
more.

The basic syntax works in the manner

-   `map("vector to apply function to","Function to apply across vectors","Additional parameters")`

The **`map()`** function applies a function to each element of a list or
vector, and return a list.

``` r
# Double each element in the vector
double_it <- function(x) x * 2
map(1:5, double_it)
```

    [[1]]
    [1] 2

    [[2]]
    [1] 4

    [[3]]
    [1] 6

    [[4]]
    [1] 8

    [[5]]
    [1] 10

While we are discussing return types, let’s introduce some of the other
`map` functions. They are:

-   `map_dbl()`: Returns a `numeric` vector.

-   `map_int()`: Returns an `integer` vector.

-   `map_lgl()`: Returns a `logical` vector.

-   `map_chr()`: Returns a `character` vector.

-   `map_df()`: Returns a `data frame` .

### Exercice1

Your task is to explore other **`map`** variants that return different
types of output. Specifically, try to guess and then verify which of the
following functions you can use other than `map`:

1.  **`map_int`**

2.  **`map_dbl`**

3.  **`map_chr`**

4.  **`map_lgl`**

5.  **`map_df`**

### **Questions:**

1.  Which **`map`** variant should you use if you want the output to be
    a numeric vector?

2.  What happens if you use **`map_int`** with the **`double_it`**
    function? Is the output as expected?

3.  Can you use **`map_chr`** or **`map_lgl`** with the **`double_it`**
    function? What happens?

4.  What is the utility of each **`map`** variant based on the type of
    output they produce?

#### `map2()`

The **`map2`** function from the **`purrr`** package in R is used to
apply a function to pairs of elements from two lists or vectors. The
function takes three main arguments:

-   **`.x`**: The first list or vector

-   **`.y`**: The second list or vector

-   **`.f`**: The function to apply

Both **`.x`** and **`.y`** should have the same length, and the function
**`.f`** is applied to corresponding elements from **`.x`** and
**`.y`**.

Here’s an example to illustrate how **`map2`** works. Let’s say we have
two numeric vectors **`vec1`** and **`vec2`**, and we want to multiply
corresponding elements from these vectors.

``` r
# Define two numeric vectors
vec1 <- c(1, 2, 3, 4, 5)
vec2 <- c(6, 7, 8, 9, 10)

# Define a function to multiply two numbers
multiply_int <- function(x, y) {
  return(x * y)
}
```

Give me the corresponding `map` function to this for loop

``` r
# Initialize an empty numeric vector to store the results
result <- numeric(length(vec1))

# Loop through the vectors and apply the function multiply_int
for (i in seq_along(vec1)) {
  result[i] <- multiply_int(vec1[i], vec2[i])
}

# Print the result
print(result)
```

    [1]  6 14 24 36 50

Use **`map2`** to multiply corresponding elements from **`vec1`** and
**`vec2`**.

``` r
# Use map2 to apply the function
result <- ---(---, ---, ---)

# Print the result
print(result)
```

Just like the **`map`** function has several variants to produce
different types of output (**`map_dbl`**, **`map_int`**, **`map_chr`**,
etc.), **`map2`** also has similar variants:

-   **`map2_dbl`**: Returns a numeric vector.

-   **`map2_int`**: Returns an integer vector.

-   **`map2_chr`**: Returns a character vector.

-   **`map2_lgl`**: Returns a logical vector.

-   **`map2_df`**: Returns a data frame.

These variants work the same way as **`map2`**, but they simplify the
output to a specific type instead of a list.

#### `pmap()`

The **`pmap`** function is used when you have
`more than two lists or vectors` and you want to apply a function to
corresponding elements from each list or vector.

#### Example 1

Let’s create an example where we have three vectors, and we want to find
the sum of corresponding elements from these vectors.

``` r
# Define three numeric vectors
vec1 <- c(1, 2, 3)
vec2 <- c(4, 5, 6)
vec3 <- c(7, 8, 9)
```

We’ll first put them into a list.

``` r
# Create a list containing the vectors
list_of_vecs <- list(vec1, vec2, vec3)
```

We’ll define a function to find the sum of corresponding elements.

``` r
# Define a function to find the sum of three numbers
sum_it <- function(x, y, z) {
  return(x + y + z)
}
```

and we’ll apply this function to these vectors using **`pmap.`**

``` r
# Use pmap to apply the function
result <- pmap_dbl(list_of_vecs, sum_it)

# Print the result
print(result)
```

    [1] 12 15 18

In this example, **`pmap_dbl`** will return a numeric vector containing
the sums of corresponding elements from **`vec1`**, **`vec2`**, and
**`vec3`**.

The **`pmap`** function in the **`purrr`** package also has variants
similar to **`map`** and **`map2`**, designed to simplify the output to
a specific type:

-   **`pmap_dbl`**: Returns a numeric vector.

-   **`pmap_int`**: Returns an integer vector.

-   **`pmap_chr`**: Returns a character vector.

-   **`pmap_lgl`**: Returns a logical vector.

-   **`pmap_df`**: Returns a data frame.

#### Example2

Let’s say you have a list of data frames, and each data frame has the
same structure—two columns named **`A`** and **`B`**. You want to create
a new column **`Sum`** in each data frame, which is the sum of columns
**`A`** and **`B`**.

First, let’s create a list of data frames:

``` r
# Load the necessary packages
library(purrr)
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
library(tibble)

# Create three data frames
df1 <- tibble(A = c(1, 2, 3), B = c(4, 5, 6))
df2 <- tibble(A = c(7, 8, 9), B = c(10, 11, 12))
df3 <- tibble(A = c(13, 14, 15), B = c(16, 17, 18))

# Create a list of data frames
list_of_dfs <- list(df1, df2, df3)
```

Now, let’s use **`pmap`** to create the new column **`Sum`** in each
data frame:

``` r
# Function to add a new column sum = A + B
add_column_Sum <- function(df) {
  df %>% mutate(Sum = A + B)
}

# Use pmap to apply the function to each data frame
new_list_of_dfs <- pmap(list_of_dfs, add_column_Sum)

# Show the first data frame in the new list to see the added column C
print(new_list_of_dfs[[1]])
```

Why are we getting an error?

-   When you have multiple lists (or vectors) and a function that can
    operate on multiple arguments, you can pass the lists directly to
    **`pmap`**.

-   When you have a single list (or a list of data frames) and a
    function that operates on a single argument, you need to wrap the
    list in another list to pass it to **`pmap`**.


#### `imap()`

The **`imap`** function in **`purrr`** is used to iterate over both the
elements and their indices in a list or vector. The function takes two
arguments: the first is the element, and the second is the index. This
can be very useful when you want to perform operations that depend on
the index as well as the value of each element.

Let’s say we have a list of sequences and you want to label each
sequence with a unique identifier based on its position in the list.

``` r
library(dplyr)
library(purrr)
# Create a list of DNA sequences
dna_sequences <- list(
  "ATCG",
  "GCTA",
  "TTAA",
  "CCGG"
)

# Use imap to add a unique identifier to each sequence
sequences_with_ids <- imap(dna_sequences, ~ tibble(sequence = .x, id = paste0("seq_", .y)))

# Print the modified list of sequences with identifiers
print(sequences_with_ids[[1]])
```

    # A tibble: 1 × 2
      sequence id   
      <chr>    <chr>
    1 ATCG     seq_1

In this example, each string in **`dna_sequences`** represents a DNA
sequence:

-   **`.x`** refers to the DNA sequence itself.

-   **`.y`** refers to the index of that sequence in the list.

We use **`imap`** to create a tibble for each sequence that contains the
sequence itself and a unique identifier **`id`**, which is generated
based on its index in the original list (using
**`paste0("seq_", .y)`**).

The result is a list of tibbles, each containing a DNA sequence and its
unique identifier, which could be useful for downstream analyses or for
merging with other datasets.

## References

1.  https://lcolladotor.github.io/jhustatcomputing2023/posts/17-loop-functions/
2.  https://www.r4epi.com/using-the-purrr-package
3.  http://talimi.se/r/purrr/
4.  https://rpubs.com/cliex159/867722
5.  https://www.weirdfishes.blog/blog/practical-purrr/
6.  https://onunicornsandgenes.blog/2021/08/08/using-r-plyr-to-purrr/
7.  https://www.r-bloggers.com/2018/09/using-purrrs-map-family-functions-in-dplyrmutate/
8.  https://henrywang.nl/transform-list-into-dataframe-with-tidyr-and-purrr/
9.  https://www.painblogr.org/2020-06-19-purring-through-exploratory-analyses.html
10. https://www.pluralsight.com/guides/explore-r-libraries:-purrr
11. https://lar-purrr.netlify.app/#75
12. https://www.gerkelab.com/blog/2018/09/import-directory-csv-purrr-readr/
13. https://dcl-prog.stanford.edu/purrr-parallel.html
    
