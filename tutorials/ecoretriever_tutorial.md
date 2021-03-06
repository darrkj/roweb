---
title: ecoretriever tutorial
layout: tutorial
packge_version: 0.1.0
---



<section id="installation">

## Installation

### Install EcoData Retriever

To use the R package `ecoretriever` you first need to install `Retriever`. Installers are available for all major operating systems from the [Download page](http://www.ecodataretriever.org/) or it can be installed from [source](https://github.com/weecology/retriever).

### Add Retriever to the path

The R package takes advantage of the EcoData Retriever's command line interface which must be enabled by adding it to the path on Mac platforms. On a Windows platform the Retriever should be added automatically to the path.

### Install R package

Install and load `ecoretriever` into the R session. Stable version from CRAN


```r
install.packages("ecoretriever")
```

Or development version from Github:


```r
install.packages("devtools")
devtools::install_github("ropensci/ecoretriever")
```


```r
library('ecoretriever')
```

<section id="usage">

## Usage

### List the datasets available via the Retriever


```r
ecoretriever::datasets()
```

### Install into csv

Install the Gentry dataset into csv files in your working directory


```r
ecoretriever::install(dataset = 'AvianBodySize', connection = 'csv', data_dir = "~/")
head(read.csv("~/AvianBodySize_species.csv")[,c(1:10)])
#>   record_id family species_number             species_name
#> 1         1      1              1         Struthio camelus
#> 2         2      3              7 Dromaius novaehollandiae
#> 3         3      4              8        Apteryx australis
#> 4         4      4              9           Apteryx owenii
#> 5         5      4             10          Apteryx haastii
#> 6         6      5             14            Tinamus major
#>          english_name subspecies m_mass m_mass_n f_mass f_mass_n
#> 1             Ostrich            115000       NA 100000       NA
#> 2                 Emu             32700       NA  38300       NA
#> 3          Brown Kiwi              2208       11   2535       26
#> 4 Little Spotted Kiwi              1135       51   1351       41
#> 5  Great Spotted Kiwi              2610       12   3270        7
#> 6       Great Tinamou              1059       NA    991       NA
```

### Read into R

Install and load a dataset as a list


```r
Gentry <- ecoretriever::fetch('Gentry')
```


```r
head(Gentry$sites)
#>   record_id site_id                                site_name country
#> 1         1       1                                Kitlope 1  Canada
#> 2         2       2                                Kitlope 2  Canada
#> 3         3       3                          Mt. St. Hilaire  Canada
#> 4         4       4           Metthieson Hammock County Park   U.S.A
#> 5         5       5                      San Felasko Hammock   U.S.A
#> 6         6       6 University of Florida Horticulture Woods   U.S.A
#>     state_province     continent abbreviation   lat     lon min_elev
#> 1 British Columbia North America     KITLOPE1 53.07 -127.83       20
#> 2 British Columbia North America     KITLOPE2 52.21 -127.79       10
#> 3         Montreal North America     MTSTHILA 45.62  -73.58       NA
#> 4          Florida North America     MATTHIES 25.72  -80.27       15
#> 5          Florida North America     SANFELAS 29.68  -82.43       30
#> 6          Florida North America     UFHORTIC 29.67  -82.33       50
#>   max_elev precip
#> 1       20   2100
#> 2       10   2100
#> 3       NA    750
#> 4       15   1375
#> 5       30   1375
#> 6       50   1375
```

### Write data into a database

Install a dataset into a SQLite database


```r
ecoretriever::install(dataset = 'Gentry', connection = 'sqlite', db_file = "gentrysqlite.sqlite3")
ecoretriever::install(dataset = 'MCDB', connection = 'sqlite', db_file = "mcdbsqlite.sqlite3")
```

Load `RSQLite` and connect to the database


```r
library("RSQLite")
db <- dbConnect(SQLite(), "mcdbsqlite.sqlite3")
```

List tables in the database


```r
dbListTables(db)
#> [1] "MCDB_communities" "MCDB_references"  "MCDB_sites"      
#> [4] "MCDB_species"     "MCDB_trapping"
```

List fields in the table


```r
dbListFields(db, "MCDB_species")
#> [1] "species_id"    "family"        "genus"         "species"      
#> [5] "species_level"
```

Query and get data


```r
dbGetQuery(db, "SELECT * FROM MCDB_species LIMIT 10")
#>    species_id      family     genus       species species_level
#> 1        ABBE Abrocomidae  Abrocoma     bennettii             1
#> 2        ABLO  Cricetidae Abrothrix    longipilis             1
#> 3        ABOL  Cricetidae Abrothrix     olivaceus             1
#> 4        ACSP     Muridae    Acomys spinosissimus             1
#> 5        ACWI     Muridae    Acomys       wilsoni             1
#> 6        ACPY Acrobatidae Acrobates      pygmaeus             1
#> 7        AECH     Muridae  Aethomys  chrysophilus             1
#> 8        AEHI     Muridae  Aethomys        hindei             1
#> 9        AKAZ  Cricetidae    Akodon        azarae             1
#> 10       AKCU  Cricetidae    Akodon        cursor             1
```

<section id="citing">

## Citing

To cite `ecoretriever` in publications use:

Software: 

> Daniel McGlinn and Ethan White (2014). ecoretriever: R Interface to the EcoData Retriever. R package version 0.1.0 https://github.com/ropensci/ecoretriever/

Publication:

> Morris, Benjamin D., and Ethan P. White. (2013). The EcoData Retriever: Improving Access to Existing Ecological Data. PLoS ONE 8 (6) (jun): 65848. doi:10.1371/journal.pone.0065848.

<section id="license_bugs">

## License and bugs

* License: [MIT](http://opensource.org/licenses/MIT)
* Report bugs at [our Github repo for ecoretriever](https://github.com/ropensci/ecoretriever/issues?state=open)

[Back to top](#top)
