# Get DataPackageR extdata path

Get DataPackageR extdata path

## Usage

``` r
project_extdata_path(file = NULL)
```

## Arguments

- file:

  `character` or `NULL` (default).

## Value

`character`

## Details

Returns the path to the data package extdata subdirectory, or constructs
a path to a file in the extdata subdirectory from the file argument.

## Examples

``` r
if(rmarkdown::pandoc_available()){
project_extdata_path(file = "mydata.csv")
}
#> [1] "/tmp/RtmpCXunvE/file63f23833e35/inst/extdata/mydata.csv"
```
