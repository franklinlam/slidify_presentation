---
title       : A Simple Shiny Application  
subtitle    : Outlier Effect in Simple Regression Model
author      : Franklin
job         : A Developing Data Product Student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : github   # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview

- This is a simple Shiny application developed for the 'Developing Data Product' course
- The application illustrates the effect of outlier 
  outlier on the regression model. 
- Users can use the sliders on the left panel to re-position the outlier and increase the number of outliers. 

--- .class #1 

## Run the application
1. Download the server.R and run.R from GitHub: https://github.com/franklinlam/developing_data_product.git, OR
2. Directly run the application on Rstudio: Type 'runGitHub("developing_data_product", "franklinlam")' 

<img align="center" height=400 src="assets/img/application-screen.png" />

--- .class #2 

## ui.R
UI R script that runs on the client side:

```r
shinyUI(fluidPage(
  titlePanel("Outlier Effect in Simple Regression Model"),
  sidebarLayout(
    sidebarPanel(
    strong("Outlier"),
    sliderInput(inputId='x', label='X-coordinate',value = 5, min = 1, max = 10, step = 0.5,),
    sliderInput(inputId='y', label='Y-coordinate',value = 5, min = 1, max = 10, step = 0.5,),
    sliderInput(inputId='no',label='Numbers',value = 1, min = 1, max = 5, step = 1,),
    strong("How to run the application"),
    p("1. Download and install RStudio"),
    p("2. Install the 'shiny' package on RStudio"),
    p("3. Download and save the server.R and ui.R in the current directory of RStudio"),
    p("4. Type 'runApp()' to run the application"),
    strong("Enjoy!")
  ),
  mainPanel(
    plotOutput('newRPlot'),
    p("Description: This is a simple Shiny application showing the effect of outlier 
      outlier on the regression model. You can use the sliders to re-position 
      the outlier and increase the number of outliers.")
  )
  )
))
```

--- .class #3
## server.R
Server R script that runs on the server side:


```r
library(UsingR)
data(galton)

shinyServer(
  function(input, output) {
    output$newRPlot <- renderPlot({
      pure_data <- data.frame(x=1:10, y=1:10)
      noise <- data.frame(x=input$x, y=input$y)
      noise_data <- pure_data
      for (i in 1:input$no) {noise_data <- rbind(noise_data, noise)}
      fit <- lm(y~x, data=noise_data)
      plot(y~x, data=pure_data, main="Regression Model")
      abline(fit, col="red", lwd=2)
      points(noise, col="blue", pch=19, cex = 1.1* input$no)
      text(2, 7, paste("Intercept = ", round(coef(fit)[1],digits=3)))
      text(2, 8, paste("Slope = ", round(coef(fit)[2],digits=3)))
    })
    
  }
)
```
