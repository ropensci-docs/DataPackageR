# Using DataPackageR

### Purpose

This vignette demonstrates how to use DataPackageR to build a data
package. DataPackageR aims to simplify data package construction. It
provides mechanisms for reproducibly preprocessing and tidying raw data
into into documented, versioned, and packaged analysis-ready data sets.
Long-running or computationally intensive data processing can be
decoupled from the usual `R CMD build` process while maintaining [data
lineage](https://en.wikipedia.org/wiki/Data_lineage).

For demonstration purposes, in this vignette we will subset and package
the `mtcars` data set.

### Set up a new data package.

We will set up a new data package based on the `mtcars` example in the
[README](https://github.com/ropensci/DataPackageR/blob/master/README.md).
The
[`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)
API is used to set up a new package. The user needs to provide:

- R or Rmd files that process data.
- A list of R object names created by those files.
- Optionally, a path to a directory of raw data (will be copied into the
  package).
- Optionally, a list of additional files that may be dependencies of
  your R or Rmd data processing files.

``` r
library(DataPackageR)

# Let's reproducibly package the cars in the mtcars dataset with speed
# > 20. Our dataset will be called `cars_over_20`.

# Get the code file that turns the raw data to our packaged and
# processed analysis-ready dataset.
processing_code <-
    system.file("extdata", "tests", "subsetCars.Rmd", package = "DataPackageR")

# Create the package framework.
DataPackageR::datapackage_skeleton(name = "mtcars20",
                                   force = TRUE,
                                   code_files = processing_code,
                                   r_object_names = "cars_over_20",
                                   path = tempdir()
                                   #dependencies argument is empty
                                   #raw_data_dir argument is empty.
                                   )
 [32m✔ [39m Creating  [34m/tmp/RtmpXDamri/mtcars20/ [39m.
 [32m✔ [39m Setting active project to  [34m"/tmp/RtmpXDamri/mtcars20" [39m.
 [32m✔ [39m Creating  [34mR/ [39m.
 [32m✔ [39m Writing  [34mDESCRIPTION [39m.
 [32m✔ [39m Writing  [34mNAMESPACE [39m.
 [32m✔ [39m Setting active project to  [34m"<no active project>" [39m.
 [32m✔ [39m Setting active project to  [34m"/tmp/RtmpXDamri/mtcars20" [39m.
 [32m✔ [39m Creating  [34mdata-raw/ [39m.
 [32m✔ [39m Creating  [34mdata/ [39m.
 [32m✔ [39m Creating  [34minst/extdata/ [39m.
```

#### What’s in the package skeleton structure?

The process above has created a DataPackageR source tree named
“mtcars20” in a temporary directory. For a real use case, you would pick
a path on your file system where you could then initialize a new GitHub
repository for the package.

The contents of `mtcars20` are:

                    levelName
    1  mtcars20              
    2   ¦--data              
    3   ¦--data-raw          
    4   ¦   °--subsetCars.Rmd
    5   ¦--datapackager.yml  
    6   ¦--DESCRIPTION       
    7   ¦--inst              
    8   ¦   °--extdata       
    9   ¦--R                 
    10  °--Read-and-delete-me

You should fill out the `DESCRIPTION` file to describe your data
package. It contains a new `DataVersion` string that will be
automatically incremented when the data package is built *if the
packaged data has changed*.

The user-provided code files reside in `data-raw`. They are executed
during the data package build process.

#### A note about the YAML config file.

A `datapackager.yml` file is used to configure and control the build
process.

The contents are:

    configuration:
      files:
        subsetCars.Rmd:
          enabled: yes
      objects: cars_over_20
      render_root:
        tmp: '80750'

The two main pieces of information in the configuration are a list of
the files to be processed and the data sets the package will store.

This example packages an R data set named `cars_over_20` (the name was
passed to
[`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)),
which is created by the `subsetCars.Rmd` file.

The objects must be listed in the yaml configuration file.
[`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)
ensures this is done for you automatically.

DataPackageR provides an API for modifying this file, so it does not
need to be done by hand.

Further information on the contents of the YAML configuration file, and
the API are in the [YAML Configuration
Details](https://docs.ropensci.org/DataPackageR/articles/YAML_Configuration_Details.md)
vignette.

#### Where do I put my raw datasets?

Raw data (provided the size is not prohibitive) can be placed in
`inst/extdata`.

The
[`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)
API has the `raw_data_dir` argument, which will copy the contents of
`raw_data_dir` (and its subdirectories) into `inst/extdata`
automatically.

In this example we are reading the `mtcars` data set that is already in
memory, rather than from the file system.

#### An API to read raw data sets from within an R or Rmd processing script.

As stated in the README, in order for your processing scripts to be
portable, you should not use absolute paths to files. DataPackageR
provides an API to point to the data package root directory and the
`inst/extdata` and `data` subdirectories. These are useful for
constructing portable paths in your code to read files from these
locations.

For example, to construct a path to a file named “mydata.csv” located in
`inst/extdata` in your data package source tree:

- use `DataPackageR::project_extdata_path("mydata.csv")` in your `R` or
  `Rmd` file. This would return: e.g.,
  /tmp/RtmpXDamri/mtcars20/inst/extdata/mydata.csv

Similarly:

- [`DataPackageR::project_path()`](https://docs.ropensci.org/DataPackageR/reference/project_path.md)
  constructs a path to the data package root directory. (e.g.,
  /tmp/RtmpXDamri/mtcars20)
- [`DataPackageR::project_data_path()`](https://docs.ropensci.org/DataPackageR/reference/project_data_path.md)
  constructs a path to the data package `data` subdirectory. (e.g.,
  /tmp/RtmpXDamri/mtcars20/data)

Raw data sets that are stored externally (outside the data package
source tree) can be constructed relative to the
[`project_path()`](https://docs.ropensci.org/DataPackageR/reference/project_path.md).

#### YAML header metadata for R files and Rmd files.

If your processing scripts are Rmd files, the usual yaml header for
rmarkdown documents should be present.

If your processing scripts are R files, you can still include a yaml
header, but it should be commented with `#'` and it should be at the top
of your R file. For example, a test R file in the DataPackageR package
looks as follows:

    #'---
    #\'title: Sample report from R script
    #'author: Greg Finak
    #'date: August 1, 2018
    #'---
    data <- runif(100)

This will be converted to an Rmd file with a proper yaml header, which
will then be turned into a vignette and indexed in the built package.

### Build the data package.

Once the skeleton framework is set up, run the preprocessing code to
build `cars_over_20`, and reproducibly enclose it in a package.

``` r
DataPackageR::package_build(file.path(tempdir(),"mtcars20"))

 [32m✔ [39m 1 data set(s) created by subsetCars.Rmd
 [31m• [39m cars_over_20
 [32m☘ [39m Built all datasets!
 [36mNon-interactive NEWS.md file update.
 [39m [36m* Added: cars_over_20
 [39m
 [32m✔ [39m Creating  [34mvignettes/ [39m.
 [32m✔ [39m Creating  [34minst/doc/ [39m.
 [1m [22m [36mℹ [39m Setting  [32mConfig/roxygen2/version [39m to  [34m"8.1.0" [39m
 [1m [22m [36mℹ [39m Loading  [34mmtcars20 [39m
 [1m [22mWriting  [34mNAMESPACE [39m
 [1m [22mWriting  [34mmtcars20.Rd [39m
 [1m [22mWriting  [34mcars_over_20.Rd [39m
 [36m── [39m  [36mR CMD build [39m  [36m───────────────────────────────────────────────────────────────── [39m
* checking for file ‘/tmp/RtmpXDamri/mtcars20/DESCRIPTION’ ... OK
* preparing ‘mtcars20’:
* checking DESCRIPTION meta-information ... OK
* checking for LF line-endings in source and make files and shell scripts
* checking for empty or unneeded directories
* looking to see if a ‘data/datalist’ file should be added
* building ‘mtcars20_1.0.tar.gz’

 [32m [1mNext Steps [22m [39m 
 [37m [33m [1m1. Update your package documentation. [22m [37m
 [39m [37m   - Edit the documentation.R file in the package source [32mdata-raw [37msubdirectory and update the roxygen markup. [39m 
 [37m   - Rebuild the package documentation with  [31mdocument() [37m. [39m 
 [37m [33m [1m2. Add your package to source control. [22m [37m
 [39m [37m   - Call  [31mgit init . [37m in the package source root directory. [39m 
 [37m   -  [31mgit add [37m the package files. [39m 
 [37m   -  [31mgit commit [37m your new package. [39m 
 [37m   - Set up a github repository for your pacakge. [39m 
 [37m   - Add the github repository as a remote of your local package repository. [39m 
 [37m   -  [31mgit push [37m your local repository to gitub. [39m 
[1] "/tmp/RtmpXDamri/mtcars20_1.0.tar.gz"
```

#### Documenting your data set changes in NEWS.

When you build a package in interactive mode, you will be prompted to
input text describing the changes to your data package (one line).

These will appear in the NEWS.md file in the following format:

    DataVersion: xx.yy.zz
    ========
    A description of your changes to the package

    [The rest of the file]

#### The build.

If everything goes smoothly, you will have a new package built in the
parent directory.

In this case we have a new package: `mtcars20_1.0.tar.gz`.

#### A note about the package source directory after building.

The package source directory changes after the first build.

                             levelName
    1  mtcars20                       
    2   ¦--data                       
    3   ¦   °--cars_over_20.rda       
    4   ¦--data-raw                   
    5   ¦   ¦--documentation.R        
    6   ¦   °--subsetCars.Rmd         
    7   ¦--DATADIGEST                 
    8   ¦--datapackager.yml           
    9   ¦--DESCRIPTION                
    10  ¦--inst                       
    11  ¦   ¦--doc                    
    12  ¦   ¦   ¦--subsetCars.html    
    13  ¦   ¦   °--subsetCars.Rmd     
    14  ¦   °--extdata                
    15  ¦       °--Logfiles           
    16  ¦           °--subsetCars.html
    17  ¦--man                        
    18  ¦   ¦--cars_over_20.Rd        
    19  ¦   °--mtcars20.Rd            
    20  ¦--NAMESPACE                  
    21  ¦--NEWS.md                    
    22  ¦--R                          
    23  ¦   °--mtcars20.R             
    24  ¦--Read-and-delete-me         
    25  °--vignettes                  
    26      °--subsetCars.Rmd         

#### Update the auto-generated documentation.

After the first build, the `R` directory contains `mtcars.R` that has
auto-generated `roxygen2` markup documentation for the data package and
for the `cars_over20` packaged data.

The processed `Rd` files can be found in `man`.

The auto-generated documentation source is in the `documentation.R` file
in `data-raw`.

You should update this file to properly document your objects. Then
rebuild the documentation:

``` r
DataPackageR::document(file.path(tempdir(),"mtcars20"))
 [1m [22m [36mℹ [39m Loading  [34mmtcars20 [39m
[1] TRUE
```

Updating documentation does not reprocess the data.

Once the the documentation is updated in `R/mtcars.R`, then run
[`package_build()`](https://docs.ropensci.org/DataPackageR/reference/package_build.md)
again.

#### Why not just use R CMD build?

If the processing script is time consuming or the data set is
particularly large, then `R CMD build` would run the code each time the
package is installed. In such cases, raw data may not be available, or
the environment to do the data processing may not be set up for each
user of the data. DataPackageR decouples data processing from package
building/installation for data consumers.

### Installing and using the new data package.

#### Accessing vignettes, data sets, and data set documentation.

The package source also contains files in the `vignettes` and `inst/doc`
directories that provide a log of the data processing.

When the package is installed, these will be accessible via the
[`vignette()`](https://rdrr.io/r/utils/vignette.html) API.

The vignette will detail the processing performed by the
`subsetCars.Rmd` processing script.

The data set documentation will be accessible via `?cars_over_20`, and
the data sets via [`data()`](https://rdrr.io/r/utils/data.html).

``` r
# Create a temporary library to install into.
dir.create(file.path(tempdir(),"lib"))

# Let's install the package we just created.
# This can also be done with with `install = TRUE` in package_build() or document().

install.packages(file.path(tempdir(),"mtcars20_1.0.tar.gz"),
                 type = "source", repos = NULL,
                 lib = file.path(tempdir(),"lib"))
lns <- loadNamespace
if (!"package:mtcars20"%in%search())
  attachNamespace(lns('mtcars20',lib.loc = file.path(tempdir(),"lib"))) #use library() in your code
data("cars_over_20") # load the data

cars_over_20 # now we can use it.
   speed dist
44    22   66
45    23   54
46    24   70
47    24   92
48    24   93
49    24  120
50    25   85
?cars_over_20 # See the documentation you wrote in data-raw/documentation.R.

vignettes <- vignette(package = "mtcars20", lib.loc = file.path(tempdir(),"lib"))
vignettes$results
      Package    LibPath               Item        
Topic "mtcars20" "/tmp/RtmpXDamri/lib" "subsetCars"
      Title                                            
Topic "A Test Document for DataPackageR (source, html)"
```

#### Using the DataVersion.

Your downstream data analysis can depend on a specific version of the
data in your data package by testing the DataVersion string in the
DESCRIPTION file.

We provide an API for this:

``` r
# We can easily check the version of the data.
DataPackageR::data_version("mtcars20", lib.loc = file.path(tempdir(),"lib"))
[1] '0.1.0'

# You can use an assert to check the data version in  reports and
# analyses that use the packaged data.
assert_data_version(data_package_name = "mtcars20",
                    version_string = "0.1.0", acceptable = "equal",
                    lib.loc = file.path(tempdir(),"lib"))  #If this fails, execution stops
                                           #and provides an informative error.
```

## Migrating old data packages.

Version 1.12.0 has moved away from controlling the build process using
`datasets.R` and an additional `masterfile` argument.

The build process is now controlled via a `datapackager.yml`
configuration file located in the package root directory. See [YAML
Configuration
Details](https://docs.ropensci.org/DataPackageR/articles/YAML_Configuration_Details.md).

### Create a datapackager.yml file.

You can migrate an old package by constructing such a config file using
the
[`construct_yml_config()`](https://docs.ropensci.org/DataPackageR/reference/construct_yml_config.md)
API.

``` r

# Assume I have file1.Rmd and file2.R located in /data-raw, and these
# create 'object1' and 'object2' respectively.

config <- construct_yml_config(code = c("file1.Rmd", "file2.R"),
                               data = c("object1", "object2"))
cat(yaml::as.yaml(config))
configuration:
  files:
    file1.Rmd:
      enabled: yes
    file2.R:
      enabled: yes
  objects:
  - object1
  - object2
  render_root:
    tmp: '412256'
```

`config` is a newly constructed yaml configuration object. It can be
written to the package directory:

``` r

path_to_package <- tempdir() # e.g., if tempdir() was the root of our package.
yml_write(config, path = path_to_package)
```

Now the package at `path_to_package` will build with version 1.12.0 or
greater.

#### Reading data sets from Rmd files.

In versions prior to 1.12.1 we would read data sets from `inst/extdata`
in an `Rmd` script using paths relative to `data-raw` in the data
package source tree.

For example:

##### The old way.

``` r

# read 'myfile.csv' from inst/extdata relative to data-raw where the Rmd is rendered.
read.csv(file.path("../inst/extdata","myfile.csv"))
```

Now `Rmd` and `R` scripts are processed in `render_root` defined in the
yaml config.

To read a raw data set we can get the path to the package source
directory using an API call:

##### The new way.

``` r

# DataPackageR::project_extdata_path() returns the path to the data package inst/extdata subdirectory directory.
# DataPackageR::project_path() returns the path to the data package root directory.
# DataPackageR::project_data_path() returns the path to the data package data subdirectory directory.
read.csv(DataPackageR::project_extdata_path("myfile.csv"))
```

## Partial builds.

We can also perform partial builds of a subset of files in a package by
toggling the `enabled` key in the yaml config file.

This can be done with the following API:

``` r

config <- yml_disable_compile(config,filenames = "file2.R")
yml_write(config, path = path_to_package) # write modified yml to the package.
configuration:
  files:
    file1.Rmd:
      enabled: yes
    file2.R:
      enabled: no
  objects:
  - object1
  - object2
  render_root:
    tmp: '412256'
```

Note that the modified configuration needs to be written back to the
package source directory in order for the changes to take effect.

The consequence of toggling a file to `enable: no` is that it will be
skipped when the package is rebuilt, but the data will still be retained
in the package, and the documentation will not be altered.

This is useful in situations where we have multiple data sets, and we
want to re-run one script to update a specific data set, but not the
other scripts because they may be too time consuming.

## Multi-script pipelines.

We may have situations where we have multi-script pipelines. There are
two ways to share data among scripts.

1.  file system artifacts
2.  data objects passed to subsequent scripts

### File system artifacts.

The yaml configuration property `render_root` specifies the working
directory where scripts will be rendered.

If a script writes files to the working directory, that is where files
will appear. These can be read by subsequent scripts.

### Passing data objects to subsequent scripts.

A script can access a data object designated to be packaged by
previously ran scripts using
[`datapackager_object_read()`](https://docs.ropensci.org/DataPackageR/reference/datapackager_object_read.md).

For example, `script2.Rmd` will run after `script1.Rmd`. `script2.Rmd`
needs to access a data object that has been designated to be packaged
named `dataset1`, which was created by `script1.Rmd`. This data set can
be accessed by `script2.Rmd` using the following expression:

`dataset1 <- DataPackageR::datapackager_object_read("dataset1")`.

Passing of data objects among scripts can be turned off via:

`package_build(deps = FALSE)`

## Next steps.

We recommend the following once your package is created.

### Place your package under source control.

You now have a data package source tree.

- **Place your package under version control**
  1.  Call `git init` in the package source root to initialize a new git
      repository.
  2.  [Create a new repository for your data package on
      GitHub](https://docs.github.com/articles/create-a-repo/).
  3.  Push your local package repository to `GitHub`. [see step
      7](https://docs.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)

This will let you version control your data processing code, and will
provide a mechanism for sharing your package with others.

For more details on using git and GitHub with R, there is an excellent
guide provided by Jenny Bryan: [Happy git and GitHub for the
useR](https://happygitwithr.com/) and Hadley Wickham’s [book on R
packages](https://r-pkgs.org/).

## Additional Details.

### Fingerprints of stored data objects.

DataPackageR calculates an md5 checksum of each data object it stores,
and keeps track of them in a file called `DATADIGEST`.

- Each time the package is rebuilt, the md5 sums of the new data objects
  are compared against `DATADIGEST`.
- If they do not match, the build process checks that the `DataVersion`
  string has been incremented in the `DESCRIPTION` file.
- If it has not, the build process will exit and produce an error
  message.

#### DATADIGEST

The `DATADIGEST` file contains the following:

    DataVersion: 0.1.0
    cars_over_20: 3ccb5b0aaa74fe7cfc0d3ca6ab0b5cf3

#### DESCRIPTION

The description file has the new `DataVersion` string.

    Package: mtcars20
    Title: What the Package Does (One Line, Title Case)
    Version: 1.0
    Authors@R: 
        person("First", "Last", , "first.last@example.com", role = c("aut", "cre"))
    Description: What the package does (one paragraph).
    License: `use_mit_license()`, `use_gpl3_license()` or friends to pick a
        license
    Encoding: UTF-8
    Roxygen: list(markdown = TRUE)
    DataVersion: 0.1.0
    Depends: 
        R (>= 3.5.0)
    Date: 2026-08-30
    Suggests: 
        knitr,
        rmarkdown
    VignetteBuilder: knitr
    Config/roxygen2/version: 8.1.0
