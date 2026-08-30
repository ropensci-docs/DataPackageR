# Build documentation for a data package using DataPackageR.

Build documentation for a data package using DataPackageR.

## Usage

``` r
document(path = ".", install = FALSE, ...)
```

## Arguments

- path:

  `character` the path to the data package source root.

- install:

  `logical` install the package. (default FALSE)

- ...:

  additional arguments to `install`

## Value

Called for side effects. Returns TRUE on successful exit.

## Examples

``` r
# A simple Rmd file that creates one data object
# named "tbl".
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f,"foo.Rmd")
con <- file(f)
writeLines("```{r}\n tbl = data.frame(1:10) \n```\n",con=con)
close(con)
# \donttest{
# construct a data package skeleton named "MyDataPackage" and pass
# in the Rmd file name with full path, and the name of the object(s) it
# creates.

pname <- basename(tempfile())
datapackage_skeleton(name=pname,
   path=tempdir(),
   force = TRUE,
   r_object_names = "tbl",
   code_files = f)

# call package_build to run the "foo.Rmd" processing and
# build a data package.
package_build(file.path(tempdir(), pname), install = FALSE)
document(path = file.path(tempdir(), pname), install = FALSE)
# }
}
#> ✔ Creating /tmp/RtmpwtfBDE/file59e4147b5b1/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e4147b5b1".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e4147b5b1
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e44bbc742".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e4147b5b1".
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
#> ℹ Loading file59e4147b5b1
#> Writing NAMESPACE
#> Writing file59e4147b5b1.Rd
#> Writing tbl.Rd
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#> * checking for file ‘/tmp/RtmpwtfBDE/file59e4147b5b1/DESCRIPTION’ ... OK
#> * preparing ‘file59e4147b5b1’:
#> * checking DESCRIPTION meta-information ... OK
#> * checking for LF line-endings in source and make files and shell scripts
#> * checking for empty or unneeded directories
#> * looking to see if a ‘data/datalist’ file should be added
#> * building ‘file59e4147b5b1_1.0.tar.gz’
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
#> 
#> ℹ Loading file59e4147b5b1
#> [1] TRUE
```
