# Leaflet in R

I dig maps. R's [leaflet](https://rstudio.github.io/leaflet/) package makes it easy for even noobs to get their own maps up and running.

```r
library(ggplot2)
library(leaflet)
library(plyr)
library(readxl)
library(rgdal)

# Set your working directory to the folder containing the
# county shapefile then read it into the R environment. 
# It is available in the repo but can also be downloaded 
# at https://www.census.gov/geo/maps-data

counties <- readOGR(dsn = '.', layer = 'cb_2015_us_county_500k')

# You've heard the rumors, but Texas does actually fit within 
# the contiguous United States. Let's involve ourselves with 
# its counties. Note that '48' identifies Texas' counties in 
# the shapefile (see FIPS file)

texas <- subset(counties, counties$STATEFP %in% '48')

leaflet(texas) %>% 
  addProviderTiles('Stamen.TonerLite') %>% 
  addPolygons(color = '#002868', fillColor = '#BF0A30', 
    opacity = 1, fillOpacity = .75, popup = ~NAME)

```
A handsome map of Texas in a few lines of code:

![alt text](https://raw.githubusercontent.com/loeuf-quatre/leaflet-maps-in-R/master/graphics/texas-counties.png)

What if you wanna jazz it up with some demo data? I again relied on the [Census Bureau](http://www2.census.gov/prod2/statcomp/usac/excel/VST01.xls) for vital statistics. The metric of interest is on sheet five, column VST020207D (2007 live births per 1000) 

```r
birthRate <- read_excel('vital-statistics-by-county-2007-census.xls', 
  sheet = 5, col_names = T)
  
# Whittle the data frame
birthRate <- birthRate[, c('STCOU', 'VST020207D')]

# The birth rate data is aggregated by a concatenation of state 
# and county codes. The same columns exist in our shapefile, so 
# it's a simple matter of pasting them together then merging the 
# data sets
texas@data$STCOU <- paste0(texas@data$STATEFP, texas@data$COUNTYFP)
  
texas <- merge(texas, birthRate, by = 'STCOU')

leaflet(texas) %>% 
  addProviderTiles('Stamen.TonerLite') %>% 
  addPolygons(color = ~colorNumeric('RdYlGn', texas$VST020207D)(VST020207D), 
    stroke = F, fillOpacity = .9, popup = ~paste(NAME, VST020207D, sep = ' '), 
    group = 'Birth Rate') %>%
  addLegend('bottomright', pal = colorNumeric('RdYlGn', texas$VST020207D), 
    values = ~VST020207D, title = 'Birth Rate <br> (2007)', opacity = 1) %>% 
  addLayersControl(overlayGroups = c('Birth Rate'), 
    options = layersControlOptions(collapsed = F))
```

![alt text](https://raw.githubusercontent.com/loeuf-quatre/leaflet-maps-in-R/master/graphics/texas-counties-birth-rate.png)

Ahhhh so satisfying.
