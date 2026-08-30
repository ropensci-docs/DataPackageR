# Pre-process, document and build a data package

Combines the preprocessing, documentation, and build steps into one.

## Usage

``` r
package_build(
  packageName = NULL,
  vignettes = FALSE,
  deps = TRUE,
  install = FALSE,
  ...
)
```

## Arguments

- packageName:

  `character` path to package source directory. Defaults to the current
  path when NULL.

- vignettes:

  `logical` specify whether to build vignettes. Default FALSE.

- deps:

  `logical` should we pass data objects into subsequent scripts? Default
  TRUE

- install:

  `logical` automatically install and load the package after building.
  Default FALSE

- ...:

  additional arguments passed to `install.packages` when `install=TRUE`.

## Value

Character vector. File path of the built package.

## Details

Note that if `package_build` returns an error when rendering an `.Rmd`
internally, but that same `.Rmd` can be run successfully manually using
[`rmarkdown::render`](https://pkgs.rstudio.com/rmarkdown/reference/render.html),
then the following code facilitates debugging. Set
`options(error = function(){ sink(); recover()})` before running
`package_build` . This will enable examination of the active function
calls at the time of the error, with output printed to the console
rather than `knitr`'s default sink. After debugging, evaluate
`options(error = NULL)` to revert to default error handling. See section
"22.5.3 RMarkdown" at <https://adv-r.hadley.nz/debugging.html> for more
details.

## Examples

``` r
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f,"foo.Rmd")
con <- file(f)
writeLines("```{r}\n tbl = data.frame(1:10) \n```\n",con=con)
close(con)
pname <- basename(tempfile())
datapackage_skeleton(name=pname,
   path=tempdir(),
   force = TRUE,
   r_object_names = "tbl",
   code_files = f)

package_build(file.path(tempdir(),pname), install = FALSE)
}
#> ✔ Creating /tmp/RtmpwtfBDE/file59e5cd657fe/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e5cd657fe".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e5cd657fe
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e4147b5b1".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e5cd657fe".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied foo.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
#> 
#> ✔ 1 data set(s) created by foo.Rmd
#> • tbl
#> ☘ Built all datasets!
#> Non-interactive NEWS.md file update.
#> * Added: tbl
#> 
#> ✔ Creating vignettes/.
#> ✔ Creating inst/doc/.
#> ℹ Setting Config/roxygen2/version to "8.1.0"
#> ℹ Loading file59e5cd657fe
#> Writing NAMESPACE
#> Writing file59e5cd657fe.Rd
#> Writing tbl.Rd
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#> * checking for file ‘/tmp/RtmpwtfBDE/file59e5cd657fe/DESCRIPTION’ ... OK
#> * preparing ‘file59e5cd657fe’:
#> * checking DESCRIPTION meta-information ... OK
#> * checking for LF line-endings in source and make files and shell scripts
#> * checking for empty or unneeded directories
#> * looking to see if a ‘data/datalist’ file should be added
#> * building ‘file59e5cd657fe_1.0.tar.gz’
#> 
#> Next Steps 
#> 1. Update your package documentation.
#>    - Edit the documentation.R file in the package sourcedata-rawsubdirectory and update the roxygen markup. 
#>    - Rebuild the package documentation with document(). 
#> 2. Add your package to source control.
#>    - Call git init . in the package source root directory. 
#>    - git add the package files. 
#>    - git commit your new package. 
#>    - Set up a github repository for your pacakge. 
#>    - Add the github repository as a remote of your local package repository. 
#>    - git push your local repository to gitub. 
#> [1] "/tmp/RtmpwtfBDE/file59e5cd657fe_1.0.tar.gz"
```
