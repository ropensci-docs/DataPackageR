# DataPackageR

A framework to automate the processing, tidying and packaging of raw
data into analysis-ready data sets as R packages.

## Details

DataPackageR will automate running of data processing code, storing
tidied data sets in an R package, producing data documentation stubs,
tracking data object finger prints (md5 hash) and tracking and
incrementing a "DataVersion" string in the DESCRIPTION file of the
package when raw data or data objects change. Code to perform the data
processing is passed to DataPackageR by the user. The user also
specifies the names of the tidy data objects to be stored, documented
and tracked in the final package. Raw data should be read from
"inst/extdata" but large raw data files can be read from sources
external to the package source tree.

Configuration is controlled via the datapackager.yml file created at the
package root. Its properties include a list of R and Rmd files that are
to be rendered / sourced and which read data and do the actual
processing. It also includes a list of r object names created by those
files. These objects are stored in the final package and accessible via
the [`data()`](https://rdrr.io/r/utils/data.html) API. The documentation
for these objects is accessible via "?object-name", and md5 fingerprints
of these objects are created and tracked.

The Rmd and R files used to process the objects are transformed into
vignettes accessible in the final package so that the processing is
fully documented.

A DATADIGEST file in the package source keeps track of the data object
fingerprints. A DataVersion string is added to the package DESCRIPTION
file and updated when these objects are updated or changed on subsequent
builds.

Once the package is built and installed, the data objects created in the
package are accessible via the
[`data()`](https://rdrr.io/r/utils/data.html) API, and Calling
[`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)
and passing in R / Rmd file names, and r object names constructs a
skeleton data package source tree and an associated `datapackager.yml`
file.

Calling
[`package_build()`](https://docs.ropensci.org/DataPackageR/reference/package_build.md)
sets the build process in motion.

## See also

Useful links:

- <https://github.com/ropensci/DataPackageR>

- <https://docs.ropensci.org/DataPackageR/>

- Report bugs at <https://github.com/ropensci/DataPackageR/issues>

## Author

**Maintainer**: Dave Slager <dslager@fredhutch.org>
([ORCID](https://orcid.org/0000-0003-2525-2039)) \[contributor\]

Authors:

- Greg Finak <greg.finak@gmail.com> (Original author and creator of
  DataPackageR) \[copyright holder\]

Other contributors:

- Paul Obrecht \[contributor\]

- Ellis Hughes <ellishughes@live.com>
  ([ORCID](https://orcid.org/0000-0003-0637-4436)) \[contributor\]

- Jimmy Fulp <williamjfulp@gmail.com> \[contributor\]

- Marie Vendettuoli ([ORCID](https://orcid.org/0000-0001-9321-1410))
  \[contributor\]

- Jason Taylor <jmtaylor@fredhutch.org> \[contributor\]

- Kara Woo (Kara reviewed the package for rOpenSci, see
  \<https://github.com/ropensci/onboarding/issues/230\>) \[reviewer\]

- William Landau (William reviewed the package for rOpenSci, see
  \<https://github.com/ropensci/onboarding/issues/230\>) \[reviewer\]

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

# "install" the data package
pkgload::load_all(file.path(tempdir(), pname))

# read the data version
data_version(pname)

# list the data sets in the package.
data(package = pname)

# The data objects are in the package source under "/data"
list.files(pattern="rda", path = file.path(tempdir(),pname,"data"), full = TRUE)

# The documentation that needs to be edited is in "/R"
list.files(pattern="R", path = file.path(tempdir(), pname,"R"), full = TRUE)
readLines(list.files(pattern="R", path = file.path(tempdir(),pname,"R"), full = TRUE))
# view the documentation with
?tbl
}
#> ✔ Creating /tmp/RtmpwtfBDE/file59e479645b7/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e479645b7".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file59e479645b7
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
#> ✔ Setting active project to "<no active project>".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/file59e479645b7".
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
#> ℹ Loading file59e479645b7
#> Writing NAMESPACE
#> Writing file59e479645b7.Rd
#> Writing tbl.Rd
#> ── R CMD build ─────────────────────────────────────────────────────────────────
#> * checking for file ‘/tmp/RtmpwtfBDE/file59e479645b7/DESCRIPTION’ ... OK
#> * preparing ‘file59e479645b7’:
#> * checking DESCRIPTION meta-information ... OK
#> * checking for LF line-endings in source and make files and shell scripts
#> * checking for empty or unneeded directories
#> * looking to see if a ‘data/datalist’ file should be added
#> * building ‘file59e479645b7_1.0.tar.gz’
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
#> ℹ Loading file59e479645b7
#> ℹ Rendering development documentation for "tbl"
```
