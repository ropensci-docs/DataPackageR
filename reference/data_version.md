# Get the DataVersion for a package

Retrieves the DataVersion of a package if available

## Usage

``` r
data_version(pkg, lib.loc = NULL)
```

## Arguments

- pkg:

  `character` the package name

- lib.loc:

  `character` path to library location.

## Value

Object of class 'package_version' and 'numeric_version' specifying the
DataVersion of the package

## See also

[`packageVersion`](https://rdrr.io/r/utils/packageDescription.html)

## Examples

``` r
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f,"foo.Rmd")
con <- file(f)
writeLines("```{r}\n vec = 1:10 \n```\n",con=con)
close(con)
pname <- basename(tempfile())
datapackage_skeleton(name = pname,
   path=tempdir(),
   force = TRUE,
   r_object_names = "vec",
   code_files = f)

   package_build(file.path(tempdir(),pname), install = FALSE)

   pkgload::load_all(file.path(tempdir(),pname))
   data_version(pname)
}
#> ✔ Creating /tmp/RtmpwtfBDE/file59e7ec88b3c/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e7ec88b3c".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e7ec88b3c
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e1c9ce6eb".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e7ec88b3c".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied foo.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
#> 
#> ✔ 1 data set(s) created by foo.Rmd
#> • vec
#> ☘ Built all datasets!
#> Non-interactive NEWS.md file update.
#> * Added: vec
#> 
#> ✔ Creating vignettes/.
#> ✔ Creating inst/doc/.
#> ℹ Setting Config/roxygen2/version to "8.1.0"
#> ℹ Loading file59e7ec88b3c
#> Writing NAMESPACE
#> Writing file59e7ec88b3c.Rd
#> Writing vec.Rd
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#> * checking for file ‘/tmp/RtmpwtfBDE/file59e7ec88b3c/DESCRIPTION’ ... OK
#> * preparing ‘file59e7ec88b3c’:
#> * checking DESCRIPTION meta-information ... OK
#> * checking for LF line-endings in source and make files and shell scripts
#> * checking for empty or unneeded directories
#> * looking to see if a ‘data/datalist’ file should be added
#> * building ‘file59e7ec88b3c_1.0.tar.gz’
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
#> ℹ Loading file59e7ec88b3c
#> [1] ‘0.1.0’
```
