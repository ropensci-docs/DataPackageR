# Add a raw data set to inst/extdata

The file or directory specified by `path` will be moved into the
inst/extdata directory.

## Usage

``` r
use_raw_dataset(path = NULL, ignore = FALSE)
```

## Arguments

- path:

  `character` path to file or directory.

- ignore:

  `logical` whether to ignore the path or file in git and R build.

## Value

invisibly returns TRUE for success. Stops on failure.

## Examples

``` r
if(rmarkdown::pandoc_available()){
myfile <- tempfile()
file <- system.file("extdata", "tests", "extra.Rmd",
                     package = "DataPackageR")
raw_data <- system.file("extdata", "tests", "raw_data",
                        package = "DataPackageR")
datapackage_skeleton(
  name = "datatest",
  path = tempdir(),
  code_files = file,
  force = TRUE,
  r_object_names = "data")
use_raw_dataset(raw_data)
}
#> ✔ Creating ./.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/datatest".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: datatest
#> Title: What the Package Does (One Line, Title Case)
#> Version: 0.0.0.9000
#> Authors@R (parsed):
#>     * First Last <first.last@example.com> [aut, cre]
#> Description: What the package does (one paragraph).
#> License: `use_mit_license()`, `use_gpl3_license()` or friends to
#>     pick a license
#> Encoding: UTF-8
#> Roxygen: list(markdown = TRUE)
#> RoxygenNote: 8.1.0
#> ✔ Writing NAMESPACE.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/datatest".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied extra.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
```
