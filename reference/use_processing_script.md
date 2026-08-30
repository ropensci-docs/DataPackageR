# Add a processing script to a data package.

The Rmd or R file or directory specified by `file` will be moved into
the data-raw directory. It will also be added to the yml configuration
file. Any existing file by that name will be overwritten when overwrite
is set to TRUE

## Usage

``` r
use_processing_script(
  file = NULL,
  title = NULL,
  author = NULL,
  overwrite = FALSE
)
```

## Arguments

- file:

  `character` path to an existing file or name of a new R or Rmd file to
  create.

- title:

  `character` title of the processing script for the yaml header. Used
  only if file is being created.

- author:

  `character` author name for the yaml header. Used only if the file is
  being created.

- overwrite:

  `logical` default FALSE. Overwrite existing file of the same name.

## Value

invisibly returns TRUE for success. Stops on failure.

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
use_processing_script(file = "newScript.Rmd",
    title = "Processing a new dataset",
    author = "Y.N. Here.")
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/test".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/datatest".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied extra.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
#> Attempting to create  newScript.Rmdconfiguration:
#>   files:
#>     extra.Rmd:
#>       enabled: yes
#>     newScript.Rmd:
#>       enabled: yes
#>   objects: data
#>   render_root:
#>     tmp: '879475'
```
