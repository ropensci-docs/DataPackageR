# Get DataPackageR Project Root Path

Get DataPackageR Project Root Path

## Usage

``` r
project_path(file = NULL)
```

## Arguments

- file:

  `character` or `NULL` (default).

## Value

`character`

## Details

Returns the path to the data package project root, or constructs a path
to a file in the project root from the file argument.

## Examples

``` r
if(rmarkdown::pandoc_available()){
project_path( file = "DESCRIPTION" )
}
#> [1] "/tmp/RtmpwtfBDE/file59e5cd657fe/DESCRIPTION"
```
