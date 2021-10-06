This template demonstrates many of the bells and whistles of the `reprex::reprex_document()` output format. The YAML sets many options to non-default values, such as using `#;-)` as the comment in front of output.

## Code style

Since `style` is `TRUE`, this difficult-to-read code (look at the `.Rmd` source file) will be restyled according to the Tidyverse style guide when it’s rendered. Whitespace rationing is not in effect!

``` r
x=1;y=2;z=x+y;z
#;-) [1] 3
```

## Quiet tidyverse

The tidyverse meta-package is quite chatty at startup, which can be very useful in exploratory, interactive work. It is often less useful in a reprex, so by default, we suppress this.

However, when `tidyverse_quiet` is `FALSE`, the rendered result will include a tidyverse startup message about package versions and function masking.

``` r
library(tidyverse)
#;-) ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
#;-) ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
#;-) ✓ tibble  3.1.4     ✓ dplyr   1.0.7
#;-) ✓ tidyr   1.1.3     ✓ stringr 1.4.0
#;-) ✓ readr   2.0.1     ✓ forcats 0.5.1
#;-) ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
#;-) x dplyr::filter() masks stats::filter()
#;-) x dplyr::lag()    masks stats::lag()
```

## Chunks in languages other than R

Remember: knitr supports many other languages than R, so you can reprex bits of code in Python, Ruby, Julia, C++, SQL, and more. Note that, in many cases, this still requires that you have the relevant external interpreter installed.

Let’s try Python!

``` python
x = 'hello, python world!'
print(x.split(' '))
#;-) ['hello,', 'python', 'world!']
```

And bash!

``` bash
echo "Hello Bash!";
pwd;
ls | head;
#;-) Hello Bash!
#;-) /Users/ewiniar/STAT-240/lecture
#;-) 02-examples.Rmd
#;-) lec1.Rmd
#;-) lec1_reprex.Rmd
#;-) lec1_reprex_std_out_err.txt
```

Write a function in C++, use Rcpp to wrap it and …

``` cpp
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericVector timesTwo(NumericVector x) {
  return x * 2;
}
```

then immediately call your C++ function from R!

``` r
timesTwo(1:4)
#;-) [1] 2 4 6 8
```

## Standard output and error

Some output that you see in an interactive session is not actually captured by rmarkdown, when that same code is executed in the context of an `.Rmd` document. When `std_out_err` is `TRUE`, `reprex::reprex_render()` uses a feature of `callr:r()` to capture such output and then injects it into the rendered result.

Look for this output in a special section of the rendered document (and notice that it does not appear right here).

``` r
system2("echo", args = "Output that would normally be lost")
```

## Session info

Because `session_info` is `TRUE`, the rendered result includes session info, even though no such code is included here in the source document.

<details style="margin-bottom:10px;">
<summary>
Standard output and standard error
</summary>

``` sh
x Install the styler package in order to use `style = TRUE`.
running: python  -c "x = 'hello, python world!'
print(x.split(' '))"
running: bash  -c 'echo "Hello Bash!";
pwd;
ls | head;'
Building shared library for Rcpp code chunk...
Output that would normally be lost
```

</details>
<details style="margin-bottom:10px;">
<summary>
Session info
</summary>

``` r
sessionInfo()
#;-) R version 4.1.1 (2021-08-10)
#;-) Platform: aarch64-apple-darwin20 (64-bit)
#;-) Running under: macOS Big Sur 11.5.2
#;-) 
#;-) Matrix products: default
#;-) BLAS:   /Library/Frameworks/R.framework/Versions/4.1-arm64/Resources/lib/libRblas.0.dylib
#;-) LAPACK: /Library/Frameworks/R.framework/Versions/4.1-arm64/Resources/lib/libRlapack.dylib
#;-) 
#;-) locale:
#;-) [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
#;-) 
#;-) attached base packages:
#;-) [1] stats     graphics  grDevices utils     datasets  methods   base     
#;-) 
#;-) other attached packages:
#;-) [1] forcats_0.5.1   stringr_1.4.0   dplyr_1.0.7     purrr_0.3.4    
#;-) [5] readr_2.0.1     tidyr_1.1.3     tibble_3.1.4    ggplot2_3.3.5  
#;-) [9] tidyverse_1.3.1
#;-) 
#;-) loaded via a namespace (and not attached):
#;-)  [1] tidyselect_1.1.1 xfun_0.25        haven_2.4.3      colorspace_2.0-2
#;-)  [5] vctrs_0.3.8      generics_0.1.0   htmltools_0.5.2  yaml_2.2.1      
#;-)  [9] utf8_1.2.2       rlang_0.4.11     pillar_1.6.2     glue_1.4.2      
#;-) [13] withr_2.4.2      DBI_1.1.1        dbplyr_2.1.1     readxl_1.3.1    
#;-) [17] modelr_0.1.8     lifecycle_1.0.0  cellranger_1.1.0 munsell_0.5.0   
#;-) [21] gtable_0.3.0     rvest_1.0.1      evaluate_0.14    knitr_1.33      
#;-) [25] tzdb_0.1.2       fastmap_1.1.0    fansi_0.5.0      broom_0.7.9     
#;-) [29] Rcpp_1.0.7       backports_1.2.1  scales_1.1.1     jsonlite_1.7.2  
#;-) [33] fs_1.5.0         hms_1.1.0        digest_0.6.27    stringi_1.7.4   
#;-) [37] grid_4.1.1       cli_3.0.1        tools_4.1.1      magrittr_2.0.1  
#;-) [41] crayon_1.4.1     pkgconfig_2.0.3  ellipsis_0.3.2   xml2_1.3.2      
#;-) [45] reprex_2.0.1     lubridate_1.7.10 assertthat_0.2.1 rmarkdown_2.10  
#;-) [49] httr_1.4.2       rstudioapi_0.13  R6_2.5.1         compiler_4.1.1
```

</details>
