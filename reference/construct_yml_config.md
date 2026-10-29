# Construct a datapackager.yml configuration

Constructs a datapackager.yml configuration object from a vector of file
names and a vector of object names (all quoted). Can be written to disk
via `yml_write`. `render_root` is set to a randomly generated named
subdirectory of [`tempdir()`](https://rdrr.io/r/base/tempfile.html).

## Usage

``` r
construct_yml_config(code = NULL, data = NULL, render_root = NULL)
```

## Arguments

- code:

  A vector of filenames

- data:

  A vector of quoted object names

- render_root:

  The root directory where the package data processing code will be
  rendered. Defaults to is set to a randomly generated named
  subdirectory of [`tempdir()`](https://rdrr.io/r/base/tempfile.html).

## Value

a datapackager.yml configuration represented as an R object

## Examples

``` r
conf <- construct_yml_config(code = c('file1.rmd','file2.rmd'), data=c('object1','object2'))
tmp <- normalizePath(tempdir(), winslash = "/")
yml_write(conf,path=tmp)
```
