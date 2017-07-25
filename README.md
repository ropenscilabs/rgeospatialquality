rgeospatialquality
==================

[![Travis-CI Build Status](https://travis-ci.org/ropenscilabs/rgeospatialquality.svg?branch=master)](https://travis-ci.org/ropenscilabs/rgeospatialquality) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/c1h4vnaabh6ml19e/branch/master?svg=true)](https://ci.appveyor.com/project/jotegui/rgeospatialquality-ctpju/branch/master) [![codecov.io](https://codecov.io/github/ropenscilabs/rgeospatialquality/coverage.svg?branch=master)](https://codecov.io/github/ropenscilabs/rgeospatialquality?branch=master) [![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/rgeospatialquality)](http://cran.r-project.org/package=rgeospatialquality)
[![](https://badges.ropensci.org/27_status.svg)](https://github.com/ropensci/onboarding/issues/27)

`rgeospatialquality` provides R native access to the methods of the [Geospatial Data Quality REST API](https://github.com/VertNet/api-geospatial/blob/master/GeospatialQuality.md). With this API, users can perform some basic quality checks on the geospatial aspect of biodiversity data. See the link above for more information on the API rationale, development, methods and usage.

Latest version is *0.3.3*

Installation
------------

The release version of the package is available from CRAN. You can install it with `install.packages("rgeospatialquality")`.

Users can also install any version (including development versions) via the `install_github` command, from `devtools` package. **Important note**: Windows users will need to install [Rtools](http://cran.r-project.org/bin/windows/Rtools/) first.

``` r
install.packages("devtools")
devtools::install_github("ropenscilabs/rgeospatialquality")
```

`rgeospatialquality` depends on three packages:

1.  `httr` to perform the `GET` and `POST` requests to the API URL
2.  `jsonlite` to transform input data to JSON and to parse JSON responses to R native formats (`list` and `data.frame`)
3.  `plyr` to make some dataset transformation operations

Also, it suggests the installation of ROpenSci's `rgbif` (<https://github.com/ropensci/rgbif>), for executing examples.

``` r
library(rgeospatialquality)
```

Get quality flags for single records
------------------------------------

There are two ways to assess single records and get information on their spatial quality: by providing a list-type object with named elements or by passing the required data via function arguments. In any case, flags are calculated with the function `parse_record`, and the result is a named list with the quality information.

The API works on four specific fields, which should be present to provide the most complete answer: `decimalLatitude`, `decimalLongitude`, `countryCode` and `scientificName`. None of them is mandatory, but the more complete the provided information, the better the result set will be. [See the API documentation](https://github.com/VertNet/api-geospatial/blob/master/GeospatialQuality.md).

### Passing a record

``` r
rec <- list(decimalLatitude=42.1833,
            decimalLongitude=-1.8332,
            countryCode="ES",
            scientificName="Puma concolor")

parse_record(record=rec)
```

### Passing individual values as arguments

``` r
parse_record(decimalLatitude=42.1833,
             decimalLongitude=-1.8332,
             countryCode="ES",
             scientificName="Puma concolor")
```

### Structure of response

The response is a list of named elements, each element being the result of a single test. For more info on the meaning of these flags, please [check out the API documentation](https://github.com/VertNet/api-geospatial/blob/master/GeospatialQuality.md).

This is what any of the two calls above would return:

    ## Warning in gq_parse(req): Due to problems with third-party services, the
    ## API is not available at this moment. We apologize for any inconvenience.

    ## NULL

Get quality flags for sets of more than one record
--------------------------------------------------

Apart from assessing records one by one, the API also allows sending a set of records to evaluate them all with a single call, using the `add_flags` function. Records must be provided in the form of a `data.frame`. Just as before, each record should have the four key fields (`decimalLatitude`, `decimalLongitude`, `countryCode` and `scientificName`) to give a response as accurate as possible, although none is mandatory. This time, however, the function returns the provided `data.frame` with a new column, called `flags`, consisting of a list of all geospatial quality assessment results.

``` r
rec1 <- list(decimalLatitude=42.1833, decimalLongitude=-1.8332, countryCode="ES", scientificName="Puma concolor", ...)
rec2 <- list(...)
...
df <- rbind(rec1, rec2, ...)
df2 <- add_flags(df)
```

One easy way to directly get occurrences with the right format is to use the `occ_data` function in ROpenSci's `rgbif` package (<https://github.com/ropensci/rgbif>). There is a vignette (`rgbif-synergy`) illustrating how to integrate the two packages to improve the workflow of biodiversity data analysis.

Meta
----

-   Please [report any issues or bugs](https://github.com/ropenscilabs/rgeospatialquality/issues).
-   License: MIT
-   Get citation information doing `citation(package = 'rgeospatialquality')`
-   Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

[![ropensci\_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
