# Add a data object to a data package.

The data object will be added to the yml configuration file.

## Usage

``` r
use_data_object(object_name = NULL)
```

## Arguments

- object_name:

  Name of the data object. Should be created by a processing script in
  data-raw. `character` vector of length 1.

## Value

invisibly returns TRUE for success.

## Examples

``` r
if(rmarkdown::pandoc_available()){
myfile <- tempfile()
file <- system.file("extdata", "tests", "extra.Rmd",
                     package = "DataPackageR")
datapackage_skeleton(
  name = "datatest",
  path = tempdir(),
  code_files = file,
  force = TRUE,
  r_object_names = "data")
use_data_object(object_name = "newobject")
}
#> ✔ Creating /tmp/RtmpwtfBDE/datatest/.
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e5cd657fe".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/datatest".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied extra.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
#> configuration:
#>   files:
#>     extra.Rmd:
#>       enabled: yes
#>   objects:
#>   - data
#>   - newobject
#>   render_root:
#>     tmp: '92632'
```
