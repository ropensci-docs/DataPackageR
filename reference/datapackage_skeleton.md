# Create a Data Package skeleton for use with DataPackageR.

Creates a package skeleton directory structure for use with
DataPackageR. Adds the DataVersion string to DESCRIPTION, creates the
DATADIGEST file, and the data-raw directory. Updates the
Read-and-delete-me file to reflect the additional necessary steps.

## Usage

``` r
datapackage_skeleton(
  name = NULL,
  path = ".",
  force = FALSE,
  code_files = character(),
  r_object_names = character(),
  raw_data_dir = character(),
  dependencies = character()
)
```

## Arguments

- name:

  `character` name of the package to create.

- path:

  A `character` path where the package is located. See
  [`package.skeleton`](https://rdrr.io/r/utils/package.skeleton.html)

- force:

  `logical` Force the package skeleton to be recreated even if it
  exists. see
  [`package.skeleton`](https://rdrr.io/r/utils/package.skeleton.html)

- code_files:

  Optional `character` vector of paths to Rmd files that process raw
  data into R objects.

- r_object_names:

  `vector` of quoted r object names , tables, etc. created when the
  files in `code_files` are run.

- raw_data_dir:

  `character` pointing to a raw data directory. Will be moved with all
  its subdirectories to "inst/extdata"

- dependencies:

  `vector` of `character`, paths to R files that will be moved to
  "data-raw" but not included in the yaml config file. e.g., dependency
  scripts.

## Value

No return value, called for side effects

## Examples

``` r
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f,"foo.Rmd")
con <- file(f)
writeLines("```{r}\n tbl = data.frame(1:10) \n```\n",con=con)
close(con)
pname <- basename(tempfile())
datapackage_skeleton(name = pname,
   path = tempdir(),
   force = TRUE,
   r_object_names = "tbl",
   code_files = f)
   }
#> ✔ Creating /tmp/RtmpwtfBDE/file59e44bbc742/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e44bbc742".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e44bbc742
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e7ec88b3c".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e44bbc742".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied foo.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
```
