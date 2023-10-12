reading data from web
================

Setup and load packages

Import data

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)
```

this imports every table stored as a list first() shortcut to get first
element in a list of things slice to remove first row with the Note info
we extracted table elements from the html file

``` r
marj_use_df <-
  nsduh_html |> 
  html_table() |> 
  first() |> 
  slice(-1)
```

Import Star Wars

``` r
swm_url = "https://www.imdb.com/list/ls070150896/"

swm_html = 
  read_html(swm_url)
```

first try to select the titles; Use Selector Gadget!

``` r
swm_title_vec = swm_html |> 
  html_elements(".lister-item-header a") |> 
  html_text()

swm_gross_rev_vec = swm_html |> 
  html_elements(".text-small:nth-child(7) span:nth-child(5)") |> 
  html_text()


swm_df = 
  tibble(
    title = swm_title_vec,
    swm_gross_rev_vec
  )
```

## APIs

sending request to API to get the data; good bc reproducible click on
API on website and click csv and copy link Get water data from NYC
content() strip out request info just want the data

``` r
nyc_water_df = GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") |> 
  content("parsed")
```

    ## Rows: 44 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

BRFSS data downloads 1000 out of 134000 rows; that’s all it allowed us
to download can adjust limit to get more of the rows found instructions
for getting more code from the website limits on the data size are not
uncommon

``` r
brfss_df = GET("https://data.cdc.gov/resource/acme-vg9e.csv",
               query = list("$limit" = 5000)) |> 
  content()
```

    ## Rows: 5000 Columns: 23
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

don’t just download export the csv from the website; better to use API
for reproducibility and easiest to run with updated datasets each time

Pokemon gives you weird data rectangle

``` r
poke_df = GET("https://pokeapi.co/api/v2/pokemon/ditto") |> 
  content()
```
