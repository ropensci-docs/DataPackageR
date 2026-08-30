# Options consulted by DataPackageR

User-configurable options consulted by DataPackageR, which provide a
mechanism for setting default behaviors for various functions.

If the built-in defaults don't suit you, set one or more of these
options. Typically, this is done in the `.Rprofile` startup file, which
you can open for editing with
[`usethis::edit_r_profile()`](https://usethis.r-lib.org/reference/edit.html) -
this will set the specified options for all future R sessions. The
following setting is recommended to not be prompted upon each package
build for a NEWS update:

`options(DataPackageR_interact = FALSE)`

## Options for the DataPackageR package

\- `DataPackageR_interact`: Upon package load, this defaults to the
value of [`interactive()`](https://rdrr.io/r/base/interactive.html),
unless the option has been previously set (e.g., in `.Rprofile`). TRUE
prompts user interactively for a NEWS update on
[`package_build()`](https://docs.ropensci.org/DataPackageR/reference/package_build.md).
See the example above and the [rOpenSci blog
post](https://ropensci.org/blog/2018/09/18/datapackager/) for more
details on how to set this to FALSE, which will never prompt user for a
NEWS update. FALSE is also the setting used for DataPackageR internal
package tests.

\- `DataPackageR_verbose`: Default upon package load is TRUE. FALSE
suppresses all console output and is currently only used for automated
unit tests of the DataPackageR package.

\- `DataPackageR_packagebuilding`: Default upon package load is FALSE.
This option is used internally for package operations and changing it is
not recommended.
