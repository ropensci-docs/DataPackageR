# Changelog

## DataPackageR 0.16.2

CRAN release: 2025-12-08

### Significant user-facing changes

- Remove functionality for logging to a logfile
  ([\#163](https://github.com/ropensci/DataPackageR/issues/163))
- Remove ‘log’ argument from package_build()
  ([\#163](https://github.com/ropensci/DataPackageR/issues/163))

### Unchanged

- User still sees same messages, warnings, and errors on the console
- Changes to data objects still automatically added to the data package
  NEWS.md
- For now, rendered output files still written to inst/extdata/Logfiles

### Maintenance

- Drop dependency on futile.logger package, which has not been updated
  since 2016.
- Revert usethis bug workaround from
  [\#162](https://github.com/ropensci/DataPackageR/issues/162), now
  fixed upstream
  ([\#165](https://github.com/ropensci/DataPackageR/issues/165))

## DataPackageR 0.16.1

CRAN release: 2024-09-17

### Minor user-facing improvements

- Suppress warning from project\*\_path() functions when file does not
  exist ([\#160](https://github.com/ropensci/DataPackageR/issues/160))

### Maintenance

- Remove dependency on crayon package, switch to cli package
  ([\#159](https://github.com/ropensci/DataPackageR/issues/159))
- Remove invalid ORCID placeholder created by usethis that caused
  failing CRAN checks
  ([\#162](https://github.com/ropensci/DataPackageR/issues/162))

## DataPackageR 0.16.0

CRAN release: 2024-06-03

### Bug fixes

- Throw error if data object name is same as package name. Fixes
  ([\#62](https://github.com/ropensci/DataPackageR/issues/62))
- Fix triplicate warnings when data version changes
  ([\#128](https://github.com/ropensci/DataPackageR/issues/128))
- Fix bug where automatic updates to NEWS.md and the log file did not
  reflect the actual changes to data objects
  ([\#67](https://github.com/ropensci/DataPackageR/issues/67))
  - The actual data packaging and versioning behavior was as intended,
    but now the NEWS.md and log file reflect that behavior
- Remove inconsistently auto-generated ‘see also’ links from package and
  data object help files

### Significant user-facing changes

- [`document()`](https://docs.ropensci.org/DataPackageR/reference/document.md)
  now defaults to `install = FALSE`, like
  [`package_build()`](https://docs.ropensci.org/DataPackageR/reference/package_build.md)
  ([\#130](https://github.com/ropensci/DataPackageR/issues/130))

### Minor user-facing improvements

- Tweaks to PDF manual and vignette index titles
- Tweaks to spacing in console outputs and log file
- Suppress repetitive log file lines about updating NEWS.md
- Remove `assertthat`, `devtools`, `stringr`, and `purrr` from Imports
- Add `pkgbuild` and `pkgload` to Imports
- Move `withr` from Imports to Suggests
- Make some long-deprecated functions officially Defunct
  - [`dataVersion()`](https://docs.ropensci.org/DataPackageR/reference/DataPackageR-defunct.md),
    renamed years ago to
    [`data_version()`](https://docs.ropensci.org/DataPackageR/reference/data_version.md)
  - [`datapackage.skeleton()`](https://docs.ropensci.org/DataPackageR/reference/DataPackageR-defunct.md),
    renamed years ago to
    [`datapackage_skeleton()`](https://docs.ropensci.org/DataPackageR/reference/datapackage_skeleton.md)
  - [`keepDataObjects()`](https://docs.ropensci.org/DataPackageR/reference/DataPackageR-defunct.md)
- Keep existing DataPackageR_verbose option if set outside of package

### Internal improvements

- Refactor and split up internal omnibus function
  [`DataPackageR()`](https://docs.ropensci.org/DataPackageR/reference/DataPackageR-package.md)
  - New internal functions
    - `do_doc()`
    - `do_digests()`
    - `validate_pkg_name()`
    - `validate_package_skeleton()`
    - `validate_DataVersion()`
    - `validate_description()`
    - `validate_yml()`
    - `get_yml_objects()`
    - `get_yml_r_files()`
  - Deleted internal functions
    - `read.description()`, now uses
      [`desc::desc()`](https://desc.r-lib.org/reference/desc.html)
    - `read_pkg_description()`, now uses
      [`desc::desc()`](https://desc.r-lib.org/reference/desc.html)
    - [`comment()`](https://rdrr.io/r/base/comment.html), now uses
      [`roxygen2::parse_file()`](https://roxygen2.r-lib.org/reference/parse_package.html)
  - Simplify internal functions
    - `.increment_data_version()`
    - `.digest_data_env()`
    - `.save_data()`
    - `.doc_parse()`
    - `.check_dataversion_string()`
  - Use base R
    [`compareVersion()`](https://rdrr.io/r/utils/compareVersion.html)
    and class `package_version` to simplify code involving the
    DataVersion and do additional sanity checks
- Miscellaneous cleanup and maintenance
- Make unit tests more independent from each other

## DataPackageR 0.15.9

CRAN release: 2024-04-17

### Bug fixes

- Fix bug where `.Rprofile` options setting for `DataPackageR_interact`
  was overwritten upon package load
- Fix bugs where document() and package_build() left the data package
  attached
- Fix unit test inter-dependencies
- Make package template depend on R \>= 3.5.0, suppressing `.rda`
  serialization warnings upon install
- Close a file connection, suppressing warnings about orphaned
  connections
- Fix broken test that led to archiving on CRAN
- Fix package documentation method for data packages and DataPackageR
  itself (r-lib/roxygen2#1491)

### Minor improvements

- Various maintenance tweaks
- Documentation improvements
- Update maintainer and contact info
- New global option `DataPackageR_verbose` to suppress console output,
  e.g. during unit testing

## DataPackageR 0.15.8

CRAN release: 2021-03-17

- Fix to datapackager_object_read that was causing a test to break.
  `get` needs to have `inherits=FALSE`.
- Other fixes for `usethis` 1.6.0
- Fixes to tests that were failing on CRAN
- In `package_build`, remove `devtools::reload` and put
  `devtools::unload` and in front of install.packages
- in `document`, remove `devtools::reload` and put in `devtools::unload`
  and install.packages

## DataPackageR 0.15.7

CRAN release: 2019-03-30

- Fix test and vignette bugs related to upcoming version of usethis
  (1.5)

## DataPackageR 0.15.6

CRAN release: 2019-03-13

- Fix bug in vignette and code that writes to user space during CRAN
  checks.

## DataPackageR 0.15.4.900

- Fix a bug in update_news.
- Create news files if it doesn’t exist.

## DataPackageR 0.15.4

CRAN release: 2018-10-24

- New CRAN Release

## DataPackageR 0.15.3.9000

### Features and enhancements

- Reduce the console output from logging.
  (ropensci/DataPackageR/issues/50)
- Create a new logger that logs at different thresholds to console and
  to file (ropensci/DataPackageR/issues/50)
- Default on build is not to install.
- Hide console output from Rmd render.
- Nicer messages describing data sets that are created
  (ropensci/DataPackageR/issues/51)
- Write deleted, changed, and added data objects to the NEWS file
  automatically.
- Add option to overwrite (or not) via use_processing_script. Provide
  warning.
- Add use_ignore() to ignore files and data sets in .Rbuildignore and
  .gitignore and added ignore argument to use_raw_dataset().

### Bug fixes

- code argument no longer required for construct_yml_config
- Fix the documentation for datapackager_object_read() and “Migrating
  old packages”.
- Copy over vignettes generated as pdfs into the package inst/doc
- Data objects are incrementally stored during the build process, into
  the render_root directory specified in the datapackager.yml config
  file.

## DataPackageR 0.15.3

CRAN release: 2018-08-23

- conditional tests when pandoc is missing
  (ropensci/DataPackageR/issues/46)
- add use_data_object and use_processing_script
  (ropensci/DataPackageR/issues/44)
- allow datapackage_skeleton to be called without files or data objects
  for interactive construction. (ropensci/DataPackageR/issues/44)

## DataPackageR 0.15.2

CRAN release: 2018-08-17

- Add pandoc to SystemRequirements (ropensci/DataPackageR/issues/46)
- Add use_raw_dataset() method (and tests) to add data sets to
  inst/extdata. interactively. (ropensci/DataPackageR/issues/44)

## DataPackageR 0.15.1.9000

- Development version

## DataPackageR 0.15.1

CRAN release: 2018-08-16

- Fix CRAN notes.

## DataPackageR 0.15.0

- Prepare for CRAN submission.

## DataPackageR 0.14.9

- Moving towards rOpenSci compliance
- NEWS.md updated with description of changes to data sets when version
  is bumped (or new package is created).
- Output of “next steps” for user when package is built
- New
  [`document()`](https://docs.ropensci.org/DataPackageR/reference/document.md)
  function to rebuild docs from `documentation.R` in `data-raw` without
  rebuilding the whole package.
- Improved package test.
- R scripts processed properly into vignettes.
- Packages installed and loaded after build to make vignettes and data
  sets accessible in same R session.

## DataPackageR 0.13.6

- Added a NEWS file.
- Cleaned up the examples.
- Snake case for all exported functions.

## DataPackageR 0.13.3

- Added the `render_root` property to the YAML configuration. Specifies
  where `render()` processing is done, instead of the `data-raw`
  directory.
