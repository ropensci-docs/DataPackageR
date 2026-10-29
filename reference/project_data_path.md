# Get DataPackageR data path

Get DataPackageR data path

## Usage

``` r
project_data_path(file = NULL)
```

## Arguments

- file:

  `character` or `NULL` (default).

## Value

`character`

## Details

Returns the path to the data package data subdirectory, or constructs a
path to a file in the data subdirectory from the file argument.

## Examples

``` r
if(rmarkdown::pandoc_available()){
project_data_path( file = "data.rda" )
}
#> [1] "/tmp/RtmpCXunvE/file63f23833e35/data/data.rda"
```
