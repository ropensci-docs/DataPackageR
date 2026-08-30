# Ignore specific files by git and R build.

Ignore specific files by git and R build.

## Usage

``` r
use_ignore(file = NULL, path = NULL)
```

## Arguments

- file:

  `character` File to ignore.

- path:

  `character` Path to the file.

## Value

invisibly returns 0.

## Examples

``` r
datapackage_skeleton(name="test",path = tempdir())
#> ✔ Creating /tmp/RtmpwtfBDE/test/.
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/test".
#> ✔ Creating R/.
#> ✔ Writing DESCRIPTION.
#> Package: test
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
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/datatest".
#> ✔ Setting active project to "/tmp/RtmpwtfBDE/test".
#> ✔ Added DataVersion string to 'DESCRIPTION'
#> ✔ Creating data-raw/.
#> ✔ Creating data/.
#> ✔ Creating inst/extdata/.
#> ✔ configured 'datapackager.yml' file
use_ignore("foo", ".")
#> ✔ Adding "^\\./foo$" to .Rbuildignore.
#> ✔ Adding "foo" to .gitignore.
```
