---
title       : Developing Data Products Assignment
subtitle    : Major Cities in China
author      : Wenjing Liu
job         : November 22, 2018 (Updated on December 12, 2018)
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---



##  Top 100 Largest Cities by Population in China (plotly)

<iframe src="./p.html" width=100% height=100% allowtransparency="true"></iframe>

---
## Load data

Data Source: https://simplemaps.com/data/cn-cities


```r
cities <- read.csv("D:/R/data/cn_cities.csv", 
                   header=TRUE, 
                   #colClasses="character",
                   strip.white=FALSE,
                   stringsAsFactors=FALSE, 
                   na.strings=c("NA", "NaN", "N/A", "", "#VALUE!", "#DIV/0!"),
                   encoding='UTF-8')
dim(cities)
```

```
## [1] 2187    9
```

---

## Sort data by population size descending


```r
cities <- cities[order(-cities$population),]
head(cities)
```

```
##        city      lat      lng country iso2     admin capital population
## 1  Shanghai 31.22222 121.4581   China   CN  Shanghai   admin   14987000
## 2   Beijing 39.92882 116.3889   China   CN   Beijing primary   11106000
## 3 Guangzhou 23.11667 113.2500   China   CN Guangdong   admin    8829000
## 4  Shenzhen 22.53333 114.1333   China   CN Guangdong   minor    7581000
## 5     Wuhan 30.58333 114.2667   China   CN     Hubei   admin    7243000
## 6   Tianjin 39.14222 117.1767   China   CN   Tianjin   admin    7180000
##   population_proper
## 1          14608512
## 2           7480601
## 3           3152825
## 4           1002592
## 5           4184206
## 6           3766207
```

---

## Save dataset as RDS file, use it in a Shiny app

app.R (Part 1)


```r
library(shiny); library(leaflet)
cities <- readRDS("cn_cities.rds"); cities <- cities[1:100,]

ui <- fluidPage(
    titlePanel("Top 100 Largest Cities by Population in China"),
    sidebarLayout(
        sidebarPanel(
            strong("Display the largest cities ranked by population in China."), p(),
            em("E.g. Set the slider at value (1,10), then the top 10 largest cities will be displayed. The areas of the circles represent the relative sizes of the city populations."), p(),
            sliderInput("ui_range", 
                        label = "Range of ranks:",
                        min = 1, max = nrow(cities), value = c(1, nrow(cities)))),
    mainPanel(leafletOutput("ui_map"))))
```

---

app.R (Part 2)


```r
server <- function(input, output, session) {
    filteredData <- reactive({cities[input$ui_range[1]:input$ui_range[2],]
    }) 
    output$ui_map <- renderLeaflet({
        cities %>% leaflet() %>% addTiles() %>% 
            fitBounds(~min(lng), ~min(lat), ~max(lng), ~max(lat))
    })
    observe({
      leafletProxy("ui_map", data=filteredData()) %>%
        clearShapes() %>%
        addCircles(weight=1, color="#49b2b2", radius=~sqrt(population)*50)
    })
}

shinyApp(ui, server)
```

---

## Deploy the app on Shinyapps.io or Github

<br>

1. You can visit the app online at https://wenjing.shinyapps.io/app-cn-cities/

2. Or launch it in your RStudio by running the following code.


```r
library(shiny); runGitHub( "Shiny-CN_Cities", "Nov05")
```

<br>

# Thank you for reviewing the assignment!


