
<!-- README.md is generated from README.Rmd. Please edit that file -->

# virgo

<!-- badges: start -->

<!-- badges: end -->

The **virgo** package enables the creation of interactive graphics for
exploratory data analysis. It is an *idiomatic and opinionated* R
interface to the grammar of graphics implemented by
[**Vega-Lite**](https://vega.github.io/vega-lite/) which defines the
following elements:

  - graphical elements like `mark_point()`, with the `mark_*` family of
    functions
  - interactive elements, such as brushes (using `select_interval()`)
    and sliders (using `input_slider()`), via the `select_*` and
    `input_*` family of functions
  - aesthetic mappings/encodings via `enc()`
  - data transformations via [selections]()
  - plot composition via faceting and concatenation using
    `facet_views()`, `hconcat()` and `vconcat()`

## Installation

<!--
You can install the released version of **virgo** from [CRAN](https://CRAN.R-project.org) with:

``` r
install.packages("virgo")
```

-->

You can install the development version of **virgo** from
[GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("vegawidget/virgo")
```

## Get started

For most graphics using **virgo**, you start off by passing data to the
`vega()` function, add graphical elements with marks like
`mark_point()`, and specify variables within a mark using encodings
`enc()`. You can add more layers by specifying additional marks like
`mark_smooth()`, or include small multiples with `facet_views()` or
combine plots or add interactive elements with selections.

Let’s see an example, here we show how we can compose a simple scatter
plot and gradually build up to a scatter plot with brushing, to a side
by side scatter plot.

``` r
library(virgo)
library(palmerpenguins)
p <- penguins %>% 
  vega() %>% 
  mark_circle(
    enc(
      x = bill_length_mm, 
      y = bill_depth_mm
    )
  )
p
```

Interactive elements are generated using selections, for example, we can
generate a rectangular brush with `select_interval()` and then highlight
points that fall into the brush using `encode_if()`:

``` r
selection <- select_interval()

p <- penguins %>% 
  vega() %>% 
  mark_circle(
    enc(
      x = bill_length_mm, 
      y = bill_depth_mm, 
      color = encode_if(selection, species, "black")
    )
  )
p
```

Now we generate a second scatter plot:

``` r
p_right <- penguins %>% 
  vega() %>% 
  mark_circle(
    enc(
      x = flipper_length_mm,
      y = body_mass_g,
      color = encode_if(selection, species, "black")
    )
  )
p_right
```

And link them side by side using concatenation.

``` r
hconcat(p, p_right)
```

And that’s just the beginning of what’s possible\!

## Learning more

  - [Example gallery]()
  - [Using **virgo** to explore Melbourne’s microclimate]()
  - [Guide to **virgo** for **ggplot2** users]()
  - [Composing plot interactions with selections]()

## Lifecycle

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)

The **virgo** package is under rapid development and we are still
working through our ideas for incorporating interactive graphics into
exploratory data analysis. If you have feedback we would love to hear
it\!

## Acknowledgements

  - Vega/Vega-Lite developers
  - Ian Lyttle, Hayley Jepson and Alicia Schep for their foundational
    work in the
    [**vegawidget**](https://vegawidget.github.io/vegawidget/) package
