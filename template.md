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
