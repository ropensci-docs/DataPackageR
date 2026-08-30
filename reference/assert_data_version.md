# Assert that a data version in a data package matches an expectation.

Assert that a data version in a data package matches an expectation.

## Usage

``` r
assert_data_version(
  data_package_name = NULL,
  version_string = NULL,
  acceptable = "equal",
  ...
)
```

## Arguments

- data_package_name:

  `character` Name of the package.

- version_string:

  `character` Version string in "x.y.z" format.

- acceptable:

  `character` one of "equal", "equal_or_greater", describing what
  version match is acceptable.

- ...:

  additional arguments passed to data_version (such as lib.loc)

## Value

invisible `logical` TRUE if success, otherwise stop on mismatch.

## Details

Tests the DataVersion string in `data_package_name` against
`version_string` testing the major, minor and revision portion.

Tests "data_package_name version equal version_string" or
"data_package_name version equal_or_greater version_string".

## Examples

``` r
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f, "foo.Rmd")
con <- file(f)
writeLines("```{r}\n vec = 1:10 \n```\n",con = con)
close(con)
pname <- basename(tempfile())
datapackage_skeleton(name = pname,
   path=tempdir(),
   force = TRUE,
   r_object_names = "vec",
   code_files = f)
package_build(file.path(tempdir(),pname), install = FALSE)

pkgload::load_all(file.path(tempdir(),pname))

assert_data_version(data_package_name = pname,version_string = "0.1.0",acceptable = "equal")
}
#> ✔ Creating /tmp/RtmpwtfBDE/file59e1c9ce6eb/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e1c9ce6eb".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e1c9ce6eb
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e479645b7".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e1c9ce6eb".
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
#> ℹ Loading file59e1c9ce6eb
#> Writing NAMESPACE
#> Writing file59e1c9ce6eb.Rd
#> Writing vec.Rd
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#> * checking for file ‘/tmp/RtmpwtfBDE/file59e1c9ce6eb/DESCRIPTION’ ... OK
#> * preparing ‘file59e1c9ce6eb’:
#> * checking DESCRIPTION meta-information ... OK
#> * checking for LF line-endings in source and make files and shell scripts
#> * checking for empty or unneeded directories
#> * looking to see if a ‘data/datalist’ file should be added
#> * building ‘file59e1c9ce6eb_1.0.tar.gz’
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
#> ℹ Loading file59e1c9ce6eb
```
