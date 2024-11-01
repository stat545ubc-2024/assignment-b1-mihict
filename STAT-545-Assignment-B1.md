STAT 545 Assignment B1
================
2024-10-31

# Exercise 1: Make a Function and Exercise 2: Document your Function

I would like to create a function that identifies the minimum and
maximum values of a numeric vector, matrix, or dataset. I will add in an
option to specify na.rm with the default being na.rm = FALSE if none
specified, and an option to specify additional arguements by adding in
elipses.

``` r
#' Identify Minimum and Maximum Values
#'
#' This function identifies the minimum and maximum values in a numeric dataset
#'
#' @param x A numeric vector, matrix, or data frame from which to find the
#' minimum and maximum values.
#' @return A list containing the minimum and maximum values.
#' @examples
#' # Example usage
#' my_data <- c(3, 5, 2, 8, 1, NA)
#' result <- min_max(my_data)
#' print(result)
#' @export
min_max <- function(x, na.rm=FALSE, ...) {
  if (!is.numeric(x)) {
    stop("Input must be a numeric vector, matrix, or data frame")
  }
  min_value <- min(x, na.rm = na.rm)
  max_value <- max(x, na.rm = na.rm)
  return(list(min = min_value, max = max_value))
}
```

# Exercise 3: Include examples

Now I will test my function with a few examples.

### Example 1: Penguins Bill Length

Using the palmerspenguins data set, I will find the minimum and maximum
values for bill length.

``` r
library(palmerpenguins)
data("penguins")
min_max(penguins$bill_length_mm, na.rm=TRUE)
```

    ## $min
    ## [1] 32.1
    ## 
    ## $max
    ## [1] 59.6

We can see that the shortest bill length is 32.1 mm and the longest is
59.6 mm

### Example 2: Penguins Body Mass

We can also use the same function to find the minimum and maximum body
mass.

``` r
min_max(penguins$body_mass_g, na.rm=TRUE)
```

    ## $min
    ## [1] 2700
    ## 
    ## $max
    ## [1] 6300

We see that the lightest penguin weighs 2700g and the heaviest weighs
6300g.

### Example 3: Invalid Input (Categorical Variable)

On the other hand, if we try to use the function with a categorical
variable such as sex, it will return an error and let us know that only
a numeric vector, matrix, and data frames can be used:

``` r
min_max(penguins$sex, na.rm=TRUE)
```

    ## Error in min_max(penguins$sex, na.rm = TRUE): Input must be a numeric vector, matrix, or data frame

# Exercise 4: Test the Function

Now we will test our function 3 times to ensure that it works in a
variety of settings.

First we will load the “testthat” package.

``` r
library(testthat)
```

### Test 1: Data without NAs

First we will use numerical data that does not contain NA values.
Whether or not we specify that na.rm = TRUE or FALSE, the output should
still be the same minimum and maximum value of the data set.

``` r
test_that("min_max function handles numeric data without NAs correctly", {
   # Test without specifying na.rm (default is na.rm = FALSE)
  my_data_no_na <- c(4, 10, 6, 12)
  result_no_na <- min_max(my_data_no_na)
  expect_equal(result_no_na$min, 4)  # Check minimum
  expect_equal(result_no_na$max, 12) # Check maximum

  # Test with specifying na.rm = TRUE 
  result_no_na2 <- min_max(my_data_no_na, na.rm = TRUE)
  expect_equal(result_no_na2$min, 4)  # Check minimum
  expect_equal(result_no_na2$max, 12) # Check maximum
  })
```

    ## Test passed 🎉

As we can see, when we don’t have any NA values, whether or not we
specify na.rm = TRUE we still get the same correct output with our
function.

### Test 2: Data with NAs

Now let’s test if the function still works correctly if the data set
contains NAs. If we don’t specify na.rm = TRUE, the default is FALSE and
we would expect the minimum and maximum values to equal NA. If we do
specify na.rm = TRUE, then we would expect to have the true minimum and
maximum values listed.

``` r
test_that("min_max function handles numeric data with NAs correctly", {
   # Test without specifying na.rm (default is na.rm = FALSE)
  my_data_na <- c(4, 10, 6, 12, NA)
  result_na <- min_max(my_data_na)
  expect_true(is.na(result_na$min))  # Confirm minimum is NA
  expect_true(is.na(result_na$max))   # Confirm maximum is NA

  # Test with specifying na.rm = TRUE 
  result_na2 <- min_max(my_data_na, na.rm = TRUE)
  expect_equal(result_na2$min, 4)  # Check minimum
  expect_equal(result_na2$max, 12) # Check maximum
  })
```

    ## Test passed 🎊

As we can see above, the function appropriately handles data with NA
values whether or not na.rm = TRUE is specified.

### Test 3: Vector of a Different Type

Now we will test to see if the function appropriately manages a
non-numeric vector and generates the “Input must be a numeric vector,
matrix, or data frame” message we have coded.

``` r
test_that("min_max function displays an error for categorical input", {
  categorical_data <- c("a", "b", "c")
  expect_error(min_max(categorical_data), "Input must be a numeric vector, matrix, or data frame")
})
```

    ## Test passed 😀

As we can see above, the function appropriately generates and error
message if we try to use a categorical vector.

### All of our tests have passed and our function is ready for use!
