# Edit DataPackageR yaml configuration

Edit a yaml configuration file via an API.

## Usage

``` r
yml_find(path)

yml_add_files(config, filenames)

yml_disable_compile(config, filenames)

yml_enable_compile(config, filenames)

yml_add_objects(config, objects)

yml_list_objects(config)

yml_list_files(config)

yml_remove_objects(config, objects)

yml_remove_files(config, filenames)

yml_write(config, path = NULL)
```

## Arguments

- path:

  Path to the data package source or path to write config file (for
  `yml_write`)

- config:

  an R representation of the datapackager.yml config, returned by
  yml_find, or a path to the package root.

- filenames:

  A vector of filenames.

- objects:

  A vector of R object names.

## Value

A yaml configuration structured as an R nested list.

## Details

Add, remove files and objects, enable or disable parsing of specific
files, list objects or files in a yaml config, or write a config back to
a package.

## Examples

``` r
if(rmarkdown::pandoc_available()){
f <- tempdir()
f <- file.path(f,"foo.Rmd")
con <- file(f)
writeLines("```{r}\n vec = 1:10\n```\n",con=con)
close(con)
pname <- basename(tempfile())
datapackage_skeleton(name=pname,
   path = tempdir(),
   force = TRUE,
   r_object_names = "vec",
   code_files = f)
yml <- yml_find(file.path(tempdir(),pname))
yml <- yml_add_files(yml,"foo.Rmd")
yml_list_files(yml)
yml <- yml_disable_compile(yml,"foo.Rmd")
yml <- yml_enable_compile(yml,"foo.Rmd")
yml <- yml_add_objects(yml,"data1")
yml_list_objects(yml)
yml <- yml_remove_objects(yml,"data1")
yml <- yml_remove_files(yml,"foo.Rmd")
}
#> ✔ Creating /tmp/RtmpCXunvE/file63f6bacd8d3/.
#> ✔ Setting active project to "/tmp/RtmpCXunvE/file63f6bacd8d3".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: file63f6bacd8d3
#> Title: What the Package Does (One Line, Title Case)
#> Version: 0.0.0.9000
#> Authors@R (parsed):
#>     * First Last <first.last@example.com> [aut, cre]
#> Description: What the package does (one paragraph).
#> License: `use_mit_license()`, `use_gpl3_license()` or friends to
#>     pick a license
#> Config/roxygen2/version: 8.1.1
#> Encoding: UTF-8
#> Roxygen: list(markdown = TRUE)
#> ✔ Writing NAMESPACE.
#> ✔ Setting active project to "/tmp/RtmpCXunvE/datatest".
#> ✔ Setting active project to "/tmp/RtmpCXunvE/file63f6bacd8d3".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ Copied foo.Rmd into 'data-raw'
#> ✔ configured 'datapackager.yml' file
#> configuration:
#>   files:
#>     foo.Rmd:
#>       enabled: yes
#>   objects: vec
#>   render_root:
#>     tmp: '591534'
#> 
#> foo.Rmdconfiguration:
#>   files:
#>     foo.Rmd:
#>       enabled: yes
#>   objects:
#>   - vec
#>   - data1
#>   render_root:
#>     tmp: '591534'
#> 
#> vec data1configuration:
#>   files:
#>     foo.Rmd:
#>       enabled: yes
#>   objects: vec
#>   render_root:
#>     tmp: '591534'
#> configuration:
#>   files: {}
#>   objects: vec
#>   render_root:
#>     tmp: '591534'
```
